---
layout: post
title: '[SpringMVC] IntelliJ에서 SpringMVC, Gradle 설정'
subtitle: 'IntelliJ에서 Gradle 프로젝트를 생성하고 SpringMVC를 위한 설정을 할 수 있다.'
date: 2018-10-24
author: heejeong Kwon
cover: '/images/springMVC-project/springMVC-project-setting1-main.png'
tags: springMVC project example setting gradle intellij
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - IntelliJ에서 Gradle 프로젝트를 생성할 수 있다.
> - IntelliJ에서 SpringMVC를 위한 설정을 할 수 있다.
> - git과 연동 시 .gitignore을 설정한다.

* 들어가기 전
  * OS: MacOS
  * IDE: IntelliJ (버전: 2018.1.5)
  * Spring, Spring MVC (버전: 4.3.18)
  * gradle, java 사전 설치 
  <!-- * tomcat 사전 설치  -->

## IntelliJ에서 Gradle 프로젝트 생성 
1. Create New Project
* ![](/images/springMVC-project/setting1/setting1-1.png)
2. Gradle Tab에서 **Java** 선택 후 Next
* ![](/images/springMVC-project/setting1/setting1-2.png)
3. GroupId, ArtifactId, Version 설정 후 Next
* ![](/images/springMVC-project/setting1/setting1-3.png)
* 자신의 프로젝트에 대한 고유의 좌표를 설정하는 것이다.
* GroupId? 
  * 자신의 프로젝트를 고유하게 식별하게 해 주는 것으로, 최소한 내가 컨트롤하는 domain name이어야 한다.
  * package 명명 규칙을 따른다.
  * 하위 그룹은 얼마든지 추가할 수 있다.
* ArtifactId?
  * 제품의 이름으로, 버전 정보를 생략한 jar 파일의 이름이다.
  * 프로젝트 이름과 동일하게 설정한다.
  * 소문자로만 작성하며 특수문자는 사용하지 않는다.
* Version?
  * SNAPSHOT: 개발용, RELEASE: 배포용 
  * 숫자와 점을 사용하여 버전 형태를 표현한다.(1.0, 1.1, 1.0.1, …)
4. 본인이 원하는 설정 후 Next (위와 같이 시작해도 됨)
* ![](/images/springMVC-project/setting1/setting1-4.png)
* Use auto-import 선택
* Create separate module per source set 선택 제거 
5. 프로젝트 이름과 저장할 위치 선택 후 Finish
* ![](/images/springMVC-project/setting1/setting1-5.png)
6. 생성이 완료되면 아래와 같은 프로젝트 구조 생성
* ![](/images/springMVC-project/setting1/setting1-6.png)
7. main 코드와 test 코드를 생성하기 위한 source root 필요 (기본 디렉터리가 자동으로 생성 되지 않는 경우)
* **자동 설정**
  * 1) Preferences에서 Build, Execution, Deployment Tab 선택
  * 2) Build Tools - Gradle에서 <br> -> **Create directories for empty content roots automatically** 체크 후 Apply
  * ![](/images/springMVC-project/setting1/setting1-7-1.png)
  * IntelliJ의 2018.01 이후 버전부터는 빈 내용인 root directory를 자동으로 만들지 않는다.
  <br> -> [참고 내용](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206154199-No-default-folders-created-in-new-gradle-project)
* **수동 설정**
  * 1) 프로젝트 우클릭 <br> -> Directory 선택 후 `src/main/java` 와 `src/test/java` 입력하여 디렉터리 생성 
  * 2) main의 source root는 파란색, test의 source root는 초록색으로 표기된다. <br> 만약 회색(일반 디렉터리)으로 표기 된다면 아래의 과정을 이어서 진행한다. 
  * 3) `src/main/java` 우클릭 후 Mark Directory as -> Sources Root 선택
  * 4) `src/main/resources` 우클릭 후 Mark Directory as -> Resources Root 선택
  * 5) `src/test/java` 우클릭 후 Mark Directory as -> Test Sources Root 선택
  * 6) `src/test/resources` 우클릭 후 Mark Directory as -> Test Resources Root 선택
  * ![](/images/springMVC-project/setting1/setting1-7-2.png)
8. Gradle 프로젝트 생성 완료 후 아래와 같은 프로젝트 구조 완성
* ![](/images/springMVC-project/setting1/setting1-8.png)

