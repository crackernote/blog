---
title: "Reversing Basic"
date: 2023-07-18
# weight: 1
# aliases: ["/Pentest"]
tags: ["Reversing", "Penetration Testing"]
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

## 1️⃣ Reversing 기초



#### 📜**CPU 아키텍처**

\- CISC (복합 명령어 집합 구조)
 \- 메모리 용량을 적게 차지하는 프로그램을 구성할 수 있도록 설계된 프로세스
 \- 소규모 라인의 프로그램으로 많은 작업을 수행하기 위해, 강력한 명령어를 명령어 집합에 포함
 \- 가변 명령어 형식과 길이, 많은 명령어 종류, 복잡한 주소 지정 방식, 복잡한 회로 구성
 \- 인텔 계열의 프로세서(X86/64)

\- RISC (축약 명령어 집합 구조)
 \- 적은 수의 컴퓨터 명령어를 수행하도록 설계된 프로세서
 \- 단순하지만 더 빨리 실행되는 소수의 명령어를 사용하는 아키텍처
 \- 복잡하고 강력한 명령어 집합은 오히려 간단한 명령어의 해석과 실행 시간까지 증가시킴 >> CISC
 \- 단순 명령어, 짧은 사이클 시간, 적재 및 저장 구조, 고정길이 명령어, 단순명령어 형식, 제한된 종류의 주소 지정 방식
 \- MIPS, ARM - IoT 디바이스 및 스마트폰에서 사용



#### 📜**메인 메모리**

\- 실행될 프로그램과 데이터가 CPU에 가기전에 잠시 머무는 장소
\- 직접 접근 저장매체(DASD), 읽기/쓰기, 휘발성
\- 모든 프로그램이 실행되려면 반드시 메모리를 거쳐야 한다. 
\- 체감 컴퓨터 속도 : CPU성능 + 메모리 성능(용량) 



#### 📜**가상 기억 장치의 필요성**

\- 메인 메모리 용량 8GB에 게임 프로그램 20GB를 올릴 수 없다.
\- 메모리 관리 요구사항
\- 다수의 프로세스가 동시에 실행될 수 있는 주소 공간
\- 각 프로세스 고유의 메모리 자원 보호 방법
\- 필요 시 프로세스 사이에 주소 공간 공유가 가능해야 함
\- 주소 공간을 프로그래머에게 투명하게 관리해야 함



#### 📜**가상 기억 장치**

\- 하드웨어와 소프트웨어를 사용해 구현한 메모리 관리 기술
\- 프로세스의 일부만 메모리에 적재하고 나머지는 보조기억장치에 둠
\- 프로그램이나 데이터를 페이지 또는 세그먼트 단위로 교환
\- 메모리 관리 및 보호 방식은 운영체제마다 다르게 정의



#### 📜**대표적인 어셈블러**

\- GAS(GNU 어셈블러) - AT&T 방식으로 무료이며 크로스 플랫폼을 지원. GNU Project에서 제작
\- MASM(Microsoft Macro Assembler) - Intel 방식으로 비주얼 스튜디오 2008부터 기본 탑재. 매크로 지원. 크로스 플랫폼 미지원
\- NASM(Netwide Assembler) - 80x86 플랫폼 용으로 크로스 플랫폼을 지원. MASM과 유사한 점이 많음 (Intel 방식)



#### 📜**운영체제**

하드웨어 자원을 관리하고, 응용 서비스를 제공
사용자와 하드웨어 사이의 인터페이스 역할
하드웨어의 고장 탐색, 오류 처리, 보안유지



#### 📜**빌드 과정**

   **Compile                  Assembly                  link**
C언어 -> Assembler Code -> 오브젝트 파일 -> exe
           <-                     <----------------------------------- 
      **decomplie                     disassemble**





