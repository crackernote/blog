---
title: "FridaLab.apk WriteUp"
date: 2023-07-16
# weight: 1
# aliases: ["/Mobile Security"]
tags: ["Mobile Security", "Penetration Testing", "Frida writeUp"]
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

## 1️⃣ FridaLab.apk WriteUp



#### 📜**FridaLab?**

\- https://rossmarks.uk/blog/fridalab/ 에서 제공되는 Frida를 익히기 위한 Challenge APK



#### 📜**FridaLab APP**

\- FridaLab app을 Nox 환경에서 실행해 보면 8가지의 문제가 나오는 것을 알 수 있다.

\- 8가지 문제에 대하여 조건이 만족하면 check를 눌렀을때 색상이 Green으로 바뀐다.
  (조건이 만족하지 못하는 경우는 빨간색!)

![image-20240909092715174](/images/FridaLab.apk-WriteUp/image-20240909092715174.png)



#### 📜**문제풀이 (1번)**

\- 문제 : Change class challenge_01's variable 'chall01' to : 1
 \> 문제는 Challenge_01 클래스의 변수인 chall01의 값을 1로 바꾸라는 것이다.
 \> 문제를 해결하기 위해 Jadx를 통해서 FridaLab을 분석해보자
![image-20240909092731733](/images/FridaLab.apk-WriteUp/image-20240909092731733.png)

​				\> uk.rossmarks.fridalab 패키지안에 challenge_01 클래스를 확인할 수 있다.
​				\> 이 클래스의 변수인 값을 1로 바꿔주면 된다.



```java
setImmediate(function(){ // 연결되어 있는 부분의 바로 다음을 실행
    Java.perform(function(){ // 스레드가 연결되어 있으면 아래의 코드를 실행
        //#1
        var test = Java.use("uk.rossmarks.fridalab.challenge_01");
        test.chall01.value = 1;
        console.log("[*] chall01 value is ", test.chall01.value);
     })
})
```

   \> 실행 결과
     프리다 실행 명령어를 통하여 스크립스 실행 : Frida -U -l fridalab.js uk.rossmakrs.fridalab
     실행결과 1번문제가 해결된 것을 확인 할 수 있다.
![image-20240909092741247](/images/FridaLab.apk-WriteUp/image-20240909092741247.png)



#### 📜**문제풀이 (2번)**

\- 문제 : Run chall02()

 \> 문제를 보면 chall02() 함수를 실행 하라는 것이다.
 \> MainActivity를 확인해보면, chall02 함수는 정의되어 있지만 사용을 하지 않는다. 따라서 MainActivity를 불러와 
    chall02함수를 실행해 주면 될 것이다. 
![image-20240909092749346](/images/FridaLab.apk-WriteUp/image-20240909092749346.png)

```java
//#2
//static method일 경우 Java.use 사용
//instance method일 경우 Java.choose를 사용
Java.choose("uk.rossmarks.fridalab.MainActivity",{
    onMatch : function(test){
        test.chall02();
    },
    onComplete : function(){
        console.log("[*]#2 is clear");
    }
})
```

 \> 실행 결과
    프리다 실행 명령어를 통하여 스크립스 실행 : Frida -U -l fridalab.js uk.rossmakrs.fridalab
    실행결과 2번문제가 해결된 것을 확인 할 수 있다.
![image-20240909092803442](/images/FridaLab.apk-WriteUp/image-20240909092803442.png)



#### 📜**문제풀이 (3번)**

\- 문제 : Make chall03() return true

 \> 문제를 보면 chall03() 함수에서 True를 리턴하도록 만드는 것이다.
![image-20240909092813522](/images/FridaLab.apk-WriteUp/image-20240909092813522.png)

```java
//#3
var test = Java.use("uk.rossmarks.fridalab.MainActivity");
test.chall03.implementation = function(){
    console.log("[*]#3 is clear");
    return true;
}
```

 \> 실행 결과
    프리다 실행 명령어를 통하여 스크립스 실행 : Frida -U -l fridalab.js uk.rossmakrs.fridalab
    실행결과 3번문제가 해결된 것을 확인 할 수 있다.
![image-20240909092821631](/images/FridaLab.apk-WriteUp/image-20240909092821631.png)



#### 📜**문제풀이 (4번)**

\- 문제 : Send "frida" to chall04()
  \> 문제를 보면 chall04() 인자값에 "frida"라는 문구를 보내어 함수를 실행시키도록 하는 것이다.
![image-20240909092829377](/images/FridaLab.apk-WriteUp/image-20240909092829377.png)

```java
Java.choose("uk.rossmarks.fridalab.MainActivity",{
    onMatch : function(test){
        test.chall04("frida");
    },
    onComplete : function(){
        console.log("[*]#4 is clear");
    }
})
```

 \> 실행 결과
    프리다 실행 명령어를 통하여 스크립스 실행 : Frida -U -l fridalab.js uk.rossmakrs.fridalab
    실행결과 4번문제가 해결된 것을 확인 할 수 있다.
