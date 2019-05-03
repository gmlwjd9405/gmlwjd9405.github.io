---
layout: post
title: '[DB] SQL문 기본 문법 - 검색편(select)'
subtitle: '데이터를 검색하는 select 쿼리문을 이해한다.'
date: 2019-04-25
author: heejeong Kwon
cover: '/images/database/db-main.png'
tags: database sql query db
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - SQL이란
>   - SQL 명령의 종류
> - SELECT 명령문 (SQL 기본 문법)
>   - 검색 조건 지정
>   - 검색 조건 조합
>   - 패턴 매칭에 의한 검색 

 
## SQL문
SQL은 <span style="background-color: #e1e1e1">"관계형 데이터베이스" 관리 시스템(RDBMS, Relational Database Management System)을 조작</span>할 때 사용하는 언어이다.
- RDBMS: 행과 열을 가지는 **표 형식** 데이터(2차원 데이터)를 저장하는 형태의 DB
  - 행(레코드), 열(컬럼/필드), 셀(행과 열이 만나는 부분, 하나의 데이터 값)
  - 수치형, 문자열형, 날짜시간형, NULL(값이 없는 데이터) 등의 자료형이 존재 

### SQL 명령의 종류
1. DML (Data Manipulation Language)
  - **데이터 조작**, SQL의 가장 기본이 되는 명령셋(set)
  - DB에 새롭게 데이터를 추가하거나 삭제하거나 내용을 갱신하는 등에 사용
2. DDL (Data Definition Language)
  - **데이터 정의**
  - DB 객체(object)라는 데이터 그릇을 이용하여 데이터를 관리하는데, 이 같은 객체를 만들거나 삭제하는 명령어
  - DB 객체에는 테이블, 뷰(View) 등이 있다.
3. DCL (Data Control Language)
  - **데이터 제어**
  - 트랜잭션을 제어하는 명령과 데이터 접근 권한을 제어하는 명령 포함

### DESC 명령 (SQL 명령 아님)
- 테이블의 구조 참조
  - 테이블에 어떤 열이 정의되어 있는지 알 수 있다.
  - 열 이름(Field), 열의 자료형(Type), NULL 제약사항(Null), Key 지정 정보(Key), 생략된 경우의 기본값(Default) 등 해당 테이블의 구조 정보를 확인할 수 있다.

```sql
> desc tablename;
```

## SELECT 명령문 (SQL의 DML)
- DML에 속하는 명령으로 "질의"나 "쿼리"라 불리기도 한다.

```sql
// tableName에 해당하는 테이블의 모든 열을 읽는다.
> select * from tablename;

// 위와 동일 (대소문자 구별 X)
> SELECT * from tableName;
```
  - 예약어와 DB 객체명은 대소문자를 구별하지 않는다.

### 검색 조건 지정
- **열 지정**

```sql
> select 열1, 열2 from 테이블명 where 조건식
> SELECT address, name FROM sample;
```

- **행 지정**

```sql
// select 구 -> from 구 -> where 구 (순서 중요)
> select 열 from 테이블명 where 조건식

// 수치형
> SELECT * FROM sample WHERE id=2; // id 열 값이 2와 동일한 행만 검색
> SELECT * FROM sample WHERE id<>2; // id 열 값이 2가 아닌 행만 검색

// 문자열형
> SELECT * FROM sample WHERE name='아무개'; 

// NULL값
> SELECT * FROM sample WHERE birthday IS NULL;
```
  - where 구(조건식)는 '열과 연산자, 상수로 구성되는 식'이다.

### 검색 조건 조합 
- **AND, OR, NOT**

```sql
> 조건식1 AND 조건식2
> SELECT * FROM sample WHERE a<>0 AND b<>0; // a열, b열이 모두 0이 아닌 행 검색

> 조건식1 OR 조건식2
> SELECT * FROM sample WHERE a<>0 OR b<>0; // a열이 0이 아니거나 b열이 0이 아닌 행 검색

> NOT 조건식
> SELECT * FROM sample WHERE NOT(a<>0 OR b<>0); // a열이 0이 아니거나 b열이 0이 아닌 행을 제외한 나머지 행 검색
```
  - AND는 OR에 비해 우선 순위가 높다.

### 패턴 매칭에 의한 검색
- 특정 문자나 문자열이 포함되어 있는지를 검색하고 싶은 경우 **'패턴 매칭'**(부분 검색)을 사용 
- **LIKE** 

```sql
> 열 LIKE '패턴'

// text 열에 'SQL'을 포함하는 행을 검색
> SELECT * FROM sample WHERE text LIKE 'SQL%'; // (전방일치) 
> SELECT * FROM sample WHERE text LIKE '%SQL%'; // (중간일치) 

// text 열에 '%'(메타문자)을 포함하는 행을 검색
> SELECT * FROM sample WHERE text LIKE '%\%%'; // 이스케이프 문자(/) 사용 

// text 열에 'It's'을 포함하는 행을 검색
> SELECT * FROM sample WHERE text LIKE 'It''s'; // '를 문자열 상수 안에 포함할 경우 2개를 연속해서 기술 
```
  - 사용할 수 있는 메타문자(또는 와일드카드)는 **'%'와 '_'**가 있다.
  - %(퍼센트): 임의의 문자열과 매치, 빈 물자열에도 매치한다.
  - _(언더스코어): 임의의 문자 하나
  - *는 사용할 수 없다.


<!-- # 관련된 Post -->


# Reference
> - [http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968482311](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968482311)
