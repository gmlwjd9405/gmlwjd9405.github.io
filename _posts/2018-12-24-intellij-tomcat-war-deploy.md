---
layout: post
title: '[IntelliJ] Export/Build War in IntelliJ and Deploy to Tomcat'
subtitle: 'IntelliJ에서 War 파일을 추출하고 외부 Tomcat으로 배포하여 실행할 수 있다.'
date: 2018-12-24
author: heejeong Kwon
cover: '/images/springMVC-project/springMVC-project-setting2-main.png'
tags: setting tomcat war deploy intellij
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - IntelliJ에서 War 파일을 추출하고 빌드할 수 있다.
> - 추출한 War 파일로 Tomcat에 배포할 수 있다.

* 들어가기 전
  * OS: MacOS
  * IDE: IntelliJ (버전: 2018.1.5)
  * Spring, Spring MVC (버전: 5.1.3)
  * tomcat 사전 설치 (버전: 8.5.34)

## IntelliJ에서 War 파일 추출 및 빌드
### 
[]([https://nesoy.github.io/articles/2017-01/Intellij-War](https://nesoy.github.io/articles/2017-01/Intellij-War))


<br><mark>참고</mark> Eclipse에서 War 파일 추출
* Eclipse에서 프로젝트 우클릭 > Export - Web - WAR file 선택 > Next 버튼 클릭
* Destination에 war파일이 생성될 위치 지정 > Finish 버튼 클릭

## 추출한 War 파일 Tomcat에 배포 후 실행 

### 1. 추출한 .war를 Tomcat/webapps 디렉터리 하위로 이동
* 'Tomcat/webapps/' 디렉터리 하위에 해당 <span style="background-color: #e1e1e1">WAR이름.war</span> 파일을 복사한다.
  * 명령어: `cp war파일위치/WAR이름.war tomcat위치/webapps/` 입력 
* (참고) Tomcat 설치 위치
  * macOS: '/usr/local/apache-tomcat-8.5.34'

### 2. 외부 Tomcat 실행한다.
* Linux: Tomcat/bin/startup.sh를 실행
  * macOS 명령어: `/usr/local/apache-tomcat-8.5.34/bin/startup.sh` 입력 
* Windows: Tomcat/bin/startup.bat를 실행

### 3. Tomcat을 실행시키면 패키징되어있던 war 파일의 내용이 풀린다.
* 즉, 'Tomcat/webapps/' 디렉터리 하위에 <span style="background-color: #e1e1e1">WAR이름</span>으로 디렉터리가 생성된다.
  * 해당 디렉터리 안에 풀린 .swar 파일의 내용이 있다.

### 4. url로 접속해서 테스트한다.
* `http://localhost:8080/WAR이름` 로 접속하면 해당 프로젝트를 실행할 수 있다.
* (참고) `http://localhost:8080`으로 접속하고 싶다면 
  * 'Tomcat/webapps/WAR이름/' 디렉터리 하위에 존재하는 파일들을 'Tomcat/webapps/ROOT/' 하위로 옮긴다.


# 관련된 Post
* IntelliJ에서 Gradle 프로젝트 생성 및 SpringMVC를 위한 설정하기에 대해 알고 싶으시면 [IntelliJ에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/10/24/intellij-springmvc-gradle-setting.html)를 참고하시기 바랍니다.
* Tomcat Server를 통해 "Hello World" 띄우기에 대해 알고 싶으시면 [IntelliJ에서 SpringMVC, Tomcat 설정하기](https://gmlwjd9405.github.io/2018/10/25/intellij-springmvc-tomcat-setting.html)를 참고하시기 바랍니다.
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다.


# References
> - [https://nesoy.github.io/articles/2017-01/Intellij-War](https://nesoy.github.io/articles/2017-01/Intellij-War)
> - [http://its-easy.tistory.com/4](http://its-easy.tistory.com/4)
