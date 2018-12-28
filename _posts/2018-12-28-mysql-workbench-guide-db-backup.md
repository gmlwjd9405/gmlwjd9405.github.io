---
layout: post
title: '[MySQL] MySQL Workbench DB Backup'
subtitle: 'MySQL Workbench DB 백업하기'
date: 2018-12-28
author: heejeong Kwon
cover: '/images/mysql-workbench-guide/mysql-workbench-guide-main.png'
tags: mysql 사용법 usage backup
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 자신이 설계한 Model 백업하기
> - Table 안의 Data 백업하기
> - DB 전체 백업하기 
  - DB 내의 모든 Table과 Table 안의 Data 백업하기 

* 들어가기 전
  * 해당 POST에서 설치된 버전 정보 
    * MySQL Workbench ver: 6.3.10
    * MySQL ver: 5.7
  * 전제 조건: MySQL Workbench 실행 후 **연결된 상태**
    * 기본 MySQL Workbench 사용법은 아래의 POST를 참고하자!
    * [MySQL Workbench 사용법](https://gmlwjd9405.github.io/2018/05/09/mysql-workbench-guide.html) 참고 
  

## 자신이 설계한 Model 백업하기
* MySQL Workbench에서 ERD(Entity Relationship Diagram)를 통해 DB 스키마를 모델링할 수 있다.
  * 모델링한 ERD를 통해 자동으로 쿼리를 추출하고 이를 통해서 실제 물리적인 DB 스키마를 생성할 수 있다.
  * 설계한 Model은 Workbench 전용 모델링 파일(.mwb)로 저장해두고 재사용할 수 있다.

### 1. File > New Model (MacOS: Cmd + N)
* mydb 더블 클릭 - 원하는 Schema Name('testdb')을 입력한다.
* ![](/images/mysql-workbench-guide/mac-guide6-0.png)

### 2. Model 설계하기
**두 가지 방법**
* [방법1] Add Diagram 이용
  * Add Diagram 클릭 > Place a New table 클릭 
  * ![](/images/mysql-workbench-guide/mac-guide7-1-0.png)
    ![](/images/mysql-workbench-guide/mac-guide7-2.png)
  * Ex)
    * table name을 "user"로 설정
    * AI: auto incremental, 자동으로 1씩 증가하도록 하는 것
    * id, 이름, 이메일을 등록한다.
* [방법2] Add Table 이용
  * Add Table 클릭
  * ![](/images/mysql-workbench-guide/mac-guide8.png)
  * Ex)
    * table name을 "product"(Ex. 쇼핑몰의 상품)로 설정
    * id, 상품명, 카테고리(분류), 가격, 제조사, 잔고, 설명을 등록한다.

### 3. 자신이 설계한 Model 저장하기
<!-- * ![](/images/mysql-workbench-guide/mac-guide9-2.png) -->
* MySQL Model Tab > File > Save Model As > 원하는 Name(해당  포스트에서는 'testdb')을 입력한다.
  * ![](/images/mysql-workbench-guide/mac-guide9.png)
* Local instance Tab > Open Model
  * 저장된 모델링 파일을 통해 자신이 설계한 내용(.mwb)을 불러올 수 있다.
  * .mwb 파일: MySQL Workbench Database Structure

### 4. 자신의 설계한 Model을 실제 DB에 반영하기 
* MySQL Model Tab > Database > Forward Engineer... 
  * Forward Engineer: 자신의 설계한 Model을 실제 DB에 반영하는 것
  * Reverse Engineer: DB의 내용을 가지고 Model을 만드는 것 
* ![](/images/mysql-workbench-guide/mac-guide10-1.png)
  * Stored Connection: 자신이 설정한 Connection Name('mysql-local') 선택 

---

## Table Data 백업하기 
<span style="background-color: #e1e1e1">DB <--- **Table Data** ---> file 형태(json, csv 등)</span>

### Table Data Export Wizard
DB의 Table Data를 file 형태(json, csv 등)로 저장한다.
* 백업하기 원하는 Table이름 우클릭 > Export 
  * ![](/images/mysql-workbench-guide/db-backup1.png) 
    ![](/images/mysql-workbench-guide/db-backup2.png)
* 저장할 file(json, csv 등)의 형식과 이름, 위치를 지정한다.
  * Ex) ','로 구분하는 csv file
  * ![](/images/mysql-workbench-guide/db-backup3.png)
    ![](/images/mysql-workbench-guide/db-backup4.png)
    ![](/images/mysql-workbench-guide/db-backup5.png)
* 저장된 file의 내용 확인한다.
  * ![](/images/mysql-workbench-guide/db-backup6.png)

### Table Data Import Wizard
file 형태(json, csv 등)의 Table Data를 DB에 반영한다.
* sample data를 입력하고 DB에 반영할 수 있다.
* 기존의 data에 data를 추가한다.
  * ![](/images/mysql-workbench-guide/db-backup7.png)
* Data를 넣을 Table이름 우클릭 > Import
  * ![](/images/mysql-workbench-guide/db-backup8.png) 
