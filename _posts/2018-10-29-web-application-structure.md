---
layout: post
title: '[Web] web.xml 설정 내용, 역할 및 간단한 예시 이해하기'
subtitle: 'Web Application Structure 이해하기'
date: 2018-10-29
author: heejeong Kwon
cover: '/images/web/web-main.png'
tags: spring web architecture structure setting
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Web Application Structure(웹 서비스 기본 설정 구조)을 이해한다.
> - web.xml의 설정 내용을 이해한다.
> - web.xml의 역할과 간단한 예시를 살펴본다.

## Web Application Structure(웹 서비스 기본 설정 구조)
![](/images/web/web-application-structure.png)
### 1. src
* 개발자가 작성한 Servlet 코드가 저장된다.

### 2. Libraries
* Servlet이나 JSP에서 추가로 사용하는 라이브러리 또는 드라이버
* jar로 압축한 파일이어야 한다.

### 3. WebContent
* Deploy할 때 WebContent 디렉터리 전체가 **.war**로 묶어서 보내진다.
* WEB-INF
  * ***lib:*** 
    * 추가한 모든 라이브러리 또는 드라이버가 이곳에 모두 저장된다.
  * ***classes:*** 
    * 작성한 Java Servlet 파일이 나중에 .class로 이곳에 모두 저장된다.
  * ***web.xml:*** 
    * SUN에서 정해놓은 규칙에 맞게 작성해야 하며 모든 WAS에 대하여 작성 방법이 동일하다.
    * <mark>아래 추가 설명</mark>
* .html 파일들
  * 관련된 HTML 소스를 저장한다.
  * Ex) WebContent - views Directory - index.html는 `http://localhost/helloLogin/views/index.html`와 매핑된다.


## Web.xml 기본 설정 
### 개념 
* web application의 설정을 위한 <span style="background-color: #e1e1e1">deployment descriptor</span>

### 역할
* Deploy할 때 Servlet의 정보를 설정해준다.
* 브라우저가 Java Servlet에 접근하기 위해서는 WAS(Ex. Tomcat)에 필요한 정보를 알려줘야 해당하는 Servlet을 호출할 수 있다.
  * 정보 1) 배포할 Servlet이 무엇인지
  * 정보 2) 해당 Servlet이 어떤 URL에 매핑되는지

### 간단한 예시

```xml
<web-app>

    <!-- 1. aliases 설정 -->
    <servlet>
        <servlet-name>welcome</servlet-name>
        <servlet-class>servlets.WelcomeServlet</servlet-class>
    </servlet>

    <!-- 2. 매핑 -->
    <servlet-mapping>
        <servlet-name>welcome</servlet-name>
        <url-pattern>/welcome</url-pattern>
    </servlet-mapping>

</web-app>
```

1. aliases 설정
  * **서블릿 이름을 실제 서블릿 클래스에 연결**
  * `<servlet-name>welcome</servlet-name>`과 아래 매핑 설정에서의 servlet-name은 반드시 같아야 한다.
  * `<servlet-class>servlets.WelcomeServlet</servlet-class>`은 개발자에 의해 작성된 실제 클래스 이름으로 설정해야 한다.
    * Ex. (패키지 이름).(서블릿 클래스 이름)
2. 매핑
  * **URL을 서블릿 이름에 연결**
  * `<url-pattern>/welcome</url-pattern>`은 클라이언트(browser)의 요청 URL에서 앱(프로젝트) 이름 뒤에 오는 부분으로, 슬래시('/')로 시작해야 한다.

<mark>참고</mark>  클라이언트(browser)가 요청하는 URL 정보
* `요청을 보낼 서버의 IP 주소 : Port 번호 / App 이름 / 달라고 요청하는 HTML`
  * Ex. `localhost:8080/FormHandlingServlet/LoginForm.html`


# 관련된 Post
* Web Server와 WAS의 개념과 차이에 대해 알고 싶으시면 [Web Server VS WAS](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)를 참고하시기 바랍니다.
* Web Service의 기본적인 동작 과정과 Servlet에 대해 알고 싶으시면 [Servlet 이란](https://gmlwjd9405.github.io/2018/10/28/servlet.html)을 참고하시기 바랍니다.
* JSP의 개념과 기본 문법에 대해 알고 싶으시면 [JSP 란](https://gmlwjd9405.github.io/2018/11/03/jsp.html)을 참고하시기 바랍니다.


# References
> - [https://www.javatpoint.com/jsp-tutorial](https://www.javatpoint.com/jsp-tutorial)