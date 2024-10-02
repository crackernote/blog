---
title: "DreamHack Web Beginner_session"
date: 2023-02-25
# weight: 1
# aliases: ["/DreamHack"]
tags: ["DreamHack", "Web", "Wargame", "Beginner", "session"]
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

## 1️⃣ DreamHack Web Beginner_session

1. 문제확인 (https://dreamhack.io/wargame/challenges/266)
2. 풀이

  

##### 📜**문제확인**

> 드림핵 사이트에서 문제 확인

![image-20240912124244109](/images/DreamHack_Web_Beginner_session/image-20240912124244109.png)

> 서버 오픈 후 접속하면 아래와 같이 로그인 페이지를 확인 할 수 있음

![image-20240912124323070](/images/DreamHack_Web_Beginner_session/image-20240912124323070.png)

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
    'user': 'user1234',
    'admin': FLAG
}

session_storage = {
}

@app.route('/')
def index():
    session_id = request.cookies.get('sessionid', None)
    try:
        username = session_storage[session_id]
    except KeyError:
        return render_template('index.html')

    return render_template('index.html', text=f'Hello {username}, {"flag is " + FLAG if username == "admin" else "you are not admin"}')

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
            session_id = os.urandom(4).hex()
            session_storage[session_id] = username
            resp.set_cookie('sessionid', session_id)
            return resp 
        return '<script>alert("wrong password");history.go(-1);</script>'

if __name__ == '__main__':
    import os
    session_storage[os.urandom(1).hex()] = 'admin'
    print(session_storage)
    app.run(host='0.0.0.0', port=8000)

```

- Users 라는 변수에 아이디 : 패스워드로 보이는 값 확인 가능
- /login 페이지의 코드를 확인하면, sessionid는 임의의 4바이트 수로 결정된다. 16진수로 8자리로 표현됨
  ![image-20240926102456025](/images/DreamHack_Web_Beginner_session/image-20240926102456025.png)
  ![image-20240926102504460](/images/DreamHack_Web_Beginner_session/image-20240926102504460.png)
- main 코드에서 admin의 sessionid는 1바이트 수로 결정되는데, 즉 16진수 2자리로 표현되는 것을 추측함
  

##### 📜**풀이**

> admin의 sessionid가 16진수 2자리 라는것을 예측하고, Burp에서 BruteForce를 진행하여 2자리 sessionid 를 확보할 수 있음

![image-20240926110334618](/images/DreamHack_Web_Beginner_session/image-20240926110334618.png)

![image-20240926110412404](/images/DreamHack_Web_Beginner_session/image-20240926110412404.png)

![image-20240926110432292](/images/DreamHack_Web_Beginner_session/image-20240926110432292.png)
![image-20240926110445942](/images/DreamHack_Web_Beginner_session/image-20240926110445942.png)
