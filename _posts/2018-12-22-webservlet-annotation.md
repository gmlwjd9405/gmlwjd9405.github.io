---
layout: post
title: '[Annotation] @WebServlet'
subtitle: '@WebServlet Annotation의 개념을 이해하다.'
date: 2018-12-22
author: heejeong Kwon
cover: '/images/spring-framework/spring-annotation-main.png'
tags: spring annotation webservlet servlet
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## @WebServlet 이란
* @WebServlet의 속성 값을 통해 해당 Servlet과 매핑될 URL 패턴을 지정한다.
* 이 Annotation을 통해 web.xml 파일에 별로의 설정을 하지 않더라도 해당 Servlet을 실행할 수 있다.
  ```xml
  <Servlet>
    <Servlet-name>HelloWorld</Servlet-name>
    <Servlet-class>HelloWorld</Servlet-class>
  </Servlet>
  <Servlet-mapping>
    <Servlet-name>HelloWorld</Servlet-name>
    <url-pattern>/Hello</url-pattern>
  </Servlet-mapping>
  ```

### @WebServlet의 속성들
* **name**
    * 서블릿의 이름을 설정하는 속성 
    * @WebServlet(name = "서블릿 이름")
* **urlPatterns**
    * 서블릿의 URL 목록을 설정하는 속성  
    * @WebServlet(urlPatterns = "/")
    * @WebServlet(urlPatterns = {"/"})
    * @WebServlet(urlPatterns = {"/", "/home", "/webcome"})
* **value**
    * urlPatterns와 동일한 기능을 한다.
    * value는 속성 이름 없이 값만 설정할 수 있다. 
    * @WebServlet("/calc")
    * value 속성 외에 다른 속성의 값도 필요한 경우는 속성의 이름을 생략할 수 없다.

### @WebServlet import error
`javax.servlet.annotation.WebServlet을 가져올 수 없습니다.`
* servlet-api.jar의 버전이 3.0 이상인지 확인
* @WebServlet 사용을 위한 설정 추가
    * [https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api/3.0.1](https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api/3.0.1)

* Maven
  ```xml
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</>
    <version>3.0.1</version>
  </>
  ```

* Gradle
  ```xml
  // https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api
  provided group: 'javax.servlet', name: 'javax.servlet-api', version: '3.0.1'
  ```

# 관련된 Post
* @WebServlet과 @Controller Annotation의 차이에 대해 알고 싶으시면 [@WebServlet vs @Controller](https://gmlwjd9405.github.io/2018/12/22/webservlet-vs-controller.html)를 참고하시기 바랍니다.

# References
> - [http://cocoballl.blogspot.com/2014/07/webservlet.html](http://cocoballl.blogspot.com/2014/07/webservlet.html)
