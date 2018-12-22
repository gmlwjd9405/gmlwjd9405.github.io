---
layout: post
title: '[Web] web.xml 설정 내용, 역할 및 간단한 예시 이해하기'
subtitle: 'Web Application Structure 이해하기'
date: 2018-10-29
author: heejeong Kwon
cover: '/images/web/web-webxml-main.png'
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
> - spring MVC에서 web.xml의 구체적인 설정 내용을 이해한다.

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

--- 

## web.xml 기본 설정 
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

---

## spring MVC에서의 web.xml 구체적인 설정 내용
> - DispatcherServlet
> - ContextLoaderListener
> - encodingFilter

### DispatcherServlet
* Spring Container를 생성한다.
  * Spring Container: Controller의 lifecycle 관리 
* 클라이언트의 요청을 처음으로 받는 클래스 (Spring에서 제공)
* 클라이언트의 요청을 Handler(Controller)에 보낸다.
* 그 외에 필요한 것 
  * **HadlerMapping**
    * 어떤 url을 받을지 결정
  * **ViewResolver**
    * logical view name --- prefix, suffix ---> pysical view object
* ![](/images/web/springmvc-dispatcherservlet.png)
  * 예를 들어, 쇼핑몰의 경우 의류 / 가구에 대한 요청을 별도로 처리할 수 있다.
  * 각 기능의 요청 별로 DispatcherServlet을 할당한다. 
  * 아래와 같은 설정을 각 기능에 맞게 추가한다.

```xml
<servlet>
  <servlet-name>salesServlet</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <!-- contextLoader가 해당 위치의 설정 파일을 읽어, 해당 파일을 dispatcher servlet으로 만든다. -->
  <init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/salesServlet-servlet.xml</param-value>
  </init-param>
  <load-on-startup>1</load-on-startup>
</servlet>

<!-- /sales로 시작하는 url 요청을 받아 salesServlet에서 처리한다. -->
<servlet-mapping>
  <servlet-name>salesServlet</servlet-name>
  <url-pattern>/sales</url-pattern>
</servlet-mapping>
```
  * ` <init-param>`부분은 생략이 가능하다.
    * ` <servlet-name>` 에 설정한 이름 + -servlet.xml 형식으로 설정 파일 이름을 만들고, web.xml과 같은 위치(/WEB-INF 하위)에 있어야 contextLoader가 해당 파일을 찾아서 읽을 수 있다.
    * 위와 같이 설정하면 init-param으로 dispatcher xml 파일의 이름 설정하지 않아도 자동으로 로드된다.
    * Ex) salesServlet-servlet.xml
  * salesServlet-servlet.xml 안의 설정 내용 <mark>(* 아래 참고)</mark>

### ContextLoaderListener
* Controller가 공유하는 Bean들을 포함하는 Spring Container를 생성한다.
  * 공유하는 Bean: Dao, DataSource, Service
  * ![](/images/web/springmvc-contextLoadListener.png)
  * 각 Bean에 대한 설정 파일을 따로 생성한다.
    * **service-context.xml**: Service 관련 
    * **dao-context.xml**: Dao 관련 
    * **applicationContext.xml**: DataSource 관련, properties 등록, SessionFactory, TransactionManager 등 
    * **security-context.xml**: Security 관련, BCryptPasswordEncoder 등
    * cf) **salesServlet-servlet.xml**: controller 관련, ViewResolver, [mvc:annotation-driven 설정](https://gmlwjd9405.github.io/2018/12/18/spring-annotation-enable.html) 등
* DispatcherServlet에 의해 생성된 Bean은 ContextLoaderListener에 의해 생성된 Bean을 참조할 수 있다.

```xml
<!-- 이렇게 등록된 설정 파일에 따라 등록된 Bean들은 모두 공유가 가능하다. -->
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>
    /WEB-INF/service-context.xml
    /WEB-INF/dao-context.xml
    /WEB-INF/applicationContext.xml
    /WEB-INF/security-context.xml
  </param-value>
</context-param>

<listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

### encodingFilter
* 인코딩을 UTF-8로 설정하여 필터링하겠다는 설정이다.

```xml
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>

<!-- /의 형식으로 시작하는 url에 대하여 UTF-8로 인코딩 -->
<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

<br>
<mark>* 참고</mark> salesServlet-servlet.xml 안의 설정 내용
* [Annotation 활성화](https://gmlwjd9405.github.io/2018/12/18/spring-annotation-enable.html)
  * ` <mvc:annotation-driven />`
* Component 패키지 지정 
  * ` <context:component-scan base-package="controller"/>`
  * 이 패키지를 스캔하며 annotaion이 달린 것을 bean으로 생성하여 Container에 담아둔다.
  * 이 내용은 service, dao 설정에도 필요하다.
    * ` <context:component-scan base-package="service"/>`
    * ` <context:component-scan base-package="dao"/>`
* 정적인 data 위치 mapping
  * ` <mvc:resources mapping="/static/**" location="/static/" />`
  * ` <mvc:resources mapping="/resources/**" location="/resources/" />`
  * Controller가 처리할 필요 없이 해당 위치의 디렉터리에서 바로 접근할 수 있다.
  * HTTP GET 요청에서의 정적인 data에 바로 매핑이 가능하다.
* ViewResolver
  ```xml 
  <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/views/"/>
    <property name="suffix" value=".jsp"/>
  </bean>
  ```


# 관련된 Post
* Web Server와 WAS의 개념과 차이에 대해 알고 싶으시면 [Web Server VS WAS](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)를 참고하시기 바랍니다.
* Web Service의 기본적인 동작 과정과 Servlet에 대해 알고 싶으시면 [Servlet 이란](https://gmlwjd9405.github.io/2018/10/28/servlet.html)을 참고하시기 바랍니다.
* JSP의 개념과 기본 문법에 대해 알고 싶으시면 [JSP 란](https://gmlwjd9405.github.io/2018/11/03/jsp.html)을 참고하시기 바랍니다.
* Spring MVC Framework에 대해 알고 싶으시면 [Spring MVC 이해하기](http://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html)를 참고하시기 바랍니다.

# References
> - [https://www.javatpoint.com/jsp-tutorial](https://www.javatpoint.com/jsp-tutorial)
> - [https://code.i-harness.com/ko-kr/q/37b9fa](https://code.i-harness.com/ko-kr/q/37b9fa)
> - [http://istoryful.tistory.com/5](http://istoryful.tistory.com/5)
> - [http://egloos.zum.com/iris2380/v/25062](http://egloos.zum.com/iris2380/v/25062)