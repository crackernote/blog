---
title: "MySQL"
date: 2023-07-16
# weight: 1
# aliases: ["/Pentest"]
tags: ["Web Security", "Penetration Testing", "MySQL"]
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

## 1️⃣ MySQL

MySQL의 Database, INFORMATION_SCHEMA를 사용하여 데이터베이스에 접근하는 방법에 대해 설명합니다.



### 📜**Database**

Database: 필요한 데이터를 유기적으로 결합하여 저장한 집합체: 통합된 정보들을 저장하여 운영할 수 있는 공용 데이터들의 묶음
데이터베이스는(Database) 여러 사람이 공유하여 사용할 목적으로 통합, 관리하는 데이터의 집합입니다. 새로운 환경에서도 기존의 데이터를 효율적으로 사용할 수 있도록 데이터를 구조화하여 저장한 것입니다.



### 📜**MySQL**

SQL은 Structured Query Language의 약자입니다. SQL은 데이터베이스에서 데이터를 정의, 조작, 제어하기 위해 사용하는 언어입니다. 그 중에서도 이 포스트는 MySQL 8.0에 대해 설명합니다.

MySQL은 Oracle과 더불어 가장 널리 사용되고 있는 데이터베이스 관리 시스템입니다. 다른 프로그래밍언어가 프로그램을 제작하기 위한 것이라고 한다면 SQL은 데이터베이스에 접근하기 위한 것이라고 할 수 있습니다.

MySQL의 구조는 다음과 같이 간단히 나타낼 수 있습니다.

Database Server ⊃ Database ⊃ Table(Field) ⊃ Column ⊃ Record


데이터베이스 서버 안에는 데이터베이스들이 있고, 데이터 베이스 안에는 테이블들이 있습니다. 그리고 테이블들은 칼럼(열)마다 레코드를 보관하고 있습니다.

MySQL을 이용해서 Database에 접근하는 법은 다음과 같습니다.

```
SELECT * FROM TERADA_TABLE;
```

