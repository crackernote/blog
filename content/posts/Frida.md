---
title: "Frida Basic"
date: 2023-07-10
# weight: 1
# aliases: ["/Mobile Security"]
tags: ["Mobile Security", "Penetration Testing", "Frida"]
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

## 1️⃣ Frida 기본



#### 📜**Frida?**

- Ole가 개발한 DBI(Dynamic Binary Instrumention) 프레임 워크
  (* Instrumentation : 앱이 실행중인 상태에서 코드 명령어를 삽입해 프로세스를 추적, 분석, 디버깅하는 도구)



#### 📜**Frida 특징**

- 다양한 플랫폼에서 프로세스에 대한 인젝션이 가능해 큰 확장성을 가짐

- 윈도우, 맥OS, GNU/Linux, iOS, Android 및 QNX에서 자바스크립트를 네이티브 앱에 삽입가능

  

#### 📜**Frida 주요기능**

- **함수 후킹 (특정 함수에 연결하여 반환 값 변경, 함수 재작성 등)**
- 애플리케이션 디버깅 가능
- **힙 메모리 내 객체 인스턴스 검색 및 사용**
- 실시간 트래픽 스니핑 또는 암호 해독
- **탈옥 또는 루팅되지 않은 단말기에서도 가능**



#### 📜**동작방식

![image-20240909092630840](/images/Frida/image-20240909092630840.png)



#### 📜**Frida Tools**

- frida (frida -h)

  - Frida CLI인 REPL 인터페이스로, 신속한 프로토타이핑과 손쉬운 디버깅이 목표인 도구

  - "frida -h" 명령어로 옵션 확인가능

    | **옵션**                 | **설명**                             |
    | ------------------------ | ------------------------------------ |
    | --version                | 프리다 프로그램 버전 출력            |
    | -h, --help               | 도움말 메시지 출력                   |
    | -D ID, --device=ID       | 주어진 ID로 장치에 연결              |
    | -U, --usb                | USB 장치에 연결                      |
    | -R, --remote             | 원격 프리다 서버에 연결              |
    | -H Host, --host=Host     | Host의 원격 프리다리다 서버에 연결   |
    | -a, --application        | 애플리케이션 리스트만 출력           |
    | -i, --installed          | 설치된 모든 애플리케이션 포함 출력   |
    | -l SCRIPT, --load=SCRIPT | SCRIPT를 로드                        |
    | -f FILE, --file=FILE     | spawn FILE                           |
    | --no-pause               | 시작할 때 자동으로 메인쓰레드를 시작 |

  

- frida-ps (frida-ps -h)

  - Frida에 연결된 프로세스 목록을 출력하기 위한 도구

  - "frida-ps -h" 명령어로 옵션 확인가능

    | **옵션**             | **설명**                           |
    | -------------------- | ---------------------------------- |
    | --version            | Frida 프로그램 버전 출력           |
    | -a, --application    | 애플리케이션 리스트만 출력         |
    | -D ID, --device=ID   | 주어진 ID로 장치에 연결            |
    | -H HOST, --host=HOST | HOST의 원격 프리다 서버에 연결     |
    | -h, --help           | 도움말 메시지 출력                 |
    | -i, --installed      | 설치된 모든 애플리케이션 포함 출력 |
    | -R, --remote         | 원격 Frida 서버에 연결             |
    | -U, --usb            | USB 장치에 연결                    |

    

- frida-ls-devices (frida-ps -h)

  - 연결된 디바이스를 출력하는 도구

- frida-trace (frida-trace -h)

  - 함수 호출을 동적으로 추적하기 위한 도구

  - "frida-trace -h" 명령어로 옵션 확인가능

    | **옵션**                               | **설명**                       |
    | -------------------------------------- | ------------------------------ |
    | --version                              | Frida 프로그램 버전 출력       |
    | -h, --help                             | 도움말 메시지 출력             |
    | -D ID, --device=ID                     | 주어진 ID로 장치에 연결        |
    | -U, --usb                              | USB 장치에 연결                |
    | -R, --remote                           | 원격 Frida 서버에 연결         |
    | -H HOST, --host=HOST                   | HOST의 원격 프리다 서버에 연결 |
    | -I MODULE, --include-module=MODULE     | MODULE 포함하여 실행           |
    | -X MODULE, --exclude-module=MODULE     | MODULE 배제하고 실행           |
    | -i FUNCTION, --include-module=FUNCTION | FUNCTION 포함하여 실행         |
    | -x FUNCTION, --exclude-module=FUNCTION | FUNCTION 배제하고 실행         |

    

- frida-kill

  - 프로세스를 종료하는 도구



#### 📜**Frida Script**

- Frida 에서 제공하는 스크립트 API (Javscript, C, SWIFT API)
- Javascript API
  - 기본 뼈대 구조

> Java.perform(function(){})
> (현재 스레드가 가상머신에 연결되어 있는지 확인하고, 연결되어 있다면 function을 호출)

```
Java.perform(function(){
/*
    ...
    do sth
    ...
*/
})
```



> Java.use(className)
>
> - 자바스크립트 Wrapper를 ClassName에 동적으로 가져와서 생성자를 호출하기 위해 $new()를 호출하여 객체를 인스턴스화함
> - Static, Non-Static 메소드를 사용할 수 있음
> - 메소드 구현을 변경할 수 있고 예외를 적용할 수 있음

```
var myClass = Java.use(com.mypackage.name.class) // 앱에서 사용하는 클래스와 연동되는 myclass를 정의
 
var myClassInstance = myclass.$new(); // myClass를 통해 객체 인스턴스 생성 및 정의
 
var result = myClassInstance.myMethod("param") // 클래스 내부에 있는 메소드에 접근해 인자 값을 넘겨주고 해당 결과 값을 result에 받음
 
myClass.myMethod.implementation = function(param){ // 앱에서 정의된 메소드의 구현 내용을 재작성 할 수 있음
    // do sth
}
```



- 메소드 재구현 시, 주의점!!
  (다음의 경우에는 **Overload()** 를 사용하여 재구현 해야 한다.)

```
1. 입력받은 인수가 없는 메소드
 
myClass.myMethod.overload().implementation = function() {
    // do sth
}
 
2. 두개의 바이트 배열을 인수로 입력받는 메소드
 
myClass.myMethod.overload("[B","[B").implementation = function(param1, param2){
    // do sth
}
 
3. 앱의 Context와 Boolean 형태의 인수로 입력받는 메소드
 
myClass.myMethod.overload("android.context.Context", "boolean").implementation = function(param1, param2){
    // do sth
}
```

