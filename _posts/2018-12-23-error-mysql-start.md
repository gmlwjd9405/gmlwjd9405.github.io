---
layout: post
title: '[Error] mysql.server start 사용 시 오류'
subtitle: 'ERROR! The server quit without updating PID file ...'
date: 2018-12-23
author: heejeong Kwon
cover: '/images/error/error-main.png'
tags: mysql error 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

* Mac OS: /usr/local/var/mysql/ 
* Linux OS: /var/lib/mysql/

## 오류 내용
`ERROR! The server quit without updating PID file (/usr/local/var/mysql/...)`
* 주로 새로운 mysql버전으로 실행 시키려고 할 때 이 오류가 발생한다.
  * Ex) mysql8.0으로 실행했다가 mysql5.7을 다운받아 5.7버전으로 실행시키려 하는 경우 5.7버전 실행 불가.
* ![](/images/error/mysql-start-erorr-msg.png)

## 해결 방법
1. mysql 인스턴스가 실행 중인지 확인
```bash
$ ps -ef | grep mysql
```
2. 실행 중인 프로세스를 종료
```bash
$ kill -15 PID
```
3. mysql 소유자 확인
```bash
$ ls -laF /usr/local/var/mysql/
drwxr-x---    8 heejeong  staff       256 12 24 21:22 mysql/
```
4. 소유자를 mysql로 변경한다. 
```bash
$ sudo chown -R mysql /usr/local/var/mysql/
drwxr-x---    8 _mysql    staff       256 12 24 21:22 mysql/
```

<!-- 5. 위 경로의 권한을 변경하여 해결하는 방법이 있다.
```bash
$ sudo chmod -R 777 /usr/local/var/mysql
(sudo는 root권한이 아닐 경우)
``` -->

5. mysql 실행


<!-- # 관련된 Post
* []() -->


# References
> - [https://stackoverflow.com/questions/4963171/mysql-server-startup-error-the-server-quit-without-updating-pid-file/35070831](https://stackoverflow.com/questions/4963171/mysql-server-startup-error-the-server-quit-without-updating-pid-file/35070831)
> - [http://bellsilver7.tistory.com/28](http://bellsilver7.tistory.com/28)
> - [https://manage.accuwebhosting.com/knowledgebase/2342/How-to-Fix-MySQL-Error-The-server-quit-without-updating-PID-file.html](https://manage.accuwebhosting.com/knowledgebase/2342/How-to-Fix-MySQL-Error-The-server-quit-without-updating-PID-file.html)
> - [https://www.interserver.net/tips/kb/mysql-error-server-quit-without-updating-pid-file/](https://www.interserver.net/tips/kb/mysql-error-server-quit-without-updating-pid-file/)