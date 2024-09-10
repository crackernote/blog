---
title: "DreamHack Web Beginner_Command_Injection1"
date: 2023-02-25
# weight: 1
# aliases: ["/DreamHack"]
tags: ["DreamHack", "Web", "Wargame", "Beginner", "Command Injection"]
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

1. 문제확인 (https://dreamhack.io/wargame/challenges/44)
2. 풀이

  

##### 📜**문제확인**

> 드림핵 사이트에서 문제 확인

![image-20240911082034800](/images/DreamHack_Web_Beginner_Command_Injection1/image-20240911082034800.png)

> 서버 오픈 후 접속하면 아래와 같이 Ping을 할 수 있는 사이트 확인

![image-20240911082135641](/images/DreamHack_Web_Beginner_Command_Injection1/image-20240911082135641.png)

> 문제파일 다운후 코드 확인 가능

```python
#!/usr/bin/env python3
import subprocess

from flask import Flask, request, render_template, redirect

from flag import FLAG

APP = Flask(__name__)


@APP.route('/')
def index():
    return render_template('index.html')


@APP.route('/ping', methods=['GET', 'POST'])
def ping():
    if request.method == 'POST':
        host = request.form.get('host')
        cmd = f'ping -c 3 "{host}"'
        try:
            output = subprocess.check_output(['/bin/sh', '-c', cmd], timeout=5)
            return render_template('ping_result.html', data=output.decode('utf-8'))
        except subprocess.TimeoutExpired:
            return render_template('ping_result.html', data='Timeout !')
        except subprocess.CalledProcessError:
            return render_template('ping_result.html', data=f'an error occurred while executing the command. -> {cmd}')

    return render_template('ping.html')


if __name__ == '__main__':
    APP.run(host='0.0.0.0', port=8000)

```

- 코드에서 ping() 함수에서 cmd 에 입력되는 값들을 확인 할 수 있음
  

##### 📜**풀이**

> 위의 코드에서 cmd 변수 처럼 " 8888"; ls" " 값을 입력 했지만 요청한 형식과 일치시키라는 문구 확인함

![image-20240911082918165](/images/DreamHack_Web_Beginner_Command_Injection1/image-20240911082918165.png)

> 개발자 도구를 통해 host 입력란을 확인해보니 "Pattern=[A-Za-z0-9.]{5,20}" 을 확인할 수 있음. 이 뜻은 대문자, 소문자, 점(.) 을 이용한 5~20글자만 허용한다는 의미

![image-20240911083117202](/images/DreamHack_Web_Beginner_Command_Injection1/image-20240911083117202.png)

> 개발자 도구에서 해당 내용을 지우고 다시 실행해 보니 정상 동작 하는 것을 확인

![image-20240911083535011](/images/DreamHack_Web_Beginner_Command_Injection1/image-20240911083535011.png)

> 같은 방식으로 이번엔 flag.py 를 출력

![image-20240911083638908](/images/DreamHack_Web_Beginner_Command_Injection1/image-20240911083638908.png)

![image-20240911083644369](/images/DreamHack_Web_Beginner_Command_Injection1/image-20240911083644369.png)

