---
layout: post
title: '[MySQL] MySQL 깨알 팁 모음!'
subtitle: 'MySQL 설치, 실행, 환경 설정, 접속 오류, 접속 권한, 한글 설정'
date: 2018-12-23
author: heejeong Kwon
cover: '/images/mysql-download/mysql-download-main.png'
tags: mysql plugin error 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - MySQL 설치 - 실행 - 기본 설정(root password 변경) - 접속 
  - brew를 통한 설치
  - 직접 다운로드를 통한 설치 (dmg 파일)
> - MySQL 한글 설정 (UTF-8)
  - charset utf-8
> - MySQL 삭제
> - MySQL 접속 오류
  - 임시 비밀번호 분실한 경우
  - 비밀번호가 맞아도 접속이 안되는 경우
> - MySQL 접속 권한 설정 

* 환경: MacOS, zsh

---

## MySQL 설치, 실행 및 기본 환경 설정 
### 1. [brew](https://brew.sh/)를 통한 설치
1. MySQL 설치
* 최신 버전 설치 (8.0.X)
```bash
$ brew install mysql
```
  * ![](/images/mysql-download/mysql-install-8.png)
* 5.7 버전 설치
```bash
$ brew install mysql@5.7
```
  * ![](/images/mysql-download/mysql-install-57.png)
* 다른 버전 설치 방법
  * `$ brew search mysql`로 검색, 원하는 버전의 formula 이름을 확인한다.
  * `$ brew install <설치할 formula>`으로 mysql을 설치한다.

2. MySQL 실행
* 최신 버전 실행 (8.0.X)
```bash
$ brew services start mysql
# background에서 실행시킬 필요가 없는 경우 아래 이용 
$ mysql.server start
```
* 5.7 버전 실행
  * 실행 전 환경 설정 필요 (.bash_profile 또는 .zshrc에 아래 내용 추가)