위와 같은 코드(Code)를 Query, 쿼리문이라고 합니다. 사용자는 MySQL의 문법에 따라 Query를 작성하고, 이를 통해 데이터의 입출력, 조작, 조회 등을 할 수 있습니다. MySQL의 다양한 문법은 [이곳](https://dev.mysql.com/doc/refman/8.0/en/sql-syntax.html)에서 찾아 볼 수 있습니다.



### 📜**INFORMATION_SCHEMA**

데이터베이스의 양이 방대해지고, 그 데이터들을 효율적으로 관리하기위해서 메타데이터(Metadata)라는 것이 존재합니다. 메타데이터는 쉽게 말하자면 데이터의 데이터, 정보들에 관한 정보입니다.

MySQL은 INFORMATION_SCHEMA라는 이름의 데이터베이스에 메타데이터를 저장하고 있습니다. 이 INFORMATION_SCHEMA에는 데이터베이스, 테이블, 컬럼(column)의 이름, 컬럼의 데이터 타입, 접근 권한 등 아주 민감하고 중요한 정보가 들어있습니다.

이하부터는 INFORMATION_SCHEMA에 접근하여 그 정보들을 열람하는 방법에 대해 설명합니다.

예제는 다음과 같은 Database에서 진행합니다.

```
Database Server
└ information_schema
└ performance_schema
└ sys
└ mysql
└ COUNTRIES
    └ KOREA
    -------------------------------------
    | CITY_NAME | TAG       | IS_VISIT |
    | --------- | --------- | -------- |
    | SEOUL     | CAPITAL   | 1        |
    | JEJU      | MANDARINE | 1        |
    | BUSAN     | SEAGULL   | 0        |
    -------------------------------------
    └ JAPAN
    -------------------------------------
    | CITY_NAME | VILLAGE | IS_VISIT |
    | --------- | ------- | -------- |
    | TOKYO     | SIBUYA  | 1        |
    | OSAKA     | NANBA   | 0        |
    | FUKUOKA   | HAKATA  | 1        |
    -------------------------------------
    └ USA
    ----------------------------------------
    | STATE_NAME  | BASEBALL | IS_VISIT |
    | ----------- | -------- | -------- |
    | ILLINOIS    | CUPS     | 1        |
    | NEW YORK    | METS     | 0        |
    | LOS ANGELES | DODGERS  | 1        |
```

**Note** : COUNTRIES 는 데이터베이스의 이름입니다. 이외에는 시스템 데이터베이스입니다.



#### **1. 데이터베이스 조회**

데이터베이스 서버 내의 데이터베이스의 이름을 조회할 수 있습니다.

```
> SELECT SCHEMA_NAME 
  FROM INFORMATION_SCHEMA.SCHEMATA;
> ------------------------------
    |   SCHEMA_NAME          |
    |   -------------------- | 
    |   INFORMATION_SCHEMA   |
    |   PERFORMANCE_SCHEMA   |
    |   MYSQL                | 
    |   SYS                  |
    |   COUNTRIES            |
  ------------------------------
```

**Note**:SHOW DATABASE;또한 위와 같은 결과를 보입니다.

**Note**:INFORMATION_SCHEMA.SCHEMATA에는 데이터베이스이름(SCHEMA_NAME)이외의 **칼럼**도 있습니다.



#### **2. 테이블 조회**

데이터베이스명을 알아내었다면 데이터베이스 내의 테이블의 정보 또한 획득할 수 있습니다.

```
> SELECT TABLE_NAME 
  FROM INFORMATION_SCHEMA.TABLES 
  WHERE TABLE_SCHEMA="COUNTIRES";
> ----------------------
    |   TABLE_NAME   |
    |   ----------   | 
    |   KOREA        |
    |   JAPAN        |
    |   USA          | 
  ----------------------
```

**Note**: INFORMATION_SCHEMA.TABLE에서 TABLE_SCHEMA는 해당 테이블이 속한 DB를 가리킵니다.

다음과 같이 출력 결과를 필터링할 수도 있습니다.

```
> SELECT TABLE_NAME 
  FROM INFORMATION_SCHEMA.TABLES 
  WHERE TABLE_SCHEMA="COUNTIRES" 
  LIMIT 0,1;
> ---------------------
    |   TABLE_NAME   |
    |   ----------   | 
    |   KOREA        |
  ---------------------
 
> SELECT MAX(TABLE_NAME) 
  FROM INFORMATION_SCHEMA.TABLES 
  WHERE TABLE_SCHEMA="COUNTIRES";
> ---------------------
    |   TABLE_NAME   |
    |   ----------   | 
    |   USA          |
  ---------------------
```

**Note**: 실제로 위와같은 방법이 Blind SQL Injection에서 사용됩니다.

위와 같은 방법을 통해 USE COUNTRIES;SHOW TABLES;을 사용하지 않고 데이터베이스 내의 테이블을 조회할 수 있습니다. INFORMATION_SCHEMA.TALBES 에는 테이블명(TABLE_NAME) 뿐만 아니라 테이블의 행 수(TABLE_ROWS) 같은 정보도 포함되어있습니다.



#### **3. 컬럼 조회**

테이블에 접근했다면 이제 테이블 속에 어떤 컬럼들이 있는지 확인할 수 있습니다. 이번엔 조금 다른 방식으로 데이터베이스를 특정해보겠습니다.

```
> SELECT COLUMN_NAME 
  FROM  INFORMATION_SCHEMA.COLUMNS 
  WHERE TABLE_SCHEMA!="INFORMATION_SCHEMA" 
  AND TABLE_SCHEMA!="PERFORMANCE_SCHEMA" 
  AND TABLE_SCHEMA!="MYSQL" 
  AND TABLE_SCHEMA!="SYS";
> ----------------------
    |   COLUMN_NAME   |
    |   ----------    | 
    |   CITY_NAME     |
    |   STATE_NAME    |
    |   TAG           |
    |   VILLAGE       |
    |   BASEBALL      |
    |   IS_VISIT      |
  ----------------------
```

메타데이터 데이터베이스 이외의 데이터베이스를 선택함으로서 사용중인 데이터베이스를 특정하였고, INFORMATION_SCHEMA.COLUMNS가 가지고 있는 정보 중 컬럼명(COLUMN_NAME)만을 획득하였습니다. 하지만 이것은 데이터베이스 내의 모든 컬럼명을 가져오는 것으로 테이블마다 컬럼명이 다르다면 어떤 컬럼이 어떤 테이블에 속해 있는지 알 수 없을 것입니다. 따라서 특정한 테이블의 컬럼명을 가져오기 위해서 다음과 같은 Query를 작성할 수 있습니다.

```
> SELECT COLUMN_NAME 
  FROM INFORMATION_SCHEMA.COLUMNS 
  WHERE TABLE_NAME="JAPAN";
> ----------------------
    |   COLUMN_NAME   |
    |   ----------    | 
    |   CITY_NAME     |
    |   VILLAGE       |
    |   IS_VISIT      |
  ----------------------
```



#### **4. 레코드 조회**

데이터베이스명, 테이블명, 컬럼명을 파악했다면 이제 어떤 레코드에도 접근할 수 있습니다. 위에서 조회한 정보를 바탕으로 다음과 같이 Query를 작성해 볼 수 있습니다.

```
> SELECT CITY_NAME, VILLAGE
  FROM JAPAN;
> ---------------------------
    | CITY_NAME | VILLAGE |
    | --------- | ------- |
    | TOKYO     | SIBUYA  |
    | OSAKA     | NANBA   |
    | FUKUOKA   | HAKATA  |
  ----------------------------
> SELECT VILLAGE 
  FROM JAPAN 
  WHERE VILLAGE 
  IN ('SHINJUKU','SIBUYA','OMOTESANDO','HANEDA');
> ------------------
    |   VILLAGE   |
    |   --------  | 
    |   SIBUYA    |
  ------------------
```

위의 내용들을 바탕으로 다양한 SubQuery와 함께 INFORMATION_SCHEMA의 데이터를 자유롭게 열람할 수 있습니다.

