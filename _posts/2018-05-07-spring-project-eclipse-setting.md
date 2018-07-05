---
layout: post
title: '[Spring, Eclipse] Eclipse에서 Spring MVC 프로젝트 생성하기'
subtitle: 'Eclipse에서 Spring MVC Project 생성하기, pom.xml 설정하기'
date: 2018-05-07
author: heejeong Kwon
cover: '/images/spring-project-eclipse-setting/spring-project-eclipse-setting-main.png'
tags: Spring Eclipse 환경설정
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Eclipse에서 Spring MVC Project 생성할 수 있다.
> - pom.xml의 기본 환경을 설정한다.
> - web.xml과 servlet-context.xml의 기본 설정을 이해한다.
> - @Controller의 의미를 이해한다.



## 1. Spring MVC Project 생성하기
* File > New > Other를 선택한다.
* Spring > Spring Legacy Project를 선택한다.
* 자신의 프로젝트 이름('eStore')을 입력하고, Spring MVC Project를 선택한다.
![](/images/spring-project-eclipse-setting/1.png)

* 프로젝트 top-level package를 설정한다.
  * 최소 3레벨 이상 ( [1레벨].[2레벨].[3레벨] )으로 구성한다.
  * EX) com.mycompany.myapp
![](/images/spring-project-eclipse-setting/2.png)

* Finish를 누른다.
* 프로젝트가 생성되면서 Spring Project에 필요한 Library들을 자동으로 다운받는다.
* 다운로드가 완료된 후 pom.xml 설정단계로 넘어간다. (에러가 있어도 밑의 과정을 따라가보자.)




## 2. pom.xml 설정하기
### 1) pom.xml의 기본적인 설정
* pom.xml를 더블클릭한 후 'Overview'의 Artifact를 확인한다.
  * pom.xml의 groupId, artifactId, version 세가지가 프로젝트의 좌표를 나타내므로 <span style="color:#4d0000">**반드시!!**</span> 지정해야 한다.
  * Version의 종류
    * snapshot: 개발용
    * release: 배포용(개발이 끝난 후)
![](/images/spring-project-eclipse-setting/pomxml-overview-artifact.png){: width="260" height="250"}

~~~javascript
<groupId>kr.ac.hansung.cse</groupId>
<artifactId>eStore</artifactId>
<version>1.0.0-BUILD-SNAPSHOT</version>
~~~

### 2) 초기 버전 설정
* java-version과 springframework-version을 설정한다.
![](/images/spring-project-eclipse-setting/pomxml-overview-properties.png){: width="260" height="250"}
  * **java-version**: 자신이 설치한 자바 버전에 맞게 수정한다.
    * (해당 프로젝트에서는 1.8로 진행한다.)
    * *자바 버전 확인 방법:* * Package Explorer의 JRE System Library 옆을 확인한다.
      <!-- * <div class="pull-left"><img src="/images/spring-project-eclipse-setting/java-version-check.png"></div> -->
      * ![](/images/spring-project-eclipse-setting/java-version-check.png){: width="200" height="20"}
      * ※ 만약 Package Explorer가 보이지 않는다면, **1)** Window > Open View > Other를 선택하고 **2)** Package Explorer를 검색하여 추가한다.
    * pom.xml를 더블클릭한 후 'Overview'의 Properties를 확인한다.
    * java-version 선택 후 1.8로 변경
    * ![](/images/spring-project-eclipse-setting/4.png)
  * **springframework-version**: 스프링 프레임워크 사이트에서 버전을 확인 후 수정한다.
    * (해당 프로젝트에서는 4.2.5로 진행한다.)
    * *스프링 프레임워크 버전 확인 방법:* [스프링 프레임워크 사이트](http://projects.spring.io/spring-framework/)의 버전을 확인한다.
      * ![](/images/spring-project-eclipse-setting/springframework-version-check2.png){: width="400" height="250"}
    * pom.xml를 더블클릭한 후 'Overview'의 Properties를 확인한다.
    * org.springframework-version 선택 후 4.2.5로 변경
    * ![](/images/spring-project-eclipse-setting/5.png)

* 추가 설정
  * 1) Java Resources > Libraries > JRE System Library > 우클릭 > Properties > Execution environment 선택
    * JavaSE-1.8(jre 1.8.0_73)로 변경
    * (해당 프로젝트에서는 1.8로 진행하기 때문)
    * ![](/images/spring-project-eclipse-setting/add-setting1.png)
  * 2) 프로젝트 우클릭 > Properties > Project Facets 선택
    * Dynamic Web Module: 3.1로 변경
    * Java: 1.8로 변경
    <!-- * ![](/images/spring-project-eclipse-setting/add-setting2.png) -->
  * 3) Web Project Settings > **Context root** 를 프로젝트명('eStore')으로 변경
    * URL을 줄 때 프로젝트명('eStore')으로 주기 위해서

* 이제, 생성된 프로젝트에 에러가 없는지 확인한다.

