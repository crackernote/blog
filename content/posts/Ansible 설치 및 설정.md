---
title: "Ansible 설치 및 설정하기"
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

## 1. Ansible 설치



### 📜CentOS

```python
$ yum install -y ansible 
```



### 📜Ubuntu

```python
$ apt install ansible
```



### 📜macOS

```python
$ brew install ansible
```



## 2. SSH 및 hosts 파일 설정

앤서블은 ssh로 제어 노드와 매니지드 노드가 연결됩니다. 앤서블을 이용하여 작업을 진행하기 전에 `authorized_keys`에 키를 추가하여 주는 것이 좋습니다. `ssh-copy-id` 명령을 이용하여 간편하게 키를 설정할 수 있습니다.



### 📜ssh key 설정

```python
# Ansible Control Node 에서 실행
# ssh 키 생성 
$ ssh-keygen

# ssh 키 복사 > Managed Node 로 연결하기 위함
# ssh 연결을 처리할 계정으로 연결 
$ ssh-copy-id user@test-host.com
```



### 📜hosts 설정 - Ansible Control Node 에서 실행

```python
# /etc/ansible/hosts 파일 열기
$ vi /etc/ansible/hosts

# 맨 밑에줄에 Managed Node 추가해주기 
[test_server]
127.0.0.1      >> "Managed Node IP로 추가"
```



## 3. Ansible 실행 테스트

Ansible과 ssh 설정이 제대로 되었는지  `ping` 으로 동작을 확인합니다.



### 📜Managed Node 를 대상으로 ping 동작 실행

```python
# managed node에 ping 동작 실행
$ ansible test_server -m ping
$ ansible -u kali test_server -m ping  >> "root 계정이 다른 계정으로 되어있을 경우 계정입력"

# ping 결과
"Managed Node IP" | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
} 
```



## 4. Ansible Playbook 

Ansible playbook 을 작성하여 제대로 작동하는지 테스트해봅니다. 



### 📜PlayBook 작성 (OS Name을 추출하는 playbook)

```python
---
  - name: Playbook
    hosts: ubuntu_server
    remote_user: flus
    become: yes
    become_user: root
    tasks:
      - name: Check os Name  
        shell: cat /etc/os-release  
        register: os_name  

      - name: print
        debug:
         msg: "{{ os_name.stdout }}"
```



### 📜Managed Node 를 대상으로 PlayBook 실행

```python
# managed node에 playbook 동작 실행
$ ansible-playbook first_playbook.yml


# 정상적으로 playbook 결과 출력
PLAY [Playbook] *****************************************************************************************

TASK [Gathering Facts] *****************************************************************************************
[DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host 192.168.x.x should use /usr/bin/python3, but is using 
/usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible release will default to using the
 discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information. This feature 
will be removed in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in 
ansible.cfg.
ok: [192.168.x.x]

TASK [Check os Name] *****************************************************************************************
changed: [192.168.x.x]

TASK [print] *****************************************************************************************
ok: [192.168.x.x] => {
    "msg": "NAME=\"Ubuntu\"\nVERSION=\"18.04.6 LTS (Bionic Beaver)\"\nID=ubuntu\nID_LIKE=debian\nPRETTY_NAME=\"Ubuntu 18.04.6 LTS\"\nVERSION_ID=\"18.04\"\nHOME_URL=\"https://www.ubuntu.com/\"\nSUPPORT_URL=\"https://help.ubuntu.com/\"\nBUG_REPORT_URL=\"https://bugs.launchpad.net/ubuntu/\"\nPRIVACY_POLICY_URL=\"https://www.ubuntu.com/legal/terms-and-policies/privacy-policy\"\nVERSION_CODENAME=bionic\nUBUNTU_CODENAME=bionic"
}

PLAY RECAP *****************************************************************************************
192.168.x.x : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

