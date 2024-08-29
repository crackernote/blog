---
layout: single
title: "Practical Tools"
categories: OSCP
tag: [Offensive Security, Penetration Testing, OSCP]
toc: true
toc_sticky: true
toc_label: 목차
author_profile: false
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

