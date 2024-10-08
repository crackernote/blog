---
title: "Ansible을 이용한 스크립트 실행방법"
date: 2023-11-20
# weight: 1
# aliases: ["/취약점분석 자동화"]
tags: ["취약점 분석", "Automation", "Ansible"]
author: "CrackerNote"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
disableHLJS: false # to disable highlightjs
disableShare: true
hideSummary: false
searchHidden: true
ShowReadingTime: false
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: false
ShowRssButtonInSectionTermList: true
UseHugoToc: false
searchHidden: true
typora-root-url: ../
---

## 1. 스크립트 실행을 위한 playbook 작성

Master Node에 있는 스크립트를 Control Node로 보내고 실행시켜, 실행값을 Master, Control Node 모두 csv 파일로 저장시킨다. 이렇게 여러 서버에 스크립트를 실행하여 Master Node에 결과값을 수집하여 관리가 가능하다.



### 📜PlayBook 작성

```python
#excute command example
#ansible-playbook -i [inventory_file] --extra-vars "excute_group=[list] excute_date=[YYYYMMDD]" [playbook_file]
---
- hosts: ubuntu_server  #실행 대상 목록
  gather_facts: no #대상 서버 정보 수집 여부
  connection: ssh #접근 프로토콜
  remote_user: ubuntu #대상 서버 접근 계정
  become: yes #sudo 권한 사용
  vars:
    src_path: /home/ansible/Desktop/script
    dst_path: /tmp/diagnosis/infra_script
    run_script: ubuntu.sh
    result_path: /home/flus
    result_down_path: /root/infra_result/linux/{{ excute_date }}
  
  tasks:
    - name: Check Script Dir - {{ dst_path }}
      stat: path={{dst_path}}
      register: check_dir

    - name: Make Script Dir - /tmp/diagnosis
      shell: "mkdir -m 755 -p /tmp/diagnosis"
      when: not check_dir.stat.exists

    - name: Make Script Dir - {{ dst_path }}
      shell: "mkdir -m 755 -p {{dst_path}}"
      when: not check_dir.stat.exists

    - name: Deploy Main Script
      copy: src={{src_path}}/{{ run_script }} dest={{dst_path}}/{{ run_script }} mode=755 owner=root force=yes

    - name: Run Script
      shell: "bash {{dst_path}}/{{ run_script }}"

    - name: Check Hostname
      shell: echo "`hostname`"
      register: hostname_result

    - name: Result File Name
      shell: "ls | egrep L-LINUX-"
      register: name_result

    - name: Result File Stat
      stat:
        path: "{{ result_path }}/{{ name_result.stdout }}"
      register: stat_result

    - name: Download Result File
      fetch:
        src: "{{ result_path }}/{{ name_result.stdout }}"
        dest: "{{ result_down_path }}/"
        flat: yes
      when: stat_result.stat.exists

    - name: Delete Script & Result
      file:
        path: "{{item}}"
        state: absent
      with_items:
        - "/tmp/diagnosis"
        - "{{ result_path }}/{{ name_result.stdout }}"

    - name: Check Result File
      debug:
        msg: "[CHECK_RESULT] [ OK ] {{ hostname_result.stdout }} : {{ result_down_path }}/{{ name_result.stdout }}"
      when: stat_result.stat.exists
```



## 2. Playbook 실행



### 📜 Ansible Control Node 에서 실행 결과

```python
[root@localhost Desktop]# ansible-playbook --extra-vars " excute_date=20230405" linux_security_check.yml

PLAY [ubuntu_server] *****************************************************************************************

TASK [Check Script Dir - /tmp/diagnosis/infra_script] *********************************************************************
[DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host 192.168.x.x should use /usr/bin/python3, but is using 
/usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible release will default to using the
 discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information. This feature 
will be removed in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in 
ansible.cfg.
ok: [192.168.x.x]

TASK [Make Script Dir - /tmp/diagnosis] ***********************************************************************************
[WARNING]: Consider using the file module with state=directory rather than running 'mkdir'.  If you need to use command
because file is insufficient you can add 'warn: false' to this command task or set 'command_warnings=False' in ansible.cfg
to get rid of this message.
changed: [192.168.x.x]

TASK [Make Script Dir - /tmp/diagnosis/infra_script] **********************************************************************
changed: [192.168.x.x]

TASK [Deploy Main Script] *****************************************************************************************
changed: [192.168.x.x]

TASK [Run Script] *****************************************************************************************
changed: [192.168.x.x]

TASK [Check Hostname] *****************************************************************************************
changed: [192.168.x.x]

TASK [Result File Name] *****************************************************************************************
changed: [192.168.x.x]

TASK [Result File Stat] *****************************************************************************************
ok: [192.168.x.x]

TASK [Download Result File] *****************************************************************************************
changed: [192.168.x.x]

TASK [Delete Script & Result] *****************************************************************************************
changed: [192.168.x.x] => (item=/tmp/diagnosis)
changed: [192.168.x.x] => (item=/home/flus/L-LINUX-ubuntu-result-2023-04-05.csv)

TASK [Check Result File] *****************************************************************************************
ok: [192.168.x.x] => {
    "msg": "[CHECK_RESULT] [ OK ] ubuntu : /root/infra_result/linux/20230405/L-LINUX-ubuntu-result-2023-04-05.csv"
}

PLAY RECAP *****************************************************************************************
192.168.x.x            : ok=11   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