```bash
# If you need to have mysql@5.7 first in your PATH run:
export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"
# For compilers to find mysql@5.7 you may need to set:
export LDFLAGS="-L/usr/local/opt/mysql@5.7/lib"
export CPPFLAGS="-I/usr/local/opt/mysql@5.7/include"
# For pkg-config to find mysql@5.7 you may need to set:
export PKG_CONFIG_PATH="/usr/local/opt/mysql@5.7/lib/pkgconfig"
```
```bash
$ brew services start mysql@5.7
# background에서 실행시킬 필요가 없는 경우 아래 이용 
$ /usr/local/opt/mysql@5.7/bin/mysql.server start
# 하나의 버전만 깔려 있는 경우는 이 명령어도 사용 가능 
# $ mysql.server start
```
* `ERROR! The server quit without updating PID file...` [Error 해결 방법](https://gmlwjd9405.github.io/2018/12/23/error-mysql-start.html)

3. MySQL 기본 환경 설정 (root 사용자 비밀번호 변경)
* MySQL을 실행시킨 상태(2번 과정)에서 MySQL 접속하기 전에 설정한다.
```bash
$ mysql_secure_installation
```
* 비밀번호 복잡도 검사 과정 (n)
  * 복잡한 비밀번호를 사용하도록 제한해주는 플러그인을 사용하려면 (y)
  * 그냥 쓰던 비밀번호(약한 보안) 제한받지 않고 쓰고 싶다면 (n)
* 루트 비밀번호 입력 & 확인
* 익명 사용자 삭제 (y)
  * $ mysql만으로도 접속이 가능하게 하려면 (n) 
  * 접속 시 -u 옵션을 반드시 명시하려면 (y)
* 원격 접속 허용 (n) 
  * 로컬에서만 실행하는 경우는 (n)
  * localhost외의 ip에서 root 아이디의 접속을 허용하는 경우는 (y)
* test DB 삭제 (n)
* 수정할 것이 있는가? (y or n)
  * 하나라도 권한 변경을 했다면 (y)

4. MySQL 접속 
```bash
$ mysql -uroot -p
```
* 접속 후 위에서 설정한 root 비밀번호를 입력한다.

5. mysql [버전 변경하기](https://gist.github.com/benlinton/d24471729ed6c2ace731)
* mysql 8.0 -> mysql 5.7
```bash
# Unlink current mysql(8.0) version
$ brew unlink mysql 
# Check older mysql(5.7) version
$ ls /usr/local/Cellar/mysql@5.7
# Link the older version
$ brew switch mysql@5.7 5.7.24
```
* mysql 5.7 -> mysql 8.0
```bash
# Unlink older mysql version
$ brew unlink mysql@5.7
# Check current mysql(8.0) version
$ ls /usr/local/Cellar/mysql 
# Link the current version
$ brew switch mysql 8.0.13
```

### 2. 사이트에서 직접 다운로드를 통한 설치 (dmg 파일)
1. MySQL 설치 
* [MySQL 사이트](https://dev.mysql.com/downloads/mysql/)에서 원하는 버전의 .dmg 파일을 다운로드 (로그인 없이도 다운로드 가능)
  * 자세한 내용: [https://dev.mysql.com/ - 버전 5.7 기준](https://dev.mysql.com/doc/mysql-osx-excerpt/5.7/en/osx-installation-pkg.html) 참고
* 중요! 설치할 때 주어지는 <span style="background-color: #e1e1e1">임시 비밀번호 기억</span>
  * 분실했다면 (아래 MySQL 접속 오류 설명 참고)

2. MySQL 실행
```bash
# 실행
$ sudo /usr/local/mysql/support-files/mysql.server start
# 중지
$ sudo /usr/local/mysql/support-files/mysql.server stop
# 재시작 
$ sudo /usr/local/mysql/support-files/mysql.server restart
```
* (6. MySQL alias 설정 참고)

3. MySQL 접속
```bash
# 디렉터리로 이동 후
$ cd /usr/local/mysql/bin/
# MySQL 접속 
$ ./mysql -u root -p
```
* (아래 MySQL 접속 명령어 설명 참고)

4. MySQL 기본 환경 설정 (root 사용자 비밀번호 변경)
* MySQL을 실행시키고 접속한 후(3번 과정)에 설정한다. [참고1](http://0719s.tistory.com/2), [참고2](https://to-dy.tistory.com/58)
```sql
# 둘 중 하나 
mysql> alter user 'root'@'localhost' identified by 'newpassword';
mysql> alter user 'root'@'localhost' identified with mysql_native_password by 'newpassword';
# 적용
mysql> flush privileges;
mysql> exit
```

5. 환경 변수 설정 
* bash 또는 zsh
  * `$ vi ~/.bash_profile` 또는 `$ vi ~/.zshrc`에 아래 내용 추가
* 
```bash
export PATH=/opt/local/bin:/opt/local/sbin:/usr/local/mysql/bin:$PATH
```
* mysql 설치 경로로 이동하지 않아도 어디서나 mysql을 수행할 수 있다.
```bash
# 디렉터리로 이동 후
$ cd /usr/local/mysql/bin/
# MySQL 접속 
$ ./mysql -u root -p
```
```bash
# 환경 변수 설정 후 설치 경로로 이동하지 않고 접속 가능 
$ mysql -uroot -p
```

6. MySQL alias 설정
* bash 또는 zsh
  * `$ vi ~/.bash_profile` 또는 `$ vi ~/.zshrc`에 아래 내용 추가
```bash
alias mysqlserver="sudo /usr/local/mysql/support-files/mysql.server"
```
* alias 설정 후 아래와 같이 사용 가능
  * `mysqlserver start`
  * `mysqlserver stop`
  * `mysqlserver restart`
* alias 설정 시 주의 사항
  * alias 축약이름 = "실행내용" (X) 띄어쓰기 주의!
  * alias 축약이름="실행내용" (O)

### 3. 참고
<mark>참고</mark> MySQL 접속 명령어 
* -u + 사용자명
  * 해당 사용자명으로 접속 (접속할 사용자 지정)
* -p + 비밀번호
  * 비밀번호를 설정한 경우
    1. 해당 사용자의 비밀번호를 -p 뒤에 입력하거나
    2. sudo ./mysql -u root -p 해당 명령어 이후에 입력하거나
* 비밀번호 입력 시 주의사항!
  * **Password:** 형태
    * **맥의 관리자 비밀번호(sudo 패스워드)**이다.
  * **Enter password:** 형태
    * mysql 사용자의 비밀번호
    * root 사용자의 비밀번호를 변경하기 전이면 mysql 설치할 때 받았던 임시 비밀번호

<mark>참고</mark> MySQL 버전 확인
```sql
$ mysql -V
/usr/local/mysql/bin/mysql  Ver 14.14 Distrib 5.7.20, for macos10.12 (x86_64) using  EditLine wrapper
```

---
## MySQL 한글 설정 (UTF-8)
### charset utf-8
1. mysql 접속
```bash
$ mysql -uroot -p
```
2. `mysql> status;` 명령어 입력해서 설정 확인
  * ![](/images/mysql-download/mysql-charset-before.png)
  * latin이 있다면 mysql을 나간 후 터미널을 다시 실행
3. 관리자 권한으로 /etc/my.cnf 파일 생성
```bash
# my.cnf가 기본적으로 존재하지 않으므로 아래 명령어를 통해 새로 생성
$ sudo vi /etc/my.cnf
Password: 
```
4. my.cnf 파일에 아래 내용 넣기 
```bash
[mysqld]
init_connect="SET collation_connection=utf8_general_ci"
init_connect="SET NAMES utf8"
character-set-server=utf8
collation-server=utf8_general_ci
skip-character-set-client-handshake
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
```
5. mysql 서버 정지 후 재실행 
```bash
# alias 설정 후 아래와 같이 사용 가능
$ sudo mysqlserver stop
$ sudo mysqlserver start
```
6. mysql 재접속
7. `mysql> show variables like 'c%';` 명령어를 이용해서 정상적으로 utf-8로 설정되었는지 확인 
  * ![](/images/mysql-download/mysql-charset-after2.png)

---

## [MySQL 삭제](http://dedeweb.tistory.com/30)
* 해당 내용을 모두 shell script에 넣어 한 번에 실행하는 것을 추천한다.

```bash
# brew install mysql로 설치한 경우
$ sudo rm -rf /usr/local/var/mysql
$ sudo rm -rf /usr/local/bin/mysql*
$ sudo rm -rf /usr/local/Cellar/mysql*

# mysql 홈페이지에서 DMG 파일로 설치한 경우
$ sudo rm /usr/local/mysql
$ sudo rm -rf /usr/local/mysql*
$ sudo rm -rf /Library/StartupItems/MySQLCOM
$ sudo rm -rf /Library/PreferencePanes/My*
$ sudo rm -rf ~/Library/PreferencePanes/My*
$ sudo rm -rf /Library/Receipts/mysql*
$ sudo rm -rf /Library/Receipts/MySQL*
$ sudo rm -rf /var/db/receipts/com.mysql.*
# 직접 수행!
# $ vim /etc/hostconfig and removed the line MYSQLCOM=-YES-

# mac osx(10.10 이상)은 아래 내용도 추가 
$ sudo rm -rf /Library/LaunchDaemons/com.microsoft.office.licensing.helper.plist 
$ sudo rm -rf /private/var/db/receipts/*mysql*
```
---

## MySQL 접속 오류
### # 임시 비밀번호 분실한 경우
`ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)`
* **문제 원인:** 
  * 아이디/패스워드가 다르거나 접근 권한이 없을 경우!
* **해결책** 
  * 비밀번호를 알고 있는 경우: 아래의 명령어 입력 후 기억하는 비밀번호로 접속해본다.
```bash
$ /usr/local/mysql/bin/mysql -uroot -p
```
  * 비밀번호 분실한 경우: 아래의 과정 수행 

* 1) MySQL 서버 실행
```bash
$ sudo /usr/local/mysql/support-files/mysql.server start
```
* 2) 터미널에서 접속하기
```bash
$ cd /usr/local/mysql/bin/
$ sudo ./mysql
또는
$ /usr/local/mysql/bin/mysql -uroot
```
  * MySQL 실행은 "/usr/local/mysql/bin/" 디렉터리에서 할 수 있다. 
* 3) 안전모드로 MySQL 데몬(mysqld)을 실행
```bash
$ sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables
```
  * 위와 같이 입력하면 인증을 생략하고 안전모드로 데몬을 실행한다. 
  * 입력할 비밀번호: MySQL 비밀번호가 아닌 **맥의 관리자 비밀번호**이다.
  * 위 명령어는 실행된 상태로 유지되므로 새로운 터미널 창을 열어서 MySQL에 접속한다. 
* 4) MySQL 접속
```bash
$ /usr/local/mysql/bin/mysql -uroot
```
  * 위의 명령어가 제대로 실행되었다면 비밀번호를 묻지 않고 바로 접속이 가능하다.
* 5) 비밀번호 재설정
```sql
mysql> use mysql; 
mysql> update user set authentication_string=password('root') where user='root';
// 마지막으로 변경사항을 적용하기 위해 flush privileges 명령어 실행
mysql> flush privileges;
```
  * 접속한 뒤에는 아래와 같이 비밀번호를 root로 변경한다. 
  * password=(‘root’)에 root 대신 원하는 비밀번호를 입력해도 된다. 
  * (참고로 5.7 버전 이전에는 set password=password(‘원하는 비밀번호’)였는데 컬럼명이 바꼈다.
* 6) MySQL 접속
```bash
$ /usr/local/mysql/bin/mysql -uroot
$ /usr/local/mysql/bin/mysql -uroot -proot
```
  * 다시 MySQL에 접속하면 잘 되는 것을 확인할 수 있다. 
  * 만약 접속이 거부되면 -uroot -proot와 같이 비밀번호까지 붙여서 한번에 실행하는 것을 시도한다.
  * 접속 후 명령을 실행하게 되면 다음과 같은 에러가 발생한다. 
    * `ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.`
