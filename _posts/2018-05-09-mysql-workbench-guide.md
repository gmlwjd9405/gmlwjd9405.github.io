---
layout: post
title: '[MySQL] MySQL Workbench 사용법'
subtitle: 'MySQL Workbench 사용법'
date: 2018-05-09
author: heejeong Kwon
cover: '/images/mysql-workbench-guide/mysql-workbench-guide-main.png'
tags: MySQL 사용법
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - MySQL Workbench을 이용하여 table을 설계하고 MySQL DB에 반영한다.
> - table에 들어간 columns을 조회할 수 있다.


* 들어가기 전
  * 해당 POST에서 설치된 MySQL의 버전은 5.7이다.
  * 설치 전이라면 아래의 POST를 참고하자!
    * [MySQL 설치](https://gmlwjd9405.github.io/2018/05/11/mysql-download.html)


## 1. MySQL workbench를 실행한다.
* MySQL Connections > local instance mysql 5.7 클릭
* ![](/images/mysql-workbench-guide/guide1.png)


## 2. 미리 설치할 때 정했던 pw를 입력


## 3. File > New Model
* ![](/images/mysql-workbench-guide/guide2.png)


## 4. mydb 더블 클릭 - Name을 자신의 프로젝트명(해당 프로젝트에서는 "eStore")으로 변경
* ![](/images/mysql-workbench-guide/guide3.png)


## 5. 자신이 설계한 Table을 DB에 넣는다.
* Add Table 클릭 - table name을 "product"(EX. 쇼핑몰의 상품)로 설정
* ![](/images/mysql-workbench-guide/guide4.png)
* AI: auto incremental, 자동으로 1씩 증가하도록 하는 것
* 아이디, 상품명, 카테고리(분류), 가격, 제조사, 잔고, 설명을 등록한다.


## 6. File > save model as > 자신의 프로젝트명(해당 프로젝트에서는 "eStore")으로 프로젝트 디렉터리('eStore')에 저장


## 7. 이렇게 설계한 것을 실제 DB에 반영하기 위해서 Database > Forward Engineer 클릭
* ![](/images/mysql-workbench-guide/guide5.png)
* ![](/images/mysql-workbench-guide/guide6.png)
* ![](/images/mysql-workbench-guide/guide7.png)
* ![](/images/mysql-workbench-guide/guide8.png)
* ![](/images/mysql-workbench-guide/guide9.png)
* ![](/images/mysql-workbench-guide/guide10.png)
* ![](/images/mysql-workbench-guide/guide11.png)


## 8. 실제 DB에 반영되었는지 조회
1. MySQL Model은 닫고 Local instance로 돌아온다.
2. refresh를 한다.
  * ![](/images/mysql-workbench-guide/guide12.png)
3. 해당 Schema를 기본으로 설정한다.
  * eStore 우클릭 - Set as Default Schema
  * ![](/images/mysql-workbench-guide/guide13.png)


## 9. 'desc table이름'을 통해 설계한 columns의 내용을 확인한다.
* desc product 입력 후 실행(번개모양)
* ![](/images/mysql-workbench-guide/guide14.png)


## 10. 'select * from table이름'으로 table에 들어간 columns를 확인한다.
* select * from product 입력 후 columns들을 확인한다.
* ![](/images/mysql-workbench-guide/guide15.png)


## 11. db에 반영하기 위해서는 우측 하단에 Apply를 선택한다.
* ![](/images/mysql-workbench-guide/guide16.png)



<mark>TIP</mark>  
* 개발할 당시에는 main memory db를 사용 - 빠르다
  * <span style="color:#4d0000">memory DB의 예: **h2 database**</span>  
  * http://www.h2database.com/html/main.html
  * Very fast, open source
  * in-memory databases
* 실제 release하는 경우에 MySQL을 사용



# 관련된 Post
* MySQL 설치방법 등을 알고 싶으시면 [MySQL 설치](https://gmlwjd9405.github.io/2018/05/11/mysql-download.html) 를 참고하시기 바랍니다.


# References
> - [https://getbootstrap.com/docs/3.3/getting-started/](https://getbootstrap.com/docs/3.3/getting-started/)
> - [https://getbootstrap.com/docs/3.3/examples/carousel/](https://getbootstrap.com/docs/3.3/examples/carousel/)
> - [https://getbootstrap.com/docs/3.3/css/](https://getbootstrap.com/docs/3.3/css/)
> - [https://www.w3schools.com/bootstrap/tryit.asp?filename=trybs_default&stacked=h](https://www.w3schools.com/bootstrap/tryit.asp?filename=trybs_default&stacked=h)
