---
title: "File Upload 취약점"
date: 2023-07-10
# weight: 1
# aliases: ["/Pentest"]
tags: ["Web Security", "Penetration Testing", "File Upload Vuln"]
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

## 1️⃣ File Upload 취약점 정리



#### 📜**File Upload 취약점

\-     서버에 악성스크립트 업로드 하는것

\-     web server에 업로드 할수 있느냐, WAS에 업로드하느냐가 엄청큰 차이다.

\-     PHP기반웹사이트이면 – PHP 파일 업로드

\-     ASP – ASP 기반 파일 업로드…

\-     JSP – JSP 기반 파일업로드.. 처리기 매핑에서 매핑되는 것들이 모두된다.

\-     만약에 업로드후 열어봤을때 실행되지 않는다면???
       à 텍스트로 뿌려주거나, 바이너리 파일로 다운로드된다.

\-     컨설팅 과정

1. 파일업로드 가능여부 확인(메뉴확인)

2. 이미지 파일 업로드 후 경로 확인(허가된 파일) – Jpg,Gif…

3. 파일 삭제 테스트

4. 샘플 스크립트 파일 작성 및 업로드

5.  웹쉘 업로드

   

à 삭제하기 전에 반드시 알아야 하는것!! => 이미지가 올라갔을때 이미지의 절대경로를 반드시 알아야 한다. 만약에 알수가 없으면 웹쉘을 실행할 수 없기때문에 파일 업로드 취약점 사용 불가!!!



#### 📜**취약점 대응**

\- 업로드 기능 구현 안함 - 가장좋음
\- 웹루트 경로 밖에 저장하는 것
\- 업로드된 파일을 ftp, nfs등으로 타 서버로 전송
\- 업로드된 파일명 확장자 제거하여 저장
\- 저장되는 경로에 실행 퍼미션 제거 – 가장좋음

