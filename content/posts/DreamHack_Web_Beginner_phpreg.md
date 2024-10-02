---
title: "DreamHack Web Beginner_phpreg"
date: 2023-07-25
# weight: 1
# aliases: ["/DreamHack"]
tags: ["DreamHack", "Web", "Wargame", "Beginner", "phpreg"]
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

## 1️⃣ DreamHack Web Beginner_phpreg

1. 문제확인 (https://dreamhack.io/wargame/challenges/873)
2. 풀이

  

##### 📜**문제확인**

> 드림핵 사이트에서 문제 확인

![image-20241002084555641](/images/DreamHack_Web_Beginner_phpreg/image-20241002084555641.png)

> 서버 오픈 후 접속하면 아래와 같이 id / pw 입력하는 부분 확인가능

![image-20241002084623761](/images/DreamHack_Web_Beginner_phpreg/image-20241002084623761.png)

> 문제파일 다운후 코드 확인 가능

```php+HTML
<html>
<head>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap.min.css">
<title>PHPreg</title>
</head>
<body>
  <!-- Fixed navbar -->
  <nav class="navbar navbar-default navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">
        <a class="navbar-brand" href="/">PHPreg</a>
      </div>
      <div id="navbar">
        <ul class="nav navbar-nav">
          <li><a href="/">Step 1</a></li>
          <li><a href="/step2.php">Step 2</a></li>
        </ul>
      </div><!--/.nav-collapse -->
    </div>
  </nav><br/><br/><br/>
  <div class="container">
    <div class="box">
      <!-- PHP code -->
      <?php
          // POST request
          if ($_SERVER["REQUEST_METHOD"] == "POST") {
            $input_name = $_POST["input1"] ? $_POST["input1"] : "";
            $input_pw = $_POST["input2"] ? $_POST["input2"] : "";

            // pw filtering
            if (preg_match("/[a-zA-Z]/", $input_pw)) {
              echo "alphabet in the pw :(";
            }
            else{
              $name = preg_replace("/nyang/i", "", $input_name);
              $pw = preg_replace("/\d*\@\d{2,3}(31)+[^0-8\"]\!/", "d4y0r50ng", $input_pw);
              
              if ($name === "dnyang0310" && $pw === "d4y0r50ng+1+13") {
                echo '<h4>Step 2 : Almost done...</h4><div class="door_box"><div class="door_black"></div><div class="door"><div class="door_cir"></div></div></div>';

                $cmd = $_POST["cmd"] ? $_POST["cmd"] : "";

                if ($cmd === "") {
                  echo '
                        <p><form method="post" action="/step2.php">
                            <input type="hidden" name="input1" value="'.$input_name.'">
                            <input type="hidden" name="input2" value="'.$input_pw.'">
                            <input type="text" placeholder="Command" name="cmd">
                            <input type="submit" value="제출"><br/><br/>
                        </form></p>
                  ';
                }
                // cmd filtering
                else if (preg_match("/flag/i", $cmd)) {
                  echo "<pre>Error!</pre>";
                }
                else{
                  echo "<pre>--Output--\n";
                  system($cmd);
                  echo "</pre>";
                }
              }
              else{
                echo "Wrong nickname or pw";
              }
            }
          }
          // GET request
          else{
            echo "Not GET request";
          }
      ?>
    </div>
  </div>

  <style type="text/css">
    h4 {
      color: rgb(84, 84, 84);
    }
    .box{
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    pre {
      width: 80%;
    }
    .door_box {
      position: relative;
      width: 240px;
      height: 180px;
      margin: 20px 0px;
    }
    .door_black {
      position: absolute;
      width: 140px;
      height: 180px;
      background-color: black;
      border-radius: 10px;
      right:0px;
    }
    .door {
      z-index: 2;
      position: absolute;
      width: 140px;
      height: 180px;
      background-color: #b9abf7;
      border-radius: 10px;
      right: 100px;
    }
    .door_cir{
      z-index: 3;
      position: absolute;
      border-radius: 50%;
      width: 20px;
      height: 20px;
      border: 2px solid rgba(255, 222, 113, 0.873);
      background-color: #ffea98;
      top: calc( 180px / 2 - 10px );
      right: 10px;
    }
  </style>
</body>
</html>

```

* 코드중간에 php 코드로 id와 pw 값을 post 로 전달시 값을 바꾸는 preg_replace 함수가 있으며, 바꾼 최종 Id/pw는 "dnyang0310" / "d4y0r50ng+1+13" 이 값이 되어야 함

  

##### 📜**풀이** - Step1

> "name" 변수값이 "dnyang0310" 이 되도록 입력값 구하기

* $name = preg_replace("/nyang/i", "", $input_name);
  * nyang 를 찾아 공백으로 치환 하므로, "**dnynyangang0310**" 으로 입력 시 dnyang0310 만 남을 수 있음 




> "PW" 변수값이 "d4y0r50ng+1+13" 이 되도록 입력값 구하기

* $pw = preg_replace("/\d*\@\d{2,3}(31)+\[^0-8\]\!/", "d4y0r50ng", $input_pw);

  * 정규표현식에 만족되는 값을 "d4y0r50ng" 값으로 치환하고, PW가 "d4y0r50ng+1+13" 이 되어야 하므로 정규표현식 매칭값 + "+1+13" 을 붙여줌
  * 정규식을 확인해보면
    * \d* : 0-9까지 0개 이상을 나타냄
    * \@ : "@" 특수문자 매칭
    * \d{2,3}(31) : 0-9 까지 숫자 2개 또는 3개 후에 "31" 이라는 숫자가 오면됨
    * \[^0-8\] : 0-8 아닌 숫자 매칭 (9 매칭)
    * ! : "!" 특수문자 매칭

* "**1@22319!+1+13**" 으로 입력 시 "d4y0r50ng+1+13" 만 남을 수 있음

  

> 위의 정규표현식 대로 글자 작성 후 테스트 결과 step2로 이동 가능함

![image-20241002094532954](/images/DreamHack_Web_Beginner_phpreg/image-20241002094532954.png)

##### 📜**풀이** - Step2

> 위의 코드를 보면 CMD 값을 입력하는 코드가 있으며, 이 값을 "preg_match" 로 필터링하는 if문이 존재함

* else if (preg_match("/flag/i", $cmd)) 해당 if 문을 우회 해야 하며, 문제에서 flag는 ../dream/flag.txt 에 있다고 했기 때문에 "cat ../dream/*" 입력하여 flag를 획득함

![image-20241002095053899](/images/DreamHack_Web_Beginner_phpreg/image-20241002095053899.png)
