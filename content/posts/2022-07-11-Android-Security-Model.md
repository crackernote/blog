---
layout: single
title: "Android 보안모델"
categories: Pentest
tag: [Mobile, Android Security Model, Pentesting]
toc: true
toc_sticky: true
toc_label: 목차
author_profile: false
---

## 1️⃣ Android 보안모델	

\- 기본적으로 리북스 커널이 제공하는 보안 기능을 사용

\- 스마트폰을 사용하는 방식에 맞게 보안 기능을 재구성

\- 애플리케이션 격리 : Sandbox, Permission, TrustZone

\- 플랫폼 강화 : SELinux, ASLR, Exploit mitigation

\- 디바이스 무결성 : Data Encryption

\- 업무를 위한 기능 : 프로파일 기업 서비스



#### 📜**Sandbox**

\- 앱 설치 시 고유한 UID를 자동으로 할당 (10000번부터 시작)
  ex) user id u0_a36은 uid 10036번을 가짐

\- UID별로 전용 프로세스 및 디렉터리를 가짐 <- 샌드박스

\- 시스템 데몬 및 앱은 고정된 UID 사용 (일부만 루트 0)



#### 📜**Sandbox**

\- 앱 설치 시 고유한 UID를 자동으로 할당 (10000번부터 시작)
  ex) user id u0_a36은 uid 10036번을 가짐

\- UID별로 전용 프로세스 및 디렉터리를 가짐 <- 샌드박스

\- 시스템 데몬 및 앱은 고정된 UID 사용 (일부만 루트 0)



#### 📜**Permission**

\- 앱 내의 AndroidManifest.xml 파일에 권한을 명시해 요청 가능

\- 앱 설치 시 사용자에게 해당 권한 허가 여부를 물어봄

\- 일부 권한은 미리 설치된 기본 앱이나 OS와 동일한 키로 서명된 앱만 허용



#### 📜**TrustZone**

\- 디바이스 공간을 물리적 또는 논리적으로 분리해서 주요 자원을 보호하는 방법

\- 보호되는 영역을 TEE(Trusted Execution Environment)라고 부름

\- 활용 사례 : 지문 저장 정보, 지불 관련 기능 저장, DRM 암호 정보 저장



#### 📜**코드서명 & 플랫폼키**

\- 모든 안드로이드 앱은 개발자 서명을 필요로 함

\- Same Origin Policy : 동일 개발자만 업데이트 가능 

\- 시스템 앱은 여러 플랫폼 키로 서명됨 : 같은 키 서명 시 리소스 공유



#### 📜**검증된 부트**

\- 안드로이드 4.4 버전부터 도입 : verify

\- 암호학적 해시 트리를 이용해 블록 디바이스의 무결성 검증

\- 부트로더 : 디바이스 전원이 켜질 때 실행되는 하드웨어 고유 특수 프로그램

\- 안드로이드 부트로더는 미공개 > SoC에 특화되어 있음



#### 📜**SELinux**

\- 강제 접근 제어를 포함한 접근 제어 보안 정책을 지원하는 리눅스 커널 보안 모듈

\- 안드로이드 5.0 부터 완전히 적용됨

\- 세가지 운용 모드 : 비활성(disabled), 허용(permissive), 시행(enforcing)

\- getenforce / setenforce 0

![img](https://blog.kakaocdn.net/dn/liiAy/btqC5nSTkNz/syw9ww5NKT4r1pWsiCXeG1/img.png)
