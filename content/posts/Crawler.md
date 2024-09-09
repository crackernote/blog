---
title: "crawler"
date: 2023-10-20
# weight: 1
# aliases: ["/크롤링"]
tags: ["Crawler", "Automation", "Coding"]
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

## 1️⃣ Crawler

Python 을 이용한 Crawler

### 📜crawler를 위해 필요한 내용들

- requests 
  - HTTP 통신을 위한 Python 라이브러리

- beautifulsoup 
  - HTML 분석을 위한 Python 라이브러리
  
- pyautogui
  
- CSS 선택자

  - 크롤링할 HTML 태그를 선택할때 사용함

  - 태그 선택자  **(h1, a 등 태그 이름으로 선택)**

  - id 선택자 **(#을 앞에 붙인 후 id 값으로 선택)**

    ```html
    HTML
    <div id="articleBody">
     본문--
    </div>
    
    선택자
    #airticleBody
    ```

  - class 선택자 **(.을 앞에 붙인 후 class 값으로 선택)**

    ```html
    HTML
    <div class="info_group">
     뉴스목록
    </div>
    
    선택자
    .airticleBody
    ```

  - 자식 선택자 **(바로 아래에 있는 태그를 선택한다)**

    ```html
    HTML
    <div class="logo_sports">
        <span>스포츠</span>
    </div>
    
    선택자
    .logo_sports > span
    ```

