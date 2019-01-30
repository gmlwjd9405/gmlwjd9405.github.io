---
layout: post
title: '[IntelliJ] Export War in IntelliJ and Deploy to Tomcat'
subtitle: 'IntelliJ에서 War 파일을 추출하고 외부 Tomcat으로 배포하여 실행할 수 있다.'
date: 2018-12-24
author: heejeong Kwon
cover: '/images/springMVC-project/springMVC-project-setting3-main.png'
tags: setting tomcat war export deploy intellij
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - IntelliJ에서 War 파일을 추출할 수 있다.
> - 추출한 War 파일로 Tomcat에 배포할 수 있다.

* 들어가기 전
  * OS: MacOS
  * IDE: IntelliJ (버전: 2018.1.5)
  * Spring, Spring MVC (버전: 5.1.3)
  * tomcat 사전 설치 (버전: 8.5.34)

## 기본적인 용어
* **빌드(Build)**
  * 소스코드 파일을 실행 가능한 소프트웨어 산출물로 만드는 일련의 과정을 말한다.
* Maven에서 **Artifact**
  * Maven Build의 결과로 얻을 수 있는 일반적인 .jar나 .war 또는 여타의 실행 파일을 의미한다. 
  * 즉, 빌드로 생성되는 프로젝트의 결과물
* **배포(Deploy)**
  * 응용 프로그램을 서버 상에서 활용할 수 있도록 구동시키는 것을 의미한다.
  * 즉, 실행 가능한 파일을 서버에 올려 사용자가 이용할 수 있게 하는 것을 의미한다.
  
<mark>참고</mark> 웹 애플리케이션을 배포하기 위한 패키징 유형(**3가지**)
1. package(archive)
- 아카이브(.war, .ear) 파일로 배포
  - 아카이브는 WAS(Tomcat)에 의해 압축이 풀린다.
  - 파일이 많은 경우 압축을 푸는 시간이 오래 걸릴 수 있다.
  - 원격 서버에 배포시 한 개의 파일만 전송하면 된다.
  - WAS(Tomcat)에서 제공하는 업로드를 통한 배포 기능을 활용할 수 있다.
2. exploded(expanded)
- 아카이브를 압축 해제한 형태의 디렉터리로 배포 
  - 별도의 디렉터리에 원본 소스를 복사하여 만든다. 
  - 압축 및 해제 과정이 불필요하다.
  - 파일이 많은 경우 복사하는 시간이 오래 걸릴 수 있다.
  - 원본 소스를 건드리지 않고 배포를 원하는 경우에 적합하다.
  - 원격 서버에 배포시 파일이 많은 경우 전송 시간이 오래 걸릴 수 있다.
3. in-place
- 소스 디렉터리(전체 또는 일부)를 그대로 배포
  - 추가적인 복사 과정 불필요하다.
  - 로컬 서버에 배포하는 경우에 적합하다.
  - WAS(Tomcat)가 런타임시 생성하는 파일이 소스와 섞일 수 있는 문제가 있다.

---

# IntelliJ에서 War 파일 추출 및 빌드

### 1. Project Structure > Project Settings > Artifacts
1. File - Project Structure 선택 또는 단축키 이용
* MacOS: (cmd + ;), Windows: (Ctrl + Alt + Shift + S) 이용 
2. Project Setting의 Artifacts 선택
* Build Artifact: 빌드의 결과물로 생성할 형태를 정할 수 있다.
  - archive: 아카이브(.war, .ear) 파일로 배포
  - exploded: 아카이브를 압축 해제한 형태의 디렉터리로 배포 

![](/images/springMVC-project/setting3/setting3-1.png)

### 2. War 파일 추출을 위한 Build Artifact 설정 
1. 상단에 + 클릭 > Web Application: Archive > For 'PROJECT_NAME:war exploded' 선택 
  * 빌드의 결과물로 'PROJECT_NAME:war exploded'에 대한 war 파일을 생성하도록 설정한다.
  * 'PROJECT_NAME:war exploded'가 기본 설정 
    <!-- * `out/artifacts/PROJECT_NAME_war_exploded` 디렉터리 -->
  * ![](/images/springMVC-project/setting3/setting3-2.png) 
2. PROJECT_NAME_war.war ---> PROJECT_NAME.war로 수정 
  * ![](/images/springMVC-project/setting3/setting3-3.png) 

### 3. War 파일 추출을 위한 Build 
1. Build - Build Artifacts 클릭
2. 'PROJECT_NAME:war' 선택 > build 클릭 
  * ![](/images/springMVC-project/setting3/setting3-4.png)
  * 빌드의 결과물로 'PROJECT_NAME:war exploded'에 대한 war 파일이 생성된다.
  