---
<mark>Maven 프로젝트에서 필요한 라이브러리 추가</mark>  
* Maven 프로젝트의 pom.xml를 더블클릭한 후 아래 쪽의 'pom.xml' 탭을 눌러 내용을 확인한다.
* 'dependency 태그': 하나의 라이브러리를 의미한다.
* Dependencies로 추가
  * 만약 새로운 라이브러리를 추가하고 싶으면, 'dependency 태그'를 추가함으로써, 새로운 라이브러리를 추가할 수 있다.

  ~~~javascript
  <dependency> </dependency>
  ~~~
  * ![](/images/spring-project-eclipse-setting/dependency-example.png){: width="300" height="150"}



## 3. web.xml과 servlet-context.xml의 기본 설정 이해하기
### web.xml 이해하기
* src > main > webapp > **/WEB-INF/web.xml**
![](/images/spring-project-eclipse-setting/webxml-contents.png){: width="550" height="230"}
* DispatcherServlet
~~~javascript
  <!-- Processes application requests -->
    <servlet>
       <servlet-name>appServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
          <init-param>
              <param-name>contextConfigLocation</param-name>
              <param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
          </init-param>
          <load-on-startup>1</load-on-startup>
  </servlet>
~~~
  * DispatcherServlet: Client로부터 웹 요청을 받아들이는 부분
  * /WEB-INF/spring/appServlet/servlet-context.xml: servlet-context.xml에 설정파일이 있다.

* servlet-mapping
~~~javascript
  <servlet-mapping>
       <servlet-name>appServlet</servlet-name>
       <url-pattern>/</url-pattern>
  </servlet-mapping>
~~~
  * / : 루트로 오는 모든 요청을 DispatcherServlet이 받아서 적절한 controller에 넘긴다.


### servlet-context.xml 이해하기
* src > main > webapp > **/WEB-INF/spring/appServlet/servlet-context.xml**
![](/images/spring-project-eclipse-setting/servletcontentxml-contents.png){: width="700" height="180"}
* annotation-driven
~~~javascript
  <!-- Enables the Spring MVC @Controller programming model -->
  <annotation-driven />
~~~
  * annotation을 사용하겠다는 것을 enable(활성화) 해줘야 한다.

* resources
~~~javascript
  <!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
  <resources mapping="/resources/**" location="/resources/"/>
~~~
  * resource에 해당하는 것은 DispatcherServlet을 거칠 필요가 없다.
  * 이런 URL(/resources/** )로 오는 것은 location에 있다는 것을 명시한다. (client로 바로 전달해라.)

* view의 logical name
~~~javascript
  <!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
  <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <beans:property name="prefix" value="/WEB-INF/views/" />
      <beans:property name="suffix" value=".jsp" />
  </beans:bean>
~~~
  * view의 logical name(Controller에서 return한 string값)을 prefix 와 suffix 를 이용하여 pysical object로 바꿔준다.
  * 즉, logical name -> pysical object

* base-package 설정 확인
~~~javascript
  <context:component-scan base-package="kr.ac.hansung" />
~~~
  * 만약 base-package가 처음 설정과 동일하지 않으면 변경한다.
  * src/main/java의 kr.ac.hansung 패키지 우클릭 > refactor > rename을 선택
    * kr.ac.hansung.cse 로 변경

* component-scan
~~~javascript
  <context:component-scan base-package="kr.ac.hansung.cse" />
~~~
  * 이 패키지(kr.ac.hansung.cse )에 있는 component들을 scan한다.
    * @Controller
    * @Service
    * @Repository
    * 에 해당하는 클래스들을 spring ioc container에 **bean으로 등록한다.**
  * @Controller, @Service, @Repository의 차이
    * 세 개 모두 특수화된 @Component이다. 기능적 차이는 거의 없다.
    * 하나의 약속처럼 컨트롤러는 @Controller, 서비스는 @Service, DAO(DB에 접근하는 클래스)는 @Repository로 선언한다.


## 4. @Controller의 의미를 이해한다.
* @Controller
![](/images/spring-project-eclipse-setting/controller-example.png){: width="400" height="250"}
  * @RequestMapping(value = "/", method = RequestMethod.GET)
    * "/" 로 오는 것은 이 Method에 Mapping시킨다.
    * 이 메서드에서 return 한 string인 "home"에 의해 view인 "home.jsp"가 수행된다. (logical name -> pysical object)


<!--
# 관련된 Post
* Bootstrap의 개념과 설치방법 등을 알고 싶으시면 [Bootstrap 설치 및 환경설정](https://gmlwjd9405.github.io/2018/05/02/bootstrap-download-and-setting.html) 를 참고하시기 바랍니다. -->


# References
> - [http://addio3305.tistory.com/36](http://addio3305.tistory.com/36)
> - [http://kamang-it.tistory.com/100](http://kamang-it.tistory.com/100)
> - [http://all-record.tistory.com/156](http://all-record.tistory.com/156)
> - [https://code.i-harness.com/ko/q/682ee8](https://code.i-harness.com/ko/q/682ee8)
