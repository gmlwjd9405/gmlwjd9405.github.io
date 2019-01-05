---
layout: post
title: '[Spring] Spring MVC and Spring Boot Structure'
subtitle: 'Spring MVC와 Spring Boot의 디렉터리 구조를 이해한다.'
date: 2019-01-05
author: heejeong Kwon
cover: '/images/spring-framework/spring-framework-main.png'
tags: springMVC spring springBoot structure web
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - Spring MVC Structure를 이해한다.
> - Spring Boot Structure를 이해한다.

**참고용!** <mark>'내가' 사용하는 프로젝트 구조이므로, 개발자마다 설정 방법에 따라 구조가 달라질 수 있다.</mark>

---

## Spring MVC Structure
### ㄴsrc/main/java
: 자바 소스 파일
- class(Servlet)
    - controller package
    - service package
    - dao package
    - model package

### ㄴsrc/main/resources
: 자바 소스 파일에서 사용하는 리소스 파일(mapper, sql, logging 등 설정 파일)
- logback.xml

### ㄴweb (또는 webapp)
: Web에서 사용하는 리소스 파일(static files, templates, xml, properties 등)
- static or ~~resources~~ DIR
    - css, images, fonts, js..
- WEB-INF 
    - props or database DIR
        - jdbc.properties
    - views 
        - includes DIR
            - header / footer / layout
        - jsp or html 
    - web.xml
    - XXXconfig.xml
        - applicationContext.xml
        - dispatcher-servlet.xml
        - serviceContext.xml
        - daoContext.xml
        - securityContext.xml

### ㄴpom.xml
: Maven 프로젝트 설정 파일
- 프로젝트 기본 정보
- 빌드 설정
- 프로젝트 관계 설정
- 빌드 환경
- Property 관리

<mark>참고</mark>
* /WEB-INF 디렉토리 이하는 보안상의 문제로 웹 브라우저를 통한 접근을 금지하고 있다. 
    * 하지만 포워딩을 통한 접근은 웹 브라우저를 통하지 않기 때문에 가능하다.
    * 컴파일된 클래스, 스프링 환경 설정 파일(DB 연결 정보), jsp 등 외부에서의 수정 방지
* 정적 자원들은 /static 디렉터리 사용을 권장한다.
* dispatcher-servlet.xml
    * `<mvc:resources mapping="/static/**" location="/static/" />`
    * web/static/
    * <span style="background-color: #e1e1e1">**/**</span>는 "web/" location을 의미한다.
* applicationContext.xml
    * `<context:property-placeholder location="classpath:props/jdbc.properties"/>`
    * web/WEB-INF/props/jdbc.properties
    * <span style="background-color: #e1e1e1">**classpath:**</span>는 "web/WEB-INF/" location을 의미한다.

---

## Spring Boot 
### ㄴsrc/main/java
: 자바 소스 파일
- class(Servlet)
    - web package (controller)
    - service package
    - repository package (dao)
    - domain package (model) 
    - dto package
    - config package

<!-- - security package
- validation package
- common package -->

### ㄴsrc/main/resources
: 배포할 리소스 파일(static files, templates, xml, properties 등)
- static 
    - css, images, fonts, js..
- templates 
    - includes DIR
        - header / footer / layout
    - dynamic HTML
- application-{profile}.properties or application.yml
- import.sql
- logback.xml	

<mark>참고</mark>
* Spring Boot에서는 JSP 보다는 템플릿 엔진의 사용을 권장한다.
    * [템플릿 엔진의 개념과 종류 및 필요성](https://gmlwjd9405.github.io/2018/12/21/template-engine.html) 참고 
    * JSP 사용을 위해서는 추가 설정이 필요하다.
<!-- * 기본적으로 Spring Boot는 <span style="background-color: #e1e1e1">**classpath**<span>에 있는 /static(/public, /resources, /META-INF/resources) 디렉터리 또는 ServletContext의 루트로부터 정적 콘텐트를 서비스한다. -->
* Spring Boot는 기본적으로 src/main/resources/ 디렉터리를 <span style="background-color: #e1e1e1">**classpath**</span>로 가지고 있다.
* Spring Boot에서는 <span style="background-color: #e1e1e1">**classpath**</span> 상에서 /static, /resources, /public, /META-INF/resources/ 경로를 기본으로 탐색한다. 
    * [https://docs.spring.io](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-developing-web-applications.html#boot-features-spring-mvc-static-content) 참고 
    * /WEB-INF/resources의 경우, jar 파일로 배포할 경우에는 인식하지 않기 때문에 사용하지 않도록 주의한다.
    * 정적 자원들은 /static 디렉터리 사용을 권장한다.
* Spring Boot에서는 기존의 전통적인 웹 어플리케이션 방식에서 필수로 관리되어야 하는 Tomcat 설정 및 web.xml 파일 등은 스프링부트의 내부 모듈에 의해서 구동시 자동으로 설정된다. <br> **(default로 web/ 디렉터리가 없음)** 


# 관련된 Post
* MVC Architecture에 대해 알고 싶으시면 [MVC Architecture 이해하기](https://gmlwjd9405.github.io/2018/11/05/mvc-architecture.html)를 참고하시기 바랍니다.
* Spring MVC Framework에 대해 알고 싶으시면 [Spring MVC 이해하기](http://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html)를 참고하시기 바랍니다.


# References
> - [https://gist.github.com/ihoneymon/a343e2f4a0299988206e](https://gist.github.com/ihoneymon/a343e2f4a0299988206e)
> - [http://lazyrodi.github.io/2017/09/03/2017-09-03-spring-structure/](http://lazyrodi.github.io/2017/09/03/2017-09-03-spring-structure/)