---
title: "DreamHack Web Beginner_Pathtraversal"
date: 2023-01-29
# weight: 1
# aliases: ["/DreamHack"]
tags: ["DreamHack", "Web", "Wargame", "Beginner", "pathtraversal"]
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

## 1️⃣ DreamHack Web Beginner_Pathtraversal

1. 문제확인 (https://dreamhack.io/wargame/challenges/12)
2. 풀이

  

##### 📜**문제확인**

> 드림핵 사이트에서 문제 확인

![image-20240909091348588](/images/DreamHack_Web_Beginner_PathTraversal/image-20240909091348588.png)

> 서버 오픈 후 접속하면 아래와 같이 간단한 고객정보 페이지 확인

![image-20240909091359984](/images/DreamHack_Web_Beginner_PathTraversal/image-20240909091359984.png)

> 문제파일 다운후 코드 확인 가능

```python
#!/usr/bin/python3
from flask import Flask, request, render_template, abort
from functools import wraps
import requests
import os, json

users = {
    '0': {
        'userid': 'guest',
        'level': 1,
        'password': 'guest'
    },
    '1': {
        'userid': 'admin',
        'level': 9999,
        'password': 'admin'
    }
}

def internal_api(func):
    @wraps(func)
    def decorated_view(*args, **kwargs):
        if request.remote_addr == '127.0.0.1':
            return func(*args, **kwargs)
        else:
            abort(401)
    return decorated_view

app = Flask(__name__)
app.secret_key = os.urandom(32)
API_HOST = 'http://127.0.0.1:8000'

try:
    FLAG = open('./flag.txt', 'r').read() # Flag is here!!
except:
    FLAG = '[**FLAG**]'

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/get_info', methods=['GET', 'POST'])
def get_info():
    if request.method == 'GET':
        return render_template('get_info.html')
    elif request.method == 'POST':
        userid = request.form.get('userid', '')
        info = requests.get(f'{API_HOST}/api/user/{userid}').text
        return render_template('get_info.html', info=info)

@app.route('/api')
@internal_api
def api():
    return '/user/<uid>, /flag'

@app.route('/api/user/<uid>')
@internal_api
def get_flag(uid):
    try:
        info = users[uid]
    except:
        info = {}
    return json.dumps(info)

@app.route('/api/flag')
@internal_api
def flag():
    return FLAG

application = app # app.run(host='0.0.0.0', port=8000)
# Dockerfile
#     ENTRYPOINT ["uwsgi", "--socket", "0.0.0.0:8000", "--protocol=http", "--threads", "4", "--wsgi-file", "app.py"]
```

- 코드에서 "/get_info" 와 "/flag" 등 경로 정보 확인 가능
  

##### 📜**풀이**

> 코드에서 경로 확인 후 pathtraversal 취약점으로 확인 해봤지만 빈 데이터가 나옴

![image-20240909091417851](/images/DreamHack_Web_Beginner_PathTraversal/image-20240909091417851.png)

> 버프로 다시 확인한 결과 response 에서 flag 확인 가능함

![image-20240909091425512](/images/DreamHack_Web_Beginner_PathTraversal/image-20240909091425512.png)
