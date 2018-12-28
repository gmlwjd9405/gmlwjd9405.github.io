---
layout: post
title: '[MySQL] MySQL 설치'
subtitle: 'Windows에서의 MySQL 설치 방법'
date: 2018-05-09
author: heejeong Kwon
cover: '/images/mysql-download/mysql-download-main.png'
tags: mysql 설치 plugin 다운로드
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Windows에서의 MySQL 설치할 수 있다.


* 들어가기 전
  * 여기서는 MySQL 5.7을 기준으로 설치를 진행한다.
  * 자신이 원하는 MySQL 버전에 맞게 변경한다.


## 1. MySQL 공식 사이트에서 다운로드를 진행한다.
* MySQL 공식 사이트: [https://www.mysql.com/](https://www.mysql.com/)에서 Downloads 선택
* ![](/images/mysql-download/1.png)


## 2. MySQL Community Edition (GPL)으로 다운로드를 진행한다.
* Community (GPL) Downloads: [https://dev.mysql.com/downloads/](https://dev.mysql.com/downloads/)
* MySQL Community Downloads의 MySQL Community Server 선택
* ![](/images/mysql-download/2.png)


## 3. Select Operating System > Microsoft Windows 선택
* 자신의 OS에 맞게 선택한다.

1. MySQL Installer MSI(윈도우 설치 마법사를 통한 설치) 옆의 Download 클릭
  * ![](/images/mysql-download/3.png)
2. mysql-installer-**web**-community-5.7.17.0.msi 다운로드
  * ![](/images/mysql-download/4.png)


## 4. 설치 과정
* ![](/images/mysql-download/55.png)
* ![](/images/mysql-download/66.png)
* ![](/images/mysql-download/77.png)
  * 이때, Next말고 Execute를 선택한다.(위에서 필요한 것들을 자동으로 실행) 이 후에 Next로 다운로드를 시작한다.
  * 이 과정 생략 시 아래와 같은 Error 발생
  * ![](/images/mysql-download/mysql-download-error.png)
<!-- * ![](/images/mysql-download/mysql-download-confirm.png) -->
* ![](/images/mysql-download/88.png)
* ![](/images/mysql-download/99.png)


## 관련된 Post
* MySQL Workbench 사용법을 알고 싶으시면 [MySQL Workbench 사용법](https://gmlwjd9405.github.io/2018/05/09/mysql-workbench-guide.html) 을 참고하시기 바랍니다.
* CLI를 이용한 MySQL 설치, 실행 등을 알고 싶으시면 [MySQL 깨알 팁 모음](https://gmlwjd9405.github.io/2018/12/23/mysql-tips.html)을 참고하시기 바랍니다.
* MySQL Workbench에서 DB 백업 방법을 알고 싶으시면 [MySQL Workbench DB 백업하기](https://gmlwjd9405.github.io/2018/12/28/mysql-workbench-guide-db-backup.html)를 참고하시기 바랍니다.


# References
> - [https://www.mysql.com/](https://www.mysql.com/)
> - [https://dev.mysql.com/downloads/](https://dev.mysql.com/downloads/)
