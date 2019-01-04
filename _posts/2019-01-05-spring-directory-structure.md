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

**참고용!** <mark>내가 사용하는 프로젝트 구조이므로, 설정 방법에 따라 구조가 달라질 수 있다.</mark>

---

## Spring MVC Structure
### ㄴsrc/main/java
- class(Servlet)
    - controller package
    - service package
    - dao package
    - model package

### ㄴsrc/main/resources
- logback.xml

### ㄴweb (또는 webapp)
- resources or static DIR
    - css, images, fonts, js..
- WEB-INF 
    - props or database DIR
        - jdbc.properties
    - views 
        - jsp or html 

        <!-- - templates DIR
            - header / footer / layout -->

    - web.xml
    - XXXconfig.xml
        - applicationContext.xml
        - dispatcher-servlet.xml
        - serviceContext.xml
        - daoContext.xml
        - securityContext.xml

### ㄴpom.xml

<mark>참고</mark>
* dispatcher-servlet.xml
    * `<mvc:resources mapping="/resources/**" location="/resources/" />`
    * web/resources/
    * <span style="background-color: #e1e1e1">**/**</span>는 "web/" location을 의미한다.
* applicationContext.xml
    * `<context:property-placeholder location="classpath:props/jdbc.properties"/>`
    * web/WEB-INF/props/jdbc.properties
    * <span style="background-color: #e1e1e1">**classpath:**</span>는 "web/WEB-INF/" location을 의미한다.

---

## Spring Boot (WEB-INF 없음)
### ㄴsrc/main/java
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
- static 
    - css, images, fonts, js..
- templates 
    - includes DIR
        - header / footer / layout
    - jsp or html
- application.properties or application.yml
- import.sql
- logback.xml	


# 관련된 Post
* MVC Architecture에 대해 알고 싶으시면 [MVC Architecture 이해하기](https://gmlwjd9405.github.io/2018/11/05/mvc-architecture.html)를 참고하시기 바랍니다.
* Spring MVC Framework에 대해 알고 싶으시면 [Spring MVC 이해하기](http://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html)를 참고하시기 바랍니다.


<!-- # References
> - []() -->