### 간단한 테스트 코드 작성 
1. `src/test/java` 에 패키지 및 테스트 클래스 생성
* ![](/images/springMVC-project/setting1/simple-testcode-1.png)
* main에도 동일한 패키지를 생성한다.
2. test 코드 작성 
* 아직 main에 생성되지 않은 Class를 사용하기 때문에 빨간 경고창이 출력된다.
* 이때, 해당 표기 위에 커서를 댄 상태에서 **option + enter**를 누르면 해당 Class를 생성할 수 있다.
* ![](/images/springMVC-project/setting1/simple-testcode-2.png)
* 클래스 생성 위치를 main으로 변경한 후 Class를 생성한다.
* ![](/images/springMVC-project/setting1/simple-testcode-3.png)
* 관련 클래스 자동 import
  * ![](/images/springMVC-project/setting1/simple-testcode-4.png)
  * 마찬가지로 빨간 경고창이 뜨는 위치에 커서를 댄 상태에서 **option + enter**를 누르면 static import가 가능하다.
  * `import static org.junit.Assert.assertThat;`
  * `import static org.hamcrest.CoreMatchers.is;`
3. main 코드 작성 
* 테스트 코드에 맞춰 메인 코드를 생성한다.

### 테스트 코드의 비교 종류
* <span style="background-color: #e1e1e1">assertEquals, assertTrue, assertThat</span> 중 선택

1. assertEquals
* assertEquals(new Integer(2), game.getLeastTryCount());
* 첫 번째 인자: 기대값, 두 번째 인자: 실제값
* 인자의 순서가 바뀌어도 테스트 목적에는 별 다른 지장을 주지 않는다.
2. assertThat **(추천)**
* assertThat(game.getLeastTryCount(), is(2));
* is()와 같은 method들
  * JUnit이 처음으로 의존성을 가지고 사용하는 제 3자('Hamcrest'라는 프로젝트)의 클래스들
  * 'Hamcrest'는 Matcher를 확장한 다양한 메소드들을 제공
* 사용하는 method에 대한 static import 필요 
  * `import static org.hamcrest.CoreMatchers.*;` 추가 
* 장점
  * assertEquals() 보다 가독성 높은 에러 메시지
  * Custom Matcher 사용 가능 
  * 콤비네이션이 가능 (예시와 같은 조합이 가능)  Ex. is(not(3)), eather(2).or(3) 

## SpringMVC 설정을 위한 Framework Support 추가 
1. 프로젝트 우클릭 -> Add Framework Support 클릭 
  * ![](/images/springMVC-project/setting1/setting2-1.png)
2. Java EE - Web Application 선택 후 아래로 드래그 
  * ![](/images/springMVC-project/setting1/setting2-2.png)
3. Spring - Spring MVC(ver. 4.3.18) 선택 후 OK
  * ![](/images/springMVC-project/setting1/setting2-3.png)
4. 설정이 완료되면 아래와 같은 프로젝트 구조 생성
  * ![](/images/springMVC-project/setting1/setting2-4.png)

### Tomcat Server를 통해 "Hello World" 띄우기 
* [https://gmlwjd9405.github.io/2018/10/25/intellij-springmvc-tomcat-setting.html](https://gmlwjd9405.github.io/2018/10/25/intellij-springmvc-tomcat-setting.html) 참고


## git과 연동 시 .gitignore 내용
```code
### JAVA ###
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*


### Gradle ###
.DS_Store
.gradle
/build/
/out/
/target/

# Ignore Gradle GUI config
gradle-app.setting

# Avoid ignoring Gradle wrapper jar file (.jar files are usually ignored)
!gradle-wrapper.jar
!gradle/wrapper/gradle-wrapper.jar

# Cache of project
.gradletasknamecache

# # Work around https://youtrack.jetbrains.com/issue/IDEA-116898
# # gradle/wrapper/gradle-wrapper.properties


### STS ###
.apt_generated
.classpath
.factorypath
.project
.settings
.springBeans
bin/


### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr
```

# 관련된 Post
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다.
* Tomcat Server를 통해 "Hello World" 띄우기에 대해 알고 싶으시면 [IntelliJ에서 SpringMVC, Tomcat 설정하기](https://gmlwjd9405.github.io/2018/10/25/intellij-springmvc-tomcat-setting.html)를 참고하시기 바랍니다.

# References
> - [https://johngrib.github.io/wiki/groupId-artifactId/](https://johngrib.github.io/wiki/groupId-artifactId/)
> - [https://jojoldu.tistory.com/138](https://jojoldu.tistory.com/138)
> - [http://syung05.tistory.com/26](http://syung05.tistory.com/26)
> - [http://whiteship.tistory.com/1739](http://whiteship.tistory.com/1739)