* 7) 에러 해결 
```sql
mysql> use mysql; 
mysql> set password = password('원하는 비밀번호');
```
  * 마지막으로 위와 같이 설정하면 정상적으로 MySQL 명령을 실행할 수 있다.

### # 비밀번호가 맞아도 접속이 안되는 경우
`ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)`
* **문제 원인** 
  * MySQL 서버가 켜져 있는지 확인!
* **해결책** 
  ```bash
  $ sudo /usr/local/mysql/support-files/mysql.server start
  ```
  * MySQL 서버 실행 후 다시 접속

---

## MySQL 접속 권한 설정 
* MySQL 서버 실행
```bash
$ sudo /usr/local/mysql/support-files/mysql.server start
```
* 사용자를 추가를 위해 MySQL 서버에 로그인 (root 권한)
```bash
$ mysql -u root -p
Enter password:
```
* 사용자 추가
```sql
# 로컬에서 접속 가능한 사용자
mysql> create user '사용자'@'localhost' identified by '비밀번호';
# 원격에서 접속 가능한 사용자
mysql> create user '사용자'@'원격IP주소' identified by '비밀번호';
```

    <!-- * `mysql ERROR 1819 (HY000): Your password does not satisfy the current policy requirements` 라는 에러가 발생하면 Mysql password policy requirements 에러 validation 제거하여 해결하기 을 참고하자. -->
