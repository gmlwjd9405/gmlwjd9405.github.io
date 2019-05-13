---
layout: post
title: '[DB] SQL문 기본 문법 - 데이터 추가, 삭제, 갱신'
subtitle: '데이터를 조작하는 insert, delete, update 쿼리문을 이해한다.'
date: 2019-05-13
author: heejeong Kwon
cover: '/images/database/db-main.png'
tags: database sql query db
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - INSERT 명령문 (SQL 기본 문법)
> - UPDATE 명령문 (SQL 기본 문법)
> - DELETE 명령문 (SQL 기본 문법)

### INSERT 명령문 (SQL의 DML)
- DB의 테이블에 행 추가
  - 웹 페이지에서 '신규등록'이나 '추가'와 같은 버튼을 클릭했을 때 처리되는 데이터 추가 기능 
- SELECT 명령의 경우 실행하면 그 결과가 클라이언트에게 반환되자만, INSERT 명령은 데이터가 클라이언트에서 서버로 전송되므로 반환되는 결과가 없다.

- **기본 사용 예시**
  ```sql
  > INSERT INTO 테이블명 VALUES(값1, 값2, ...);
  > INSERT INTO sample VALUES(1, 'HEEE', '2019-04-27');
  ```

- **값을 저장할 열 지정**
  ```sql
  > INSERT INTO 테이블명 (열1, 열2, ...) VALUES(값1, 값2, ...);
  > INSERT INTO sample (name, no) VALUES('HEEE2', 2);
  ```

- **NOT NULL 제약**
  - NULL을 허용하고 싶지 않은 열에 NOT NULL 제약을 걸어둔다.

- **DEFAULT**
  - 명시적으로 값을 지정하지 않았을 경우 사용하는 초깃값을 말한다.
  - `DESC 테이블명`으로 테이블의 열 구성을 살펴보면 Default라는 항목에서 초깃값 확인 가능

### DELETE 명령문 (SQL의 DML)
- DB의 테이블에서 행을 삭제
- 삭제는 행 단위로 수행되며, 열을 지정하여 해당 열만 삭제할 수는 없다.
- DELETE 명령을 실행할 때는 재확인을 위한 대화창 같은 것이 표시되지 않기 때문에 주의해야 한다.

- **기본 사용 예시**
  ```sql
  > DELETE FROM 테이블명 WHERE 조건식;
  ```
  ```sql
  // sample 테이블의 모든 데이터 삭제
  > DELETE FROM sample;
  // sample 테이블의 no 열이 2인 행 삭제 
  > DELETE FROM sample where no=2;
  ```

- **물리삭제와 논리삭제**
  - DB에서 데이터를 삭제하는 방법은 용도에 따라 크게 '물리삭제'와 '논리삭제'로 나뉜다.
  - **물리삭제**
    - SQL의 DELETE 명령을 사용해 직접 데이터를 삭제하는 것
    - Ex) SNS 서비스에서의 사용자 탈퇴 
  - **논리삭제**
    - 테이블에 '삭제 플래그'를 두어 행을 삭제하는 대신, SQL의 UPDATE 명령을 사용해 해당 플래그의 값을 유효하게 갱신해두는 것 
    - 장점: 삭제 되기 전의 상태로 간단히 되돌릴 수 있다.
    - 단점: 삭제해도 DB의 저장공간이 늘어나지 않는다. 이로 인해 DB 크기가 증가하여 검색속도가 떨어진다.
    - Ex) 쇼핑 사이트에서의 주문 취소 

### UPDATE 명령문 (SQL의 DML)
- DB의 테이블에서 데이터를 갱신
  - 웹 페이지에서 '등록'이나 '갱신'와 같은 버튼을 클릭했을 때 처리되는 데이터 갱신 기능 
- DELETE 명령어와 달리 셀 단위로 데이터를 갱신할 수 있다.
- `WHERE 구`를 생략한 경우에는 테이블의 모든 행이 갱신된다.
- 테이블에 존재하지 않는 열을 지정하면 에러가 발행하여 해당 명령은 실행되지 않는다.


- **기본 사용 예시**
  ```sql
  > UPDATE 테이블명 SET 열1=값1, 열2=값2, ... WHERE 조건식;
  ```
  ```sql
  > UPDATE sample SET name='HEEE' WHERE no=2;
  > UPDATE sample SET name='HEEE', date='2019-04-26' WHERE no=2;
  > UPDATE sample SET name=NULL; // NULL로 초기화 
  ```

- **SET 구의 실행순서**
```sql
> UPDATE sample SET no=no+1, a=no; // 1번
> UPDATE sample SET a=no, no=no+1; // 2번
```
  - MySQL에서는 기술한 식의 순서에 따라 갱신 처리가 일어난다.
    - 1번에서의 no 열의 값은 갱신된 이후의 값을 반환하고 a 열은 갱신된 값을 참조한다.
    - Ex) 1번의 경우, 초기 no: 2, 3 -> 갱신 후 no: 3, 4 / a: 3, 4
    - 2번에서의 a 열은 갱신되기 전의 값을 참조하고 이후에 no 열이 갱신된다.
    - Ex) 2번의 경우, 초기 no: 2, 3 -> 갱신 후 no: 3, 4 / a: 2, 3
    - 즉, MySQL 의 경우 갱신식 안에서 열을 참조할 때는 처리 순서를 고려할 필요가 있다.
  - Oracle에서는 SET 구에 기술한 식의 순서가 처리에 영향을 주지 않는다.
    - 1, 2번 모두 no 열의 값이 항상 갱신 이전의 값을 반환하기 때문에 no 열을 참조하는 a 열의 값은 갱신 전의 값이 된다.
    - 이후에 no 열이 갱신된다. 
    - Ex) 1, 2번 모두, 초기 no: 2, 3 -> 갱신 후 no: 3, 4 / a: 2, 3

# 관련된 Post
- SQL문 기본 문법 중 select 쿼리문에 대해 알고 싶으시면 [SQL문 기본 문법 - 검색편(select)](https://gmlwjd9405.github.io/2019/04/25/db-sql-select.html)를 참고하시기 바랍니다.

# Reference
> - [http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968482311](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968482311)
