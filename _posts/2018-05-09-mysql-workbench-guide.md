---
layout: post
title: '[MySQL] MySQL Workbench 사용법'
subtitle: 'MySQL Workbench 사용법'
date: 2018-05-09
author: heejeong Kwon
cover: '/images/mysql-workbench-guide/mysql-workbench-guide-main.png'
tags: mysql 사용법 usage
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - MySQL Workbench을 이용하여 table을 설계하고 MySQL DB에 반영한다.
> - table에 들어간 columns을 조회할 수 있다.

* 들어가기 전
  * 해당 POST에서 설치된 버전 정보 
    * MySQL Workbench ver: 6.3.10
    * MySQL ver: 5.7
  * 설치 전이라면 아래의 POST를 참고하자!
    * [MySQL 설치](https://gmlwjd9405.github.io/2018/05/09/mysql-download.html) 참고 
  * MySQL Server가 실행 중이어야 한다.
    * [MySQL 깨알 팁 모음](https://gmlwjd9405.github.io/2018/12/23/mysql-tips.html) 참고 

## 1. MySQL Workbench에서 새로운 연결을 생성한다.
1. MySQL Workbench 실행 > + 클릭 
* ![](/images/mysql-workbench-guide/mac-guide1.png)
2. 원하는 Connection Name('mysql-local')을 입력한다.
* ![](/images/mysql-workbench-guide/mac-guide2.png)
* 로컬에서 실행 중인 MySQL Server를 사용하는 경우
  * hostname: 127.0.0.1 또는 localhost
  * mysql 기본 포트: 3306
3. 아래의 Test Connection 클릭 
* MySQL을 설치할 때 지정한 root password를 입력한다.
  * ![](/images/mysql-workbench-guide/mac-guide3.png)
  * 만약 connection을 실패할 경우, mysql server가 켜져 있는지 확인하거나 password가 맞는지 확인해야 한다.
  * mysql server 실행 및 root password 재설정은 [MySQL 깨알 팁 모음](https://gmlwjd9405.github.io/2018/12/23/mysql-tips.html)을 참고하자!

## 2. 실행 중인 MySQL과 연결한다.
* MySQL Connections > 위에서 지정한 Connection Name('mysql-local') 클릭 > root password 입력
* ![](/images/mysql-workbench-guide/mac-guide4.png)
* ![](/images/mysql-workbench-guide/mac-guide5.png)

## 3. File > New Model (MacOS: Cmd + N)
* mydb 더블 클릭 - 원하는 Schema Name('testdb')을 입력한다.
* ![](/images/mysql-workbench-guide/mac-guide6-0.png)

## 4. 자신이 설계한 Table을 DB에 넣는다.
**두 가지 방법**

### [방법1] Add Diagram 이용
* Add Diagram 클릭 > Place a New table 클릭 
* ![](/images/mysql-workbench-guide/mac-guide7-1-0.png)
* ![](/images/mysql-workbench-guide/mac-guide7-2.png)
* Ex)
  * table name을 "user"로 설정
  * AI: auto incremental, 자동으로 1씩 증가하도록 하는 것
  * id, 이름, 이메일을 등록한다.

### [방법2] Add Table 이용
* Add Table 클릭
* ![](/images/mysql-workbench-guide/mac-guide8.png)
* Ex)
  * table name을 "product"(Ex. 쇼핑몰의 상품)로 설정
  * id, 상품명, 카테고리(분류), 가격, 제조사, 잔고, 설명을 등록한다.

<br><mark>참고</mark> 자신이 설계한 Model 저장 
<!-- * ![](/images/mysql-workbench-guide/mac-guide9-1.png) -->
<!-- * ![](/images/mysql-workbench-guide/mac-guide9-2.png) -->
* File > save model as > 원하는 Name(해당  포스트에서는 'testdb')을 입력한다.
* 자신이 설계한 내용을 불러올 수 있다.

## 5. 자신의 설계한 Model을 실제 DB에 반영 
* Database > Forward Engineer 클릭
  * Forward Engineer: 자신의 설계한 Model을 실제 DB에 반영하는 것
  * Reverse Engineer: DB의 내용을 가지고 Model을 만드는 것 
* ![](/images/mysql-workbench-guide/mac-guide10-1.png)
  * Stored Connection: 자신이 설정한 Connection Name('mysql-local') 선택 
* ![](/images/mysql-workbench-guide/mac-guide10-2.png)
  * MySQL을 설치할 때 지정한 root password를 입력
* ![](/images/mysql-workbench-guide/mac-guide10-3.png)
* ![](/images/mysql-workbench-guide/mac-guide10-4.png)
* ![](/images/mysql-workbench-guide/mac-guide10-5.png)

## 6. 실제 DB에 반영되었는지 조회
* ![](/images/mysql-workbench-guide/mac-guide11.png) 

1. MySQL Model은 닫고 Local instance로 돌아온다.
2. refresh를 한다.
3. 해당 Schema를 기본으로 설정한다.
  * eStore 우클릭 - Set as Default Schema
  * 해당 Schema가 진하게 표시된 것을 확인할 수 있다.

## 7. 기본 SQL 문법
### 1) 'desc table이름'을 통해 설계한 columns의 내용을 확인한다.
* Ex) desc user; 입력 후 실행 **(번개모양)**
* ![](/images/mysql-workbench-guide/mac-guide12-1-0.png) 