* 사용자에게 DB 권한 부여하기
```sql
# 모든 DB에 접근 가능하도록 권한 부여
mysql> grant all privileges on *.* to '사용자'@'localhost';
# 특정 DB(DB이름)에만 접근 가능하도록 권한 부여 
mysql> grant all privileges on DB이름.* to '사용자'@'localhost';
```
* 사용자 계정 삭제
```sql
mysql> drop user '사용자'@'localhost';
```

# 관련된 Post
* Windows MySQL 설치방법 등을 알고 싶으시면 [MySQL 설치](https://gmlwjd9405.github.io/2018/05/09/mysql-download.html) 를 참고하시기 바랍니다.
* MySQL Workbench 사용법을 알고 싶으시면 [MySQL Workbench 사용법](https://gmlwjd9405.github.io/2018/05/09/mysql-workbench-guide.html) 을 참고하시기 바랍니다.

# References
> - [권남님의 MySQL 기본 명령어 정리](http://kwonnam.pe.kr/wiki/database/mysql/basic)
> - [http://junho85.pe.kr/1018](http://junho85.pe.kr/1018)
> - [https://junhobaik.github.io/mac-install-mysql/](https://junhobaik.github.io/mac-install-mysql/)
> - [http://googry.tistory.com/31](http://googry.tistory.com/31)
> - [brew를 통해 mysql-version 변경](https://github.com/helloheesu/SecretlyGreatly/wiki/%EB%A7%A5%EC%97%90%EC%84%9C-mysql-%EC%84%A4%EC%B9%98-%ED%9B%84-%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)
> - [루트 비밀번호 변경](https://www.howtoforge.com/setting-changing-resetting-mysql-root-passwords)
> - [charset 설정 참고](http://www.nextstep.co.kr/250)
> - [https://gomdoreepooh.github.io/notes/mysql-reset-password](https://gomdoreepooh.github.io/notes/mysql-reset-password)
> - [http://codingisgame.tistory.com/12](http://codingisgame.tistory.com/12)
> - [MySQL 접속 권한 설정 참고](https://cjh5414.github.io/mysql-create-user)
> - [MySQL 삭제 참고](https://coderwall.com/p/os6woq/uninstall-all-those-broken-versions-of-mysql-and-re-install-it-with-brew-on-mac-mavericks)