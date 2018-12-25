---
layout: post
title: '[Annotation] Servlet Annotation @WebServlet vs @Controller'
subtitle: '@WebServlet과 @Controller Annotation의 차이를 이해하다.'
date: 2018-12-22
author: heejeong Kwon
cover: '/images/spring-framework/spring-annotation-main.png'
tags: spring annotation webservlet servlet controller
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## [@WebServlet](https://gmlwjd9405.github.io/2018/12/22/webservlet-annotation.html)
`javax.servlet.annotation`
```java
@Target(value=TYPE)
@Retention(value=RUNTIME)
@Documented
public @interface WebServlet
```

* 서블릿을 선언할 때 사용되는 Annotation
* 이 Annotation이 표시된 클래스는 Servlet Container에 의해 처리된다.
  * 속성 값을 통해 해당 Servlet과 매핑될 URL 패턴을 지정한다.

## @Controller
`org.springframework.stereotype`
```java
@Target(value=TYPE)
@Retention(value=RUNTIME)
@Documented
@Component
public @interface Controller
```

* 이 Annotation이 표시된 클래스는 "Controller" 임을 나타낸다.
* @Controller는 @Component의 구체화된 역할을 한다.
  * classpath scanning을 통해 구현 클래스를 자동으로 감지할 수 있도록 해준다.
* 일반적으로 RequestMapping 어노테이션을 기반으로 한 handler method와 함께 사용한다.

## @WebServlet과 @Controller Annotation의 차이
* WebServlet과 Spring MVC Controller는 같은 일을 하는 데 사용된다.

### Java Servlet의 @WebServlet
* 서블릿은 J2EE 프레임워크의 일부이며 모든 Java 애플리케이션 서버(Tomcat, Jetty 등)는 서블릿을 실행하기 위해 빌드된다.
* 서블릿은 J2EE 스택의 "하위 레벨" 계층이다. 즉, 애플리케이션 서버와 함께 미리 패키징되어 있기 때문에 애플리케이션을 실행하기 위해 **servlet.jar가 필요하지 않다.**
* 성능상의 이유로, 여러 편의를 제공하는 무거운 Spring MVC보다 Java Servlet을 사용하는 것이 유리할 때도 있다.

### Spring MVC의 @Controller
* Spring MVC
  * Java 웹 애플리케이션을 구현하는 대부분의 사람들은 서블릿 위에 구축된 일종의 **프레임워크**를 사용하여 개발을 더 쉽게 만든다. Spring MVC는 서블릿 위에 구축된 프레임워크 중 하나이다.
  * Java 웹 애플리케이션을 구현하는 데 있어서 더 많은 편의를 제공한다.
    * (binary form management, form parameter to bean conversion, parameter validation 등)
    * Spring MVC는 form 매개변수와 controller method 매개변수 매핑, binary form 제출(form이 파일을 업로드할 수 있는 경우)에서의 더 쉬운 처리 등과 같은 보다 기본적인 기능들을 제공한다.
  * 하나의 클래스에서 여러 URL과 메소드의 입력을 쉽게 관리 할 수 ​​있다.
    * 서블릿에서 동일한 작업을 수행할 수는 있지만 코드가 더 복잡하고 읽기 어렵다.
* Spring MVC의 Controller
  * 일을 더 쉽게하기 위해 서블릿 위에 구축된 **라이브러리**이다. 
  * 기본적으로 Spring MVC에서의 모든 요청은 DispatcherServlet에 매핑된다.
    * 그런 다음 DispatcherServlet은 어노테이션이 들어오는 요청과 일치하는 컨트롤러를 호출한다.
    * 매핑과 관련된 정보는 web.xml에 작성하거나 해당 서블릿에 @Controller Annotation을 달 수 있다.
  * Spring MVC의 Controller를 실행하기 위해서는 애플리케이션에 **필요한 jar를 패키징해야 한다.**


# References
> - [https://stackoverflow.com/questions/16439249/when-to-use-servlet-or-controller](https://stackoverflow.com/questions/16439249/when-to-use-servlet-or-controller)
> - [https://docs.oracle.com/javaee/6/api/javax/servlet/annotation/WebServlet.html](https://docs.oracle.com/javaee/6/api/javax/servlet/annotation/WebServlet.html)
> - [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Controller.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Controller.html)