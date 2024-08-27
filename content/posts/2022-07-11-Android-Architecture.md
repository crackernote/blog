---
layout: single
title: "Android 아키텍처"
categories: Pentest
tag: [Mobile, Android, Pentesting]
toc: true
toc_sticky: true
toc_label: 목차
author_profile: false
---

## 1️⃣ Android 아키텍처

![img](https://blog.kakaocdn.net/dn/F14HR/btqC22agqWK/6V48akzedN1nz9DDnBrncK/img.png)

#### 📜**리눅스 커널**

\- 리눅스 커널을 기반으로 하나, 일반 리눅스 커널과는 다름

\- 경량, 저전력, 안정화, 빠른 반응성 등이 주요 핵심

\- 로우 메로리 킬러, 웨이크락, 익명공유메모리, 바인더, 파라노이드 네트워킹 적용



#### 📜**네이티브 영역**

\- Init

 \> 최초로 시작되는 프로세스로 다른 프로세스를 구동

 \> 리눅스와 달리 많은 기능을 init 프로세스 하나에 통합함

 \> 전통적인 로그인 처리에 사용되는 /etc/passwd, /etc/group 파일 사용 안함
  (모바일이라는 전제조건이 단일 사용자 이기때문에...)

 

\- 네이티브 라이브러리

 \> C/C++로 작석된 라이브러리 (bionic C)

 \> 보통 C는 리눅스 관련 라이브러리, C++는 안드로이드 전용 라이브러리

 \> 프레임워크와 라이브러리 사이에는 JNI가 필요



\- HAL

  \> 안드로이드 프레임워크와 하드웨어(센서) 사이를 연결하는 인터페이스



#### 📜**Dalvic 가상머신**

\- 안드로이드의 자바 VM 구현

\- 달빅 실행 파일(dex)을 실행 가능



#### 📜**Java 가상머신  VS  Dalvic 가상머신**

\- 스택기반 VS 레지스터 기반

\- 달빅의 명령어 수는 더 적고 코드 길이는 더 길다 (호율성, 속도 향상)

![img](https://blog.kakaocdn.net/dn/cwKo1D/btqC6k8nnWN/Y5NKL4nYELOlbzNaRu44U1/img.png)



#### 📜**Android Runtime (ART)**

\- ART = "Android Runtime"

\- 안드로이드 롤리팝(L) 버전(5.0~)부터 정식 도입

\- AOT(Ahead of Time) 런타임 방식을 사용

\- 배터리 수명, 애플리케이션 렉, 가비지 컬렉터 향상 등 여러 측면에서 성능 개선



#### 📜**AOT 컴파일**

\- 설치 시점에 소스코드를 번역 (달빅은 실행 시점에 소스코드를 번역)

\- 공간은 많이 차지하지만 실행 시간을 대폭 개선할 수 있음

\- 7.0 버전의 경우 최초 설치 시에는 JIT 를 사용하고, 기기를 사용하지 않을 때나 충전 중일때 컴파일을 조금씩 하는 방식 사용



#### 📜**런타임 라이브러리 (=Core Libraries)**

\- 아파치 하모니 프로젝트에서 가져온 라이브러리로, 일부 기능은 확장 또는 완전히 새롭게 개발

\- 프로세스가 동작하면서 라이브러리를 호출할 때 사용



#### 📜**JNI(JAVA Native Interface)**

\- 자바와 다른 언어로 만든 프로그램 사이의 상호작용을 가능하게 하는 인터페이스 제공

![img](https://blog.kakaocdn.net/dn/XBoyc/btqC86hJDpU/K2AuLbizZIpvjA5gsKJNY0/img.png)



#### 📜**바인더(Binder)**

\- 프로세스 간 통신(IPC) 메커니즘(/dev/binder) (OpenBinder 기반)

\- IPC : 서로 다른 프로세스의 공간을 보호하면서 서로간의 통신을 가능하게 함

\- 왜 바인더인가?

  \> 하드웨어 가용성 : 유연한 하드웨어 설계를 가능하게 해줌

  \> 시스템 커스터마이징 : 하드웨어 제조사와 통신사에 맞는 커스터마이징 지원

  \> 애플리케이션 제어 : 애플리케이션 샌드박스 기능 제공

![img](https://blog.kakaocdn.net/dn/vqAK9/btqC9MXoRf3/s6vZbOEkCdW8lXMrUY7us0/img.png)



#### 📜**안드로이드 프레임워크 라이브러리**

\- 'android,' 으로 시작하는 패키지

\- 대부분 자바로 구현되어 있지만, 일부는 네이티브 코드로 구현됨

\- 포함 클래스

  \> 시스템에서 제공하는 상위 수준 서비스 활용 클래스

  \> 디바이스 하드웨어에 접근하게 해주는 클래스

  \> 안드로이드 앱 제작을 위한 기본적인 클래스



#### 📜**APP**

\- 시스템 앱

  \> /system 폴더 하위에 위치 (또는 플랫폼 서명 키로 서명한 앱)

  \> 사용자가 임의로 제거하거나 변경 불가

 

\- 사용자 설치 앱

  \> /data 폴더에 위치 (읽기-쓰기 가능 파티션)

  \> 각 앱은 전용 보안 샌드박스에 설치됨

  \> 권한 분리와 최소 권한의 원칙
