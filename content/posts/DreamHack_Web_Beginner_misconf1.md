---
title: "DreamHack Web Beginner_misconf1"
date: 2023-02-25
# weight: 1
# aliases: ["/DreamHack"]
tags: ["DreamHack", "Web", "Wargame", "Beginner", "misconf1"]
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

## 1️⃣ DreamHack Web Beginner_misconf1

1. 문제확인 (https://dreamhack.io/wargame/challenges/45)
2. 풀이

  

##### 📜**문제확인**

> 드림핵 사이트에서 문제 확인

![image-20240911084548017](/images/DreamHack_Web_Beginner_misconf1/image-20240911084548017.png)

> 서버 오픈 후 접속하면 아래와 그라파나 로그인 페이지 확인 가능

![image-20240911084648639](/images/DreamHack_Web_Beginner_misconf1/image-20240911084648639.png)

> 문제파일 다운후 default.ini 설정 파일 확인 가능

```python

# Google Analytics universal tracking code, only enabled if you specify an id here
google_analytics_ua_id =

# Google Tag Manager ID, only enabled if you specify an id here
google_tag_manager_id =

#################################### Security ############################
[security]
# disable creation of admin user on first start of grafana
disable_initial_admin_creation = false

# default admin user, created on startup
admin_user = admin

# default admin password, can be changed before first start of grafana, or in profile settings
admin_password = admin

# used for signing
secret_key = SW2YcwTIb9zpOOhoPsMm

# disable gravatar profile images
disable_gravatar = false
```

- 설정 파일 중간에 admin  계정과 패스워드 확인 가능
  

##### 📜**풀이**

> admin / admin 으로 로그인 후 Server Admin > Settings 에 들어가면 플래그 확인 가능

![image-20240911084906123](/images/DreamHack_Web_Beginner_misconf1/image-20240911084906123.png)

