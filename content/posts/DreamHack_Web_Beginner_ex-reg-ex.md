---
title: "DreamHack Web Beginner_ex-reg-ex"
date: 2023-07-25
# weight: 1
# aliases: ["/DreamHack"]
tags: ["DreamHack", "Web", "Wargame", "Beginner", "ex-reg-ex"]
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

## 1️⃣ DreamHack Web Beginner_ex-reg-ex

1. 문제확인 (https://dreamhack.io/wargame/challenges/834)
2. 풀이

  

##### 📜**문제확인**

> 드림핵 사이트에서 문제 확인

![image-20241002083113067](/images/DreamHack_Web_Beginner_ex-reg-ex/image-20241002083113067.png)

> 서버 오픈 후 접속하면 아래와 같이 Input 값을 넣는 페이지 확인 가능

![image-20241002083132312](/images/DreamHack_Web_Beginner_ex-reg-ex/image-20241002083132312.png)

> 문제파일 다운후 코드 확인 가능

```python
#!/usr/bin/python3
from flask import Flask, request, render_template
import re

app = Flask(__name__)

try:
    FLAG = open("./flag.txt", "r").read()       # flag is here!
except:
    FLAG = "[**FLAG**]"

@app.route("/", methods = ["GET", "POST"])
def index():
    input_val = ""
    if request.method == "POST":
        input_val = request.form.get("input_val", "")
        m = re.match(r'dr\w{5,7}e\d+am@[a-z]{3,7}\.\w+', input_val)
        if m:
            return render_template("index.html", pre_txt=input_val, flag=FLAG)
    return render_template("index.html", pre_txt=input_val, flag='?')

app.run(host="0.0.0.0", port=8000)

```

* index 함수를 확인하면, 입력되는 값을 input_val 에 저장하여 특정 정규표현식과 맞는지 확인하는 re.match 함수가 보이며, 맞으면 flag 가 출력되는 구조임

  

##### 📜**풀이**

> 'dr\w{5,7}e\d+am@[a-z]{3,7}\\.\w+' 해당 정규표현식 확인 및 분석

* dr : 'dr' 글자 매칭
* \w{5,7} : [A-Za-z0-9] 가 5개 이상 7개 이하
* e : 'e' 글자 매칭
* \d+ : 숫자가 1개 이상 매칭
* am : 'am' 글자 매칭
* @ : '@' 특수문자 매칭
* [a-z]{3,7} : 소문자 알파벳이 3개이상 7개 이하
* \\. : '.' 으로 끝나는 문자 매칭
* \w+ : [A-Za-z0-9] 가 1개이상 매칭



> 위의 정규표현식 대로 글자 작성 후 테스트 결과 Flag 획득

drAAAAAe1am@aaa.com

![image-20241002084359192](/images/DreamHack_Web_Beginner_ex-reg-ex/image-20241002084359192.png)
