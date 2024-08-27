---
layout: single
title: "Active Directory Cheatsheet"
categories: OSCP
tag: [Offensive Security, Penetration Testing, OSCP, Active Directory]
toc: true
toc_sticky: true
toc_label: 목차
author_profile: false
---

## 1️⃣ Active Directory Cheatsheet

1. Perform user hunting to track down where users are logged into in the network - find users that are members of high-value groups.
2. Dump credentials and/or obtain Kerberos tickets.
3. Gain access to the user's machine using creds/ticket.
4. (Possibly) escalate privileges in the machine.
5. Repeat steps above until you have administrative privileges in the Domain Controller.
   

#### 📜**AD Enumeration**

\- Users / Groups / Computers

- 상위 권한을 가진 user 찾기
- 



| 메서드      | 설명                                                         |
| ----------- | ------------------------------------------------------------ |
| onCreate()  | 액티비티 생성 시 호출, 화면에 보이는 뷰의 일반적인 상태를 설정 |
| onStart()   | 액티비티가 사용자에게 표시되기 직전에 호출                   |
| onResume()  | 액티비티가 사용자와 상호작용 하기 전에 호출                  |
| onPause()   | 시스템이 다른 액티비티를 재개하지 직전에 호출                |
| onStop()    | 액티비티가 더이상 사용자에게 표시되지 않을 때 호출           |
| onDestroy() | 액티비티 소멸 전에 호출(마지막 호출)                         |



##### 📜**액티비티 Flow**

![img](https://blog.kakaocdn.net/dn/lonjb/btqC8HvNMP3/6FXjAHQTl0awqiE4hbxKy0/img.png)

![img](https://blog.kakaocdn.net/dn/bHPwhV/btqC85wqtW2/o93Dmo6rYxYtpQhu8VEWOk/img.png)

#### 📜**프래그먼트**

\- 액티비티처럼 이용할 수 있는 뷰

\- 한 화면에 여러 화면 구성 시 개별 액티비티 클래스의 복잡성을 줄여줄 수 있음

\- 여러개의 프래그먼트를 하나의 액티비티에 조합해 창이 여러 개인 UI 구성 가능

\- 하나의 프래그먼트 : Fragment.class + layout.xml

\- Fragment, ListFragment, PreferenceFragment, DialogFragment 등



##### 📜**프래그먼트와 액티비티의 관계**



![img](https://blog.kakaocdn.net/dn/cbZTA0/btqC6QG76En/BV44aqqyVLOVrBNPewZuak/img.png)

#### 📜**서비스**

\- 백그라운드에서 실행되는 프로세스로, 별도의 사용자 인터페이스 제공 X

\- 화면과 상관 없이 장시간 처리해야 하는 작업 구현에 적합 

  (ex. 메신저, 음악 플레이어 등)

\- 하나의 서비스 : Service.class

\- 서비스 구동 방법

 \> startService() : 어플리케이션 구성 요소가 startService() 호출 시 동작(위엄)

 \> bindService() : 어플리케이션 구성요소가 bindService() 호출 후 바인드 되면 동작 (상호작용)

| 메서드           | 설명                                 |
| ---------------- | ------------------------------------ |
| onCreate()       | 서비스 최초 생성 시 수행 (설정 절차) |
| onStartCommand() | startService() 호출 시 동작하는 함수 |
| onBind()         | bindService() 호출 시 동작하는 함수  |
| onDestroy()      | 서비스 소멸 시 동작하는 함수         |



##### 📜**서비스 생명 주기**



![img](https://blog.kakaocdn.net/dn/brBSsp/btqC9OnqIwf/zZ6jFGEwQ6Alz08Pdo55r1/img.png)



#### 📜**브로드캐스트 리시버**



\- 안드로이드 시스템 또는 다른 앱의 브로드캐스트 메시지를 송수신하는 기능을 의미

\- 이벤트 모델로 수행되는 컴포넌트 = 없으면 말고, 있으면 모두 실행

\- 전화 수신, 배터리 부족, 문자 수신, 와이파이 발견, 블루투스 연결 등

\- 수신 방법

 \> manifest 이용 : manifest에 인텐트 등록 -> Receiver 클래스 / onReceive()

 \> context 이용 : 새로운 Receive 클래스 인스턴스 -> IntenFilter 등록



#### 📜**컨텐츠 프로바이더 (Content Provider)**



\- 앱 간의 데이터 공유를 목적으로 사용하는 컴포넌트

\- 일반적으로는 다른 앱의 데이터에 외부 앱이 접근 할 수 없음

\- 주소록, 사진첩, 메모 읽어오기 (URI 기반)

\- 안드로이드에서 제공하는 URI에는 권한이 필요한 항목과 필요하지 않은 항목들이 있음



![img](https://blog.kakaocdn.net/dn/bhpsSB/btqC6PuwzLg/Rqj4dTN0pmhqFknK4a0clk/img.png)

#### 📜**인텐트**



\- 앱 구성 요소 사이에서 작업을 요청하는 메시지 객체

\- 인텐트 유형

 \> 명시적 인텐트 : 시작할 구성요소를 명시적으로 지정하는 것

 \> 암시적 인텐트 : 특정 구성요소를 지정하지 않고, 수행할 작업을 지정하면 해당 작업을 처리할 수 있는 구성요소가 선택되어 작업을 수행
   (전화걸기, 지도에 현재 위치 표시 등)



![img](https://blog.kakaocdn.net/dn/cJ9geA/btqC9Onq1SU/IZXqvjGiCkjjNGAidIdEcK/img.png)

#### 📜**빌드 프로세스**



\- 컴파일러는 소스 코드를 DEX 파일로 변환하고 그 외 모든 것을 리소스로 변환

\- APK 관리자는 DEX 파일과 리소스를 단일 APK에 결합

\- APK 생성

 \> 디버그 : 스튜디오 자동 구성

 \> 릴리즈 : 개발자 서명 필요



![img](https://blog.kakaocdn.net/dn/m90HG/btqC6mF8vsG/ak1vyOjaadVgGdmkL38M6k/img.png)

#### 📜**안드로이드 앱 프로젝트 구성요소**



\- Manifests

 \> AndroidManifest.xml

 \> 앱 구성 요소 및 권한 정보를 정의



\- java

 \> 자바 소스파일이 들어있는 폴더

 \> 패키지명과 동일한 하위 폴더들이 만들어짐



\- res

 \> 리소스 파일이 들어있는 폴더

 \> XML 레이아웃, 그림, 문자열을 정의한 XML



![img](https://blog.kakaocdn.net/dn/byDigb/btqDf1g4efn/9aLIu7NWXTTqyzaTmHPu1K/img.png)



##### 📜**gradle**



\- 빌드 배포 도구

\- 안드로이드 스튜디오와 빌드 시스템은 독립 관계

\- 안드로이드 스튜디오는 코드 편집만, gradle이 빌드

\- build.gradle

 \> 모듈의 빌드 방법을 정의한 스크립트

 \> 빌드에 사용할 SDK버전, 사용 라이브러리 등

\- setting.gradle

 \> 앱 빌드 시 포함할 모듈을 정의