### 4. 추출한 War 파일 확인 
* out/artifacts/PROJECT_NAME_war/PROJECT_NAME.war 생성
![](/images/springMVC-project/setting3/setting3-5.png)


<mark>참고</mark> 빌드 결과물이 위치할 폴더  
* out/
  * IntellJ 프로젝트 출력 폴더
  * IntellJ IDE 프로젝트에서 모든 모듈은 표준 출력 폴더 (out/classes/production/module_name)를 사용한다.
  * 예를 들어, IntellJ에서 프로젝트를 생성하고 Run을 하면 
    * 1) 프로젝트(work space) 상단에 **"out"**이라는 디렉터리가 생성된다.
    * 2) 해당 디렉터리 안에 컴파일된 클래스 파일이 들어간다. (src와 비슷한 계층으로 구성)
  * *IntellJ*와 관련된 프로젝트의 .gitignore에 해당 내용을 추가한다.
* target/
  * Java 기반 프로젝트의 라이프사이클 관리를 목적으로 하는 빌드 도구인 Maven의 빌드 결과물 저장 폴더 
  * Maven으로 빌드하면 **"target"**이라는 디렉터리가 생성된다.
    * .jar 파일을 저장하는 것이 주용도
    * 그 외에도 빌드된 파일들을 자동으로 넣어준다.
  * 나중에 프로젝트 결과물(jar 또는 war)을 실서버에 반영할 때 target 밑에 있는 내용을 배포하게 된다.
    * 개발할 때는 이클립스 안에서 모든 것이 이루어지기 때문에 별로 중요성이 없지만
  * *Maven*과 관련된 프로젝트의 .gitignore에  해당 내용을 추가한다.

<br><mark>참고</mark> Eclipse에서 War 파일 추출
* Eclipse에서 프로젝트 우클릭 > Export - Web - WAR file 선택 > Next 버튼 클릭
* Destination에 war파일이 생성될 위치 지정 > Finish 버튼 클릭

---

# 추출한 War 파일 Tomcat에 배포 후 실행 

### 1. 추출한 .war를 Tomcat/webapps 디렉터리 하위로 이동
* 'Tomcat/webapps/' 디렉터리 하위에 해당 <span style="background-color: #e1e1e1">WAR_NAME.war</span> 파일을 복사한다.
  * 명령어: `cp war파일위치/WAR_NAME.war tomcat위치/webapps/` 입력 
* (참고) Tomcat 설치 위치
  * macOS: '/usr/local/apache-tomcat-8.5.34'

### 2. 외부 Tomcat을 실행
* Linux: Tomcat/bin/startup.sh를 실행
  * macOS 명령어: `/usr/local/apache-tomcat-8.5.34/bin/startup.sh` 입력 
* Windows: Tomcat/bin/startup.bat를 실행

### 3. Tomcat을 실행시키면 패키징되어있던 war 파일 압축 해제
* 즉, 'Tomcat/webapps/' 디렉터리 하위에 <span style="background-color: #e1e1e1">WAR_NAME</span>으로 디렉터리가 생성된다.
  * 해당 디렉터리 안에 풀린 .war 파일의 내용이 있다.

### 4. url로 접속해서 테스트
* `http://localhost:8080/WAR_NAME` 로 접속하면 해당 프로젝트를 실행할 수 있다.
* (참고) `http://localhost:8080`으로 접속하고 싶다면 
  * 'Tomcat/webapps/WAR_NAME/' 디렉터리 하위에 존재하는 파일들을 'Tomcat/webapps/ROOT/' 하위로 옮긴다.

### Tomcat 배포 과정 요약 
1. Tomcat/webapps/ 디렉터리 하위에 WAR_NAME.war 이동 
2. Tomcat Server 실행 -> WAR_NAME.war 압축 해제 
3. Tomcat/webapps/WAR_NAME 디렉터리 생성 
4. `http://localhost:8080/WAR_NAME/`로 접속 가능 


# 관련된 Post
* IntelliJ에서 Gradle 프로젝트 생성 및 SpringMVC를 위한 설정하기에 대해 알고 싶으시면 [IntelliJ에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/10/24/intellij-springmvc-gradle-setting.html)를 참고하시기 바랍니다.
* Tomcat Server를 통해 "Hello World" 띄우기에 대해 알고 싶으시면 [IntelliJ에서 SpringMVC, Tomcat 설정하기](https://gmlwjd9405.github.io/2018/10/25/intellij-springmvc-tomcat-setting.html)를 참고하시기 바랍니다.
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다.


# References
> - [https://nesoy.github.io/articles/2017-01/Intellij-War](https://nesoy.github.io/articles/2017-01/Intellij-War)
> - [http://its-easy.tistory.com/4](http://its-easy.tistory.com/4)
> - [http://ecogeo.tistory.com/tag/war:exploded](http://ecogeo.tistory.com/tag/war:exploded)

