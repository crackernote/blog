---
title: "DreamHack Web Beginner_FileDownload1"
date: 2023-02-14
# weight: 1
# aliases: ["/DreamHack"]
tags: ["DreamHack", "Web", "Wargame", "Beginner", "File Download1"]
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

## 1️⃣ DreamHack Web Beginner_File Download 1

1. 문제확인 (https://dreamhack.io/wargame/challenges/37)
2. 풀이

  

##### 📜**문제확인**

> 드림핵 사이트에서 문제 확인

![image-20240905142046901](C:\Users\kakaopaysec\AppData\Roaming\Typora\typora-user-images\image-20240905142046901.png)

> 서버 오픈 후 접속하면 아래와 같이 메모를 업로드 할 수 있는 웹서비스 확인

![image-20240905142158681](C:\Users\kakaopaysec\AppData\Roaming\Typora\typora-user-images\image-20240905142158681.png)

> 문제파일 다운후 코드 확인 가능

```python
#!/usr/bin/env python3
import os
import shutil

from flask import Flask, request, render_template, redirect

from flag import FLAG

APP = Flask(__name__)

UPLOAD_DIR = 'uploads'


@APP.route('/')
def index():
    files = os.listdir(UPLOAD_DIR)
    return render_template('index.html', files=files)


@APP.route('/upload', methods=['GET', 'POST'])
def upload_memo():
    if request.method == 'POST':
        filename = request.form.get('filename')
        content = request.form.get('content').encode('utf-8')

        if filename.find('..') != -1:
            return render_template('upload_result.html', data='bad characters,,')

        with open(f'{UPLOAD_DIR}/{filename}', 'wb') as f:
            f.write(content)

        return redirect('/')

    return render_template('upload.html')


@APP.route('/read')
def read_memo():
    error = False
    data = b''

    filename = request.args.get('name', '')

    try:
        with open(f'{UPLOAD_DIR}/{filename}', 'rb') as f:
            data = f.read()
    except (IsADirectoryError, FileNotFoundError):
        error = True


    return render_template('read.html',
                           filename=filename,
                           content=data.decode('utf-8'),
                           error=error)


if __name__ == '__main__':
    if os.path.exists(UPLOAD_DIR):
        shutil.rmtree(UPLOAD_DIR)

    os.mkdir(UPLOAD_DIR)

    APP.run(host='0.0.0.0', port=8000)

```

- 코드에서 "{UPLOAD_DIR}/{filename}" 에 파일 업로드 경로와 파일 읽는 경로가 같은 것을 확인
  

##### 📜**풀이**

> 업로드 된 경로에서 flag.py 파일을 읽음

![image-20240905142521463](C:\Users\kakaopaysec\AppData\Roaming\Typora\typora-user-images\image-20240905142521463.png)