![image-20240909092836988](/images/FridaLab.apk-WriteUp/image-20240909092836988.png)



#### 📜**문제풀이 (5번)**

\- 문제 : Always send "frida" to chall05()
  \> 문제를 보면 chall05() 인자값에 "frida"라는 문구를 check를 누를때 마다 보내 함수를 실행시키도록 하는 것이다.
![image-20240909092843747](/images/FridaLab.apk-WriteUp/image-20240909092843747.png)

![image-20240909092850954](/images/FridaLab.apk-WriteUp/image-20240909092850954.png)

```java
//#5
var test = Java.use("uk.rossmarks.fridalab.MainActivity");
test.chall05.implementation = function(){
    console.log("[*]#5 is clear");
    this.chall05("frida");
}
```

 \> 실행 결과
    프리다 실행 명령어를 통하여 스크립스 실행 : Frida -U -l fridalab.js uk.rossmakrs.fridalab
    실행결과 5번문제가 해결된 것을 확인 할 수 있다.
![image-20240909092858035](/images/FridaLab.apk-WriteUp/image-20240909092858035.png)



#### 📜**문제풀이 (6번)**

\- 문제 : Run chall06() after 10 seconds with correct value
  \> 문제를 보면 chall06() 함수를 check버튼을 누른뒤 10초 뒤에 실행시키는 것을 의미하는 것 같다
![image-20240909092905042](/images/FridaLab.apk-WriteUp/image-20240909092905042.png)

```java
//challenge 06, setTimeout(fn, delay) 필요
setTimeout(function(){
    console.log("[*]Click to solve the problem");
    setImmediate(function(){
        Java.perform(function(){
            var chall_06 = Java.use("uk.rossmarks.fridalab.challenge_06");
            chall_06.addChall06.overload("int").implementation = function(arg){
                Java.choose("uk.rossmarks.fridalab.MainActivity", {
                    onMatch : function(instance){
                        instance.chall06(chall_06.chall06.value);
                    },
                    onComplete : function(){
                        console.log("[*]Solved Challenge 06");
                    }
                })
                
            }
        
        })
        
    })
},10000);
```

 \> 실행 결과
    프리다 실행 명령어를 통하여 스크립스 실행 : Frida -U -l fridalab.js uk.rossmakrs.fridalab
    실행결과 6번문제가 해결된 것을 확인 할 수 있다.
![image-20240909092924433](/images/FridaLab.apk-WriteUp/image-20240909092924433.png)



#### 📜**문제풀이 (7번)**

\- 문제 : Bruteforce check07pin() then confirm with chall07()



#### 📜**문제풀이 (8번)**

\- 문제 : Change 'check' button's text value to 'Confirm"



#### 📜**문제풀이 정리**

```java
//fridaLab 정리
 
setImmediate(function(){
    Java.perform(function(){
        //#1
        var test = Java.use("uk.rossmarks.fridalab.challenge_01");
        test.chall01.value = 1;
        console.log("[*] chall01 value is ", test.chall01.value);
    
        //#2
        Java.choose("uk.rossmarks.fridalab.MainActivity",{
            onMatch : function(test){
                test.chall02();
            },
            onComplete : function(){
                console.log("[*]#2 is clear");
            }
        })
        
        //#3
        var test = Java.use("uk.rossmarks.fridalab.MainActivity");
        test.chall03.implementation = function(){
            console.log("[*]#3 is clear");
            return true;
        }
        
        //#4
        Java.choose("uk.rossmarks.fridalab.MainActivity",{
            onMatch : function(test){
                test.chall04("frida");
            },
            onComplete : function(){
                console.log("[*]#4 is clear");
            }
        })
        
        //#5
        var test = Java.use("uk.rossmarks.fridalab.MainActivity");
        test.chall05.implementation = function(){
            console.log("[*]#5 is clear");
            this.chall05("frida");
        }
        
        //#7
        var test = Java.use("uk.rossmarks.fridalab.challenge_07");
        test.check07Pin.implementation = function(){
            console.log("[*]#7 is clear");
            return ture;
        }
        
        //#8
        var test = Java.use("uk.rossmarks.fridalab.MainActivity");
        test.chall08.implementation = function(){
            console.log("[*]#8 is clear");
            return true;
        }
    })
})
 
//challenge 06, setTimeout(fn, delay) 필요
setTimeout(function(){
    console.log("[*]Click to solve the problem");
    setImmediate(function(){
        Java.perform(function(){
            var chall_06 = Java.use("uk.rossmarks.fridalab.challenge_06");
            chall_06.addChall06.overload("int").implementation = function(arg){
                Java.choose("uk.rossmarks.fridalab.MainActivity", {
                    onMatch : function(instance){
                        instance.chall06(chall_06.chall06.value);
                    },
                    onComplete : function(){
                        console.log("[*]Solved Challenge 06");
                    }
                })
                
            }
        
        })
        
    })
},10000);
```

