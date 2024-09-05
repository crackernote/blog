---
title: "DreamHack Web Beginner_Cookie"
date: 2023-01-14
# weight: 1
# aliases: ["/DreamHack"]
tags: ["DreamHack", "Web", "Wargame", "Beginner", "Cookie"]
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
---

## 1️⃣ DreamHack Web Beginner_Cookie

1. 문제확인 (https://dreamhack.io/wargame/challenges/6)
2. 풀dl

  

##### 📜**문제확인**

> 드림핵 사이트에서 문제 확인

![image-20240906073832905](../../../../AppData/Roaming/Typora/typora-user-images/image-20240906073832905.png)

> 서버 오픈 후 접속하면 아래와 같이 간단한 로그인 페이지 확인

![image-20240905103244101](C:\Users\kakaopaysec\AppData\Roaming\Typora\typora-user-images\image-20240905103244101.png)

> 문제파일 다운후 코드 확인 가능

```python
#!/usr/bin/python3
from flask import Flask, request, render_template, make_response, redirect, url_for

app = Flask(__name__)

try:
    FLAG = open('./flag.txt', 'r').read()
except:
    FLAG = '[**FLAG**]'

users = {
    'guest': 'guest',
    'admin': FLAG
}

@app.route('/')
def index():
    username = request.cookies.get('username', None)
    if username:
        return render_template('index.html', text=f'Hello {username}, {"flag is " + FLAG if username == "admin" else "you are not admin"}')
    return render_template('index.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'GET':
        return render_template('login.html')
    elif request.method == 'POST':
        username = request.form.get('username')
        password = request.form.get('password')
        try:
            pw = users[username]
        except:
            return '<script>alert("not found user");history.go(-1);</script>'
        if pw == password:
            resp = make_response(redirect(url_for('index')) )
            resp.set_cookie('username', username)
            return resp 
        return '<script>alert("wrong password");history.go(-1);</script>'

app.run(host='0.0.0.0', port=8000)

```

- 코드 윗부분에서 users가 'guest', 'admin' 두가지가 있음을 확인
- index 함수에서 'username' 쿠키 값을 저장하는 것을 확인 가능하며, 로그인시 그 값을 사용하는 것을 확인



##### 📜**풀이**

> guest로 로그인 후 쿠키값을 확인 

![image-20240905104758629](C:\Users\kakaopaysec\AppData\Roaming\Typora\typora-user-images\image-20240905104758629.png)

> 쿠키값을 admin으로 변경 후 새로고침을 하게되면 플래그를 획득 가능

![image-20240905104854104](C:\Users\kakaopaysec\AppData\Roaming\Typora\typora-user-images\image-20240905104854104.png)
