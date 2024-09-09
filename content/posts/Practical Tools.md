---
title: "Practical Tools"
date: 2023-09-20
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

## 1️⃣ Practical Tools



#### 📜Netcat

tcp , udp protocol 사용

- Connecting tcp/udp port
  - nc -nv 10.11.0.22 4444

- Listening on a TCP/UDP Port
  - nc -nlvp 4444

- Transferring Files w/ Netcat
  - nc -nlvp 4444 > incoming.exe
  - nc -nv 10.11.0.22 4444 < /usr/share/windows-resources/binaries/wget.exe




#### 📜Socat

- Connecting

  - socat - TCP4:<remote server's ip address>:80
  
- Listening

  - sudo socat TCP4-LISTEN:443 STDOUT

- File Transfers

  - sudo socat TCP4-LISTEN:443,fork file:secret_passwords.txt

    


#### 📜Powershell and powercat



#### 📜Wireshark



#### 📜Tcpdump

