---
title: "linux 필수 command"
date: 2023-08-20
# weight: 1
# aliases: ["/OSCP"]
tags: ["OSCP", "Penetration Testing", "Offensive Security"]
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

## 1️⃣ linux 필수 Command



#### 📜Man Pages

- man



#### 📜apropos

- apropos

   

#### 📜Listing Files

- ls -al

  

#### 📜Moving Around

- cd

- pwd

  

#### 📜Creating Directories

- mkdir

   - mkdir module one

   - cd module\ one/

   - mkdir -p

     

#### 📜Finding Files

- echo

- which

- locate

- find

  

#### 📜Managing Kali Linux Services

 - systemctl

 - SSH Service

  - sudo systemctl start ssh (ssh 시작)

  - sudo ss -antlp | grep sshd (ssh 구동 확인)

  - sudo systemctl enable ssh (부팅시 ssh 실행)

 - HTTP service

  - sudo systemctl start apache2

  - sudo ss -antlp | grep apache

  - sudo systemctl enable apache2

 - To see a table of all available servies

   - systemctl list-unit-files

     

#### 📜Searching, Installing, Removing Tools

 - apt update > apt 전체 업데이트

 - apt upgrade > 특정 패키지 업그레이드

 - apt-cache search

 - apt show

 - apt remove --purge

 - dpkg > .deb  (패키지 설치, 삭제, 정보제공을 위해 사용되는 명령어)

   

#### 📜Environment Variables

- echo

- export

- env

- history

  

#### 📜Redirection to a New File

- echo "test" > Redirection_test.txt

  

#### 📜Redirection to an Existing File

- echo "test" >> Redirection_test.txt

  

#### 📜Redirecting from a File

- wc
  - wc -m < redirection_test.txt



#### 📜Redirecting STDERR

- ls ./test 2>error.txt

  

#### 📜Piping

- cat error.txt
- cat error.txt | wc -m
- cat error.txt | wc -m > count.txt
- cat count.txt



#### 📜Text Searching and Manipulation

- grep
  (ls -la /usr/bin | grep zip)
- sed
- cut
  (cut -d ":" -f 1 /etc/passwd)
- awk
  (echo "hello::there::friend" | awk -F "::" '{print $1, $3}')
  (=> hello friend)



#### 📜Editing Files from the Command Line

- nano
- vi



#### 📜Comparing Files

- comm

- diff

- vimdiff

  

#### 📜managing process

- bg
- jobs
- fg
- ps -ef
- ps -fc
- kill



#### 📜File and Commanding Monitoring

- tail

- watch

  

#### 📜Downloading Files

- wget
- curl
- axel



#### 📜Bash History Customization

- histcontrol
- histingnore
- histtimeformat



#### 📜Alias

- alias

- lsa

- unalias

  

#### 📜Persistent bash customization

- bashrc

  

#### 📜Others

- gunzip
- head
- sort 
- uniq