### 2) 'insert into table이름 (column1, column2, column3) valuse (value1, value2, value3);으로 table에 data를 넣는다.
* Ex) insert into user (name, email) values ('heee', 'heee@email.com');
* ![](/images/mysql-workbench-guide/mac-guide12-2.png) 
* <mark>참고</mark> 명령어 반복
  * 아래의 Action Output의 명령어 우클릭 > 'Replace SQL Script With Selected items' 클릭
  * ![](/images/mysql-workbench-guide/mac-guide13.png) 

### 3) 'select * from table이름'으로 table에 들어간 columns를 확인한다.
* Ex) select * from user; 입력 후 columns들을 확인한다.
* ![](/images/mysql-workbench-guide/mac-guide12-3.png)


## 8. Result Grid에서 추가/수정한 내용을 DB에 반영하기 위해서는 우측 하단에 Apply를 선택한다.
* heee -> Heee 변경 후 Apply
* ![](/images/mysql-workbench-guide/mac-guide14-1-0.png) 
* ![](/images/mysql-workbench-guide/mac-guide14-2.png)


## DB 백업하기
* 자신이 설계한 Model 저장하기
* Table 안의 Data 백업하기
* DB 전체 백업하기 
  * DB 내의 모든 Table과 그 안의 Data
* 위의 내용은 [MySQL Workbench DB 백업하기](https://gmlwjd9405.github.io/2018/12/28/mysql-workbench-guide-db-backup.html) POST를 참고하자!


<mark>TIP</mark>  
* 개발할 당시에는 main memory db를 사용 - 빠르다
  * <span style="color:#4d0000">memory DB의 예: **h2 database**</span>  
  * http://www.h2database.com/html/main.html
  - Very fast, open source
  - in-memory databases
  - 최고의 실습용 DB라고 할 수 있다.
  - 웹용 쿼리툴을 제공한다.
  - MySQL, Oracle DB 시뮬레이션 기능이 있다.
  - 시퀀스, auto increment 두 가지 기능 모두 지원한다.
* 실제 release하는 경우에 MySQL을 사용



# 관련된 Post
* MySQL 설치 방법을 알고 싶으시면 [MySQL 설치](https://gmlwjd9405.github.io/2018/05/09/mysql-download.html) 를 참고하시기 바랍니다.
* CLI를 이용한 MySQL 설치, 실행 등을 알고 싶으시면 [MySQL 깨알 팁 모음](https://gmlwjd9405.github.io/2018/12/23/mysql-tips.html)을 참고하시기 바랍니다.
* MySQL Workbench에서 DB 백업 방법을 알고 싶으시면 [MySQL Workbench DB 백업하기](https://gmlwjd9405.github.io/2018/12/28/mysql-workbench-guide-db-backup.html)를 참고하시기 바랍니다.


# References
> - [https://getbootstrap.com/docs/3.3/getting-started/](https://getbootstrap.com/docs/3.3/getting-started/)
> - [https://getbootstrap.com/docs/3.3/examples/carousel/](https://getbootstrap.com/docs/3.3/examples/carousel/)
> - [https://getbootstrap.com/docs/3.3/css/](https://getbootstrap.com/docs/3.3/css/)
> - [https://www.w3schools.com/bootstrap/tryit.asp?filename=trybs_default&stacked=h](https://www.w3schools.com/bootstrap/tryit.asp?filename=trybs_default&stacked=h)