* file 형태(json, csv 등)의 Table Data 선택
  * ![](/images/mysql-workbench-guide/db-backup9.png)
* Data를 넣을 Table 선택한다.
  * 기존에 존재하는 table 또는 새로운 table 생성 
  * ![](/images/mysql-workbench-guide/db-backup10.png)
    ![](/images/mysql-workbench-guide/db-backup11.png)
    ![](/images/mysql-workbench-guide/db-backup12.png)
    ![](/images/mysql-workbench-guide/db-backup13.png)
* file의 내용이 DB에 반영됐는지 확인한다.
  * ![](/images/mysql-workbench-guide/db-backup14.png)

---

## DB 전체 백업하기 
<span style="background-color: #e1e1e1">DB <--- **DB Data** ---> SQL 문</span>
* DB 내의 모든 Table과 Table 안의 Data를 백업하고 백업한 내용을 다시 DB에 가져올 수 있다.

### Data Export
현재 DB의 모든 정보를 sql 문으로 저장할 수 있다.
* 왼쪽 Navigator 메뉴 - MANAGEMENT > Data Export 클릭 
* 또는 Local instance Tab > Server > Data Export 클릭
* 백업할 DB와 Table을 선택 / 옵션 설정 / 백업 파일(.sql)의 이름 및 위치 지정 > Start Export 클릭 
  * ![](/images/mysql-workbench-guide/db-backup15.png)
  * 옵션 설정
    * Objects to Export
      * 저장 프로시저, 함수 백업 
      * 이벤트 백업 
      * 트리거 백업 
    * Export Options
      * 테이블 별로 백업 파일 생성(Export to Dump Project Folder)
      * 데이터베이스 별로 백업 파일 생성(Export to Self-Contained File)
* (mysqld dump version mismatch > Continue Anyway)
  * ![](/images/mysql-workbench-guide/db-backup16.png)
* 백업이 완료됐는지 확인한다.
  * ![](/images/mysql-workbench-guide/db-backup17.png)
  
### Data Import
DB와 관련된 정보(.sql)를 가져와 DB에 적용할 수 있다.
* (선택) Test를 위해 현재 DB를 삭제한다. 
  * 현재 DB 우클릭 > Drop Schema > Drop Now
* 왼쪽 Navigator 메뉴 - MANAGEMENT > Data Import 클릭 
* 또는 Local instance Tab > Server > Data Import 클릭
* 옵션 설정 / 복원할 sql file을 선택 / 복원할 Schema 선택 > Start Import
  * ![](/images/mysql-workbench-guide/db-backup18.png)
  * 옵션 설정
    * 기존에 존재하면 해당 Schema 선택 
    * 해당 sql file이 테이블 별 백업 파일인지 데이터베이스 별 백업 파일인지에 따라 Import Oprions을 선택
  * 복원할 Schema 선택 
    * 위에서 현재 DB를 삭제했으면 New... 클릭 > 원하는 Schema Name('testdb') 입력 
    * ![](/images/mysql-workbench-guide/db-backup19-1.png)
    * ![](/images/mysql-workbench-guide/db-backup19-2.png)
* 복원이 완료됐는지 확인한다.
  * ![](/images/mysql-workbench-guide/db-backup20.png)
* 실제 DB에 반영됐는지 조회한다.
  * ![](/images/mysql-workbench-guide/db-backup21-0.png)

  1. MySQL Model은 닫고 Local instance로 돌아온다.
  2. refresh를 한다.
  3. 해당 Schema를 기본으로 설정한다.
    * eStore 우클릭 - Set as Default Schema
    * 해당 Schema가 진하게 표시된 것을 확인할 수 있다.
  4. 'select * from table이름'으로 table에 들어간 columns를 확인한다.
  * Ex) select * from user; 입력 후 columns들을 확인한다.


# 관련된 Post
* CLI를 이용한 MySQL 설치, 실행 등을 알고 싶으시면 [MySQL 깨알 팁 모음](https://gmlwjd9405.github.io/2018/12/23/mysql-tips.html)을 참고하시기 바랍니다.
* MySQL Workbench 사용법을 알고 싶으시면 [MySQL Workbench 사용법](https://gmlwjd9405.github.io/2018/05/09/mysql-workbench-guide.html) 을 참고하시기 바랍니다.
* MySQL 설치 방법을 알고 싶으시면 [MySQL 설치](https://gmlwjd9405.github.io/2018/05/09/mysql-download.html) 를 참고하시기 바랍니다.


# References
> - [https://m.blog.naver.com/PostView.nhn?blogId=islove8587&logNo=220954758979&proxyReferer=https%3A%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.nhn?blogId=islove8587&logNo=220954758979&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
> - [https://dololak.tistory.com/458?category=636506](https://dololak.tistory.com/458?category=636506)
> - [https://m.blog.naver.com/PostView.nhn?blogId=islove8587&logNo=220954758979&proxyReferer=https%3A%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.nhn?blogId=islove8587&logNo=220954758979&proxyReferer=https%3A%2F%2Fwww.google.com%2F)