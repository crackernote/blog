---
title: "DreamHack Web Beginner_php7cmp4re"
date: 2023-09-25
# weight: 1
# aliases: ["/DreamHack"]
tags: ["DreamHack", "Web", "Wargame", "Beginner", "php7cmp4re"]
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

## 1️⃣ DreamHack Web Beginner_php7cmp4re

1. 문제확인 (https://dreamhack.io/wargame/challenges/1113)
2. 풀이

  

##### 📜**문제확인**

> 드림핵 사이트에서 문제 확인

![image-20241002095313912](/images/DreamHack_Web_Beginner_php7cmp4re/image-20241002095313912.png)

> 서버 오픈 후 접속하면 아래와 같이 id / pw 입력하는 부분 확인가능

![image-20241002095549460](/images/DreamHack_Web_Beginner_php7cmp4re/image-20241002095549460.png)

> 문제파일 다운후 코드 확인 가능

```php+HTML
<html>
<head>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap.min.css">
<title>php7cmp4re</title>
</head>
<body>
    <!-- Fixed navbar -->
    <nav class="navbar navbar-default navbar-fixed-top">
      <div class="container">
        <div class="navbar-header">
          <a class="navbar-brand" href="/">php7cmp4re</a>
        </div>
        <div id="navbar">
          <ul class="nav navbar-nav">
            <li><a href="/">Index page</a></li>
          </ul>
        </div><!--/.nav-collapse -->
      </div>
    </nav>
    <div class="container">
    <?php
    require_once('flag.php');
    error_reporting(0);
    // POST request
    if ($_SERVER["REQUEST_METHOD"] == "POST") {
      $input_1 = $_POST["input1"] ? $_POST["input1"] : "";
      $input_2 = $_POST["input2"] ? $_POST["input2"] : "";
      sleep(1);

      if($input_1 != "" && $input_2 != ""){
        if(strlen($input_1) < 4){
          if($input_1 < "8" && $input_1 < "7.A" && $input_1 > "7.9"){
            if(strlen($input_2) < 3 && strlen($input_2) > 1){
              if($input_2 < 74 && $input_2 > "74"){
                echo "</br></br></br><pre>FLAG\n";
                echo $flag;
                echo "</pre>";
              } else echo "<br><br><br><h4>Good try.</h4>";
            } else echo "<br><br><br><h4>Good try.</h4><br>";
          } else echo "<br><br><br><h4>Try again.</h4><br>";
        } else echo "<br><br><br><h4>Try again.</h4><br>";
      } else{
        echo '<br><br><br><h4>Fill the input box.</h4>';
      }
    } else echo "<br><br><br><h3>WHat??!</h3>";
    ?> 
    </div> 
</body>
</html>
```

* 코드중간에 php 코드로 input1, input2 값을 post 로 전달시 if 문 조건들이 있음

  

##### 📜**풀이** 

> 코드에서 input1, input2 에 대한 if 조건 충족 시켜야 함

* input_1의 문자 길이는 4글자 미만이고 7.9랑 7.A 숫자 사이라서 7.9를 십진수로 바꿔주면 554657~554665이니까 그 사이 숫자인 554658을 아스키코드로 바꿔준다. 그러면 7.:가 나온다.
  
  input_2의 문자 길이는 2글자이고 74초과 문자열 74미만으로 십진수로 바꿔주면 74~5552이고 사이의 숫자를 아스키코드로 바꿔주면 :;가 나온다. 

![image-20241002100832444](/images/DreamHack_Web_Beginner_php7cmp4re/image-20241002100832444.png)

![image-20241002100838294](/images/DreamHack_Web_Beginner_php7cmp4re/image-20241002100838294.png)
