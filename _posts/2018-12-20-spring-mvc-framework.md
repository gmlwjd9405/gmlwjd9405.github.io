---
layout: post
title: '[SpringMVC] Spring MVC Framework란'
subtitle: 'Spring MVC를 이해한다.'
date: 2018-12-20
author: heejeong Kwon
cover: '/images/spring-framework/spring-framework-main.png'
tags: springMVC mvc architecture structure web
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - Spring MVC Architecture를 이해한다.
> - 기본 Project Structure을 이해한다.
> - Spring MVC에서 Model, View, Controller의 사용법을 이해한다.
  - Model
  - View
  - Controller
> - Spring MVC를 위한 필수적인 기본 설정 방법과 개념을 이해한다.
  - Maven Configuration (pom.xml)
  - Web Deployment Descriptor (web.xml)
  - Spring MVC Configuration

## Spring MVC Architecture란
![](/images/web/springmvc-architecture.png)
Model, View, Controller를 분리한 디자인 패턴 (개발자가 직접 구현해야 하는 것)
* Model
    * 애플리케이션의 상태(data)를 나타낸다.
    * 일반적으로 POJO로 구성된다.
    * **Java Beans**
* View
    * 디스플레이 데이터 또는 프리젠테이션 
    * Model data의 렌더링을 담당하며, HTML ouput을 생성한다.
    * **JSP**
    * JSP 이외에도 Thymeleaf, Groovy, Freemarker 등 여러 [Template Engine](https://gmlwjd9405.github.io/2018/12/21/template-engine.html)이 있다.
* Controller
    * View와 Model 사이의 인터페이스 역할
    * Model/View에 대한 사용자 입력 및 요청을 수신하여 그에 따라 적절한 결과를 Model에 담아 View에 전달한다.
    * 즉, Model Object와 이 Model을 화면에 출력할 View Name을 반환한다.
    * Controller ---> Service ---> Dao ---> DB
    * **Servlet**

Spring Framework가 제공하는 Class
* DispatcherServlet
    * Spring Framework가 제공하는 Servlet 클래스
    * 사용자의 요청을 받는다.
    * Dispatcher가 받은 요청은 HandlerMapping으로 넘어간다. 
* HandlerMapping
    * 사용자의 요청을 처리할 Controller를 찾는다. (Controller URL Mapping)
    * 요청 url에 해당하는 Controller 정보를 저장하는 table을 가진다.
    * 즉, 클래스에 @RequestMapping("/url") annotaion을 명시하면 해당 URL에 대한 요청이 들어왔을 때 table에 저장된 정보에 따라 해당 클래스 또는 메서드에 Mapping한다.
* ViewResolver
    * Controller가 반환한 View Name(the logical names)에 prefix, suffix를 적용하여 View Object(the physical view files)를 반환한다.
    * 예를 들어 view name: home, prefix: /WEB-INF/views/, suffix: .jsp는 "/WEB-INF/views/home.jsp"라는 위치의 View(JSP)에 Controller에게 받은 Model을 전달한다.
    * 이 후에 해당 View에서 이 Model data를 이용하여 적절한 페이지를 만들어 사용자에게 보여준다.

---

## 기본 Project Structure
Web Application Structure(웹 서비스 기본 설정 구조)
![](/images/web/web-project-structure.png)

* src
    * 개발자가 작성한 Servlet 코드가 저장된다.
    * Controller, Model, Service, Dao
    * src/main/java
        * 개발되는 Java 코드의 경로
    * src/main/resources
        * 서버가 실행될 때 필요한 파일들의 경로
    * src/test/java
        * 테스트 전용 경로 (각 테스트 코드 작성 경로)
    * src/test/resource
        * 테스트 시에만 사용되는 파일들의 경로
* Libraries
    * Servlet이나 JSP에서 추가로 사용하는 라이브러리 또는 드라이버
    * jar로 압축한 파일이어야 한다.
* WebContent (**전체 ROOT**) - webapp
    * Deploy할 때 WebContent 디렉터리 전체가 .war로 묶어서 보내진다.
    * resources
        * 정적인 데이터 (ex. image file, css, js, fonts)
    * WEB-INF
        * classes: 작성한 Java Servlet 파일이 나중에 .class로 이곳에 모두 저장된다. 
        * lib: 추가한 모든 라이브러리 또는 드라이버가 이곳에 모두 저장된다.
        * props: property file을 저장한다.
        * spring: <span style="background-color: #e1e1e1">**spring configuration files**</span>을 저장한다. (Spring과 관련된 설정 파일을 모아둔 것)
            * dispatcher-servlet.xml
            * applicationContext.xml
            * dao-context.xml, service-context.xml 등 
        * views: Controller와 매핑되는 .jsp 파일들을 저장한다. (JSP 파일의 경로)
        * web.xml: web application의 설정을 위한 <span style="background-color: #e1e1e1">**web deployment descriptor**</span>
            * DispatcherServlet, ContextLoadListener 설정
* pom.xml
    * <span style="background-color: #e1e1e1">**maven configuration file**</span>
    * 어떤 lib를 쓸지 명시한다.

---

## Spring MVC에서 Model, View, Controller
### Model
* Controller에서 View로 객체를 전달하는데 사용된다.
* 명명된 객체들의 집합이라고 할 수 있다.
    * Key-Value 형식의 하나의 쌍(하나의 열)을 명명된 객체라고 부른다.
    * 또한 이 명명된 객체는 model attribute라고 부른다.
    * 여러 개의 attribute가 모여 Table 형식을 이룬다.
* view에서 attribute의 key 값을 통해 value 값을 사용할 수 있다.

| Key(name) | Value |
| key1 | value |
| key2 | value2 | 

Model Inplementations
* Model을 표현하기 위해 여러 자료구조를 사용할 수 있다.
* Controller 메서드에 input argument로 값을 넣어주면 Spring Frmework가 자동으로 Model을 만들어주고 해당 Model의 주솟값만 넘겨준다.

1. java.util.map의 구현
```java
@RequestMapping("/greeting")
public String getGreeting(Map<String, Object> model) { 
	String greeting = service.getRandomGreeting();
	model.put("greeting", greeting);

	return "home";
}
```
    1. service 객체의 메서드를 호출하여 결과를 가져온다.
    2. model에 첫 번째 인자 "name"과 결과에 대한 값인 두 번째 인자 value를 넣는다.
        * view에서 해당 이름("name")으로 value에 접근한다.
    3. 해당하는 value를 보여줄 View name을 반환한다.
2. Spring에서 제공하는 Model 인터페이스 구현
```java
@RequestMapping("/special-deals")
public String getSpecialDeals(Model model) { 

	List<SpecialDial> specialDeals = service.getSpecialDeals();
	model.addAttribute(specialDeals); // value만 넣으면 name은 자동 생성

	return "home";
}
```
    * Map을 사용하는 것의 단점은 "name"을 반드시 지정해야하는 것이다.
    * Model 인터페이스는 addAttribute()와 같은 편리한 메소드를 제공한다.
        * addAttribute()는 Map 속성의 이름("name")을 자동으로 생성한다는 점을 제외하면 Map의 put()과 동일하다.
        * 자동으로 생성하고 싶지 않은 모델의 속성 이름을 결정하는 것은 여전히 가능하다.
        * **가장 자주 사용하는 Model 형식**
3. Spring에서 제공하는 ModelMap 객체
```java
@RequestMapping("/fullname")
public String getFullname(ModelMap model) { 

	// chained calls are handy!
	model.addAttribute("name", "Jon")
         .addAttribute("surname", "Snow");

	return "home";
}
```
    * 추가적인 기능을 제공한다.
        * chain으로 사용 가능

### Controller
```java
@Controller
public class HomeController {
    private static final Logget Logger = LoggerFactory.getLogger(HomeController.class);

    @RequestMapping(value = "/home", method = RequestMethod.GET)
    public String home(Locale locale, Model model) {
        Logger.info("Welcome {}.", locale);

        // Business Logic
        Date date = new Date();
        DateFormat = dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);
        String formattedDate = dateFormat.format(date);

        // BL의 결과를 Model에 저장 
        model.addAttribute("serverTime", formattedDate);

        // Return logical view name
        return "home";
    }

    @RequestMapping(value = "/login", method = RequestMethod.GET)
    public String doLogin(@RequestParam String username, @RequestParam String password) {
        ...
	    return success;
    }
}
```
* @Controller
    * bean으로 등록
    * 해당 클래스가 Controller로 사용됨을 Spring Framework에 알림
    * @Component ---구체화---> @Controller, @Service, @Repository
* @RequestMapping
    * value: 해당 url로 요청이 들어오면 이 메서드가 수행된다.
    * method: 요청 method를 명시한다.
    * 즉, 위의 예시에서는 "/home" url로 HTTP GET 요청이 들어오면 home() 메서드가 실행된다.
    ```java
    @Controller
    @RequestMapping("/home") // 1) Class Level
    public class HomeController {
            /* an HTTP GET for /home */ 
            @RequestMapping(method = RequestMethod.GET) // 2) Handler Level
            public String getAllEmployees(Model model) {
                ...
            }
            /* an HTTP POST for /home/employees */ 
            @RequestMapping(value = "/employees", method = RequestMethod.POST) 
            public String addEmployee(Employee employee) {
                ...
            }
    }
    ```
    * 1) Class Level Mapping
        * 모든 메서드에 적용되는 경우
        * "/home"로 들어오는 모든 요청에 대한 처리를 해당 클래스에서 한다는 것을 의미한다.
    * 2) Handler Level Mapping
        * 요청 url에 대해 해당 메서드에서 처리해야 되는 경우
        * "/home/employees" POST 요청에 대한 처리를 addEmployee()에서 한다는 것을 의미한다.
* model.addAttribute()
    * Business Logic의 처리 결과 값을 model attribute에 지정하면 Spring이 Model 객체를 만들어 해당 Model의 주솟값을 넘겨준다.
    * 하나의 요청 안에서만 Controller와 View가 Model을 공유한다.
* @RequestParam
    * HTTP GET 요청에 대해 매칭되는 request parameter 값이 자동으로 들어간다.
    * Ex) ` http://localhost:8080/login?username=scott&password=tiger`

### View
* View를 생성하는 방법은 여러 가지가 있다.
    * JSP 이외에도 Thymeleaf, Groovy, Freemarker 등 여러 [Tempate Engine](https://gmlwjd9405.github.io/2018/12/21/template-engine.html)이 있다.
* [JSP(Java Server Pages)](https://gmlwjd9405.github.io/2018/11/03/jsp.html)
    * [JSP 제한 사항](https://docs.spring.io/spring-boot/docs/2.0.0.RELEASE/reference/htmlsingle/#boot-features-jsp-limitations)
    * Java EE에 종속적이라는 단점이 있다.
    * SpringBoot에서는 공식적으로 jsp를 지원하지 않는다.
        * SpringBoot의 내장 Tomcat에 하드코딩 패턴때문에 jar형식으로는 webapp 내용을 가져올 수 없다.
        * 따라서 SpringBoot에서는 war가 아닌 jar로 사용할 때는 jsp를 사용할 수 없다.
* JSTL(JSP Standard Tag Library)
    * 많은 JSP 애플리케이션의 공통적인 핵심 기능을 캡슐화하는 유용한 JSP 태그 모음
    * 즉, JSP 페이지를 작성할 때 유용하게 사용할 수 있는 여러 가지 action과 함수가 포함된 라이브러리
    * 가장 많이 사용하는 태그 확장 라이브러리
    * 자신만의 Custom Tag를 추가할 수 있는 기능을 제공한다.
    * 사용하는 이유?
        * JSP에 Java Code가 들어가는 것을 막기 위해서 사용한다.
        * 즉, Java Code(JSP Scriptlet)대신 Tag를 사용하여 프로그래밍할 수 있도록 하기 위해 도입되었다.

---

## Spring MVC를 위한 필수 설정
### 1. Maven Configuration (pom.xml)
* 자신의 프로젝트에 대한 고유의 좌표 설정
    1. groupId
    * 자신의 프로젝트를 고유하게 식별하게 해 주는 것으로, 최소한 내가 컨트롤하는 domain name이어야 한다.
    * package 명명 규칙을 따른다.
    * 하위 그룹은 얼마든지 추가할 수 있다.
    2. artifactId
    * 제품의 이름으로, 버전 정보를 생략한 jar 파일의 이름이다.
    * 프로젝트 이름과 동일하게 설정한다.
    * 소문자로만 작성하며 특수문자는 사용하지 않는다.
    3. version
    * SNAPSHOT: 개발용, RELEASE: 배포용
    * 숫자와 점을 사용하여 버전 형태를 표현한다.(1.0, 1.1, 1.0.1, …)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.hee</groupId>
    <artifactId>projectName</artifactId>
	<name>projectName</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
    <!-- properties에 명시한 version이 알아서 placeholder에 주입된다. -->
	<properties>
		<java-version>1.8</java-version>
		<org.springframework-version>4.2.5.RELEASE</org.springframework-version>
		<spring-security-version>4.0.4.RELEASE</spring-security-version>
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<org.slf4j-version>1.6.6</org.slf4j-version>
	</properties>
    <dependencies>
    <!-- Spring -->
		<dependency>
            <!-- Spring core dependency -->
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		<dependency>
            <!-- Spring MVC dependency -->
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
    </dependencies>
</project>
```

<mark>참고</mark>  Maven 장점
* pom.xml에 명시한 lib를 자동으로 다운
* build process 자동화
    * compile -> test -> package(.war) -> install -> deploy

### 2. Web Deployment Descriptor (web.xml)
* 개념
    * web application의 설정을 위한 deployment descriptor
    * SUN에서 정해놓은 규칙에 맞게 작성해야 하며 모든 WAS에 대하여 작성 방법이 동일하다.
* 역할
    * Deploy할 때 Servlet의 정보를 설정해준다.
    * 브라우저가 Java Servlet에 접근하기 위해서는 WAS(Ex. Tomcat)에 필요한 정보를 알려줘야 해당하는 Servlet을 호출할 수 있다.
        * 정보 1) 배포할 Servlet이 무엇인지
        * 정보 2) 해당 Servlet이 어떤 URL에 매핑되는지
* [구체적인 설정 내용](https://gmlwjd9405.github.io/2018/10/29/web-application-structure.html)
    * DispatcherServlet
    * ContextLoaderListener
    * Filter: encodingFilter, springSecurityFilterChain

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee; http://java.sun.com/xml/ns/javaee/web-app_2.5.xsd">

	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			/WEB-INF/spring/service-context.xml
			/WEB-INF/spring/dao-context.xml
			/WEB-INF/spring/security-context.xml
			/WEB-INF/spring/applicationContext.xml
		</param-value>
	</context-param>
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<servlet>
		<servlet-name>dispatcher</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/dispatcher-servlet.xml
			</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>dispatcher</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter
		</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

	<filter>
		<filter-name>springSecurityFilterChain</filter-name>
		<filter-class>org.springframework.web.filter.DelegatingFilterProxy
		</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>springSecurityFilterChain</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
</web-app>
```
    
### 3. Spring MVC Configuration Files
**# dispatcher-servlet.xml**
* *주요 설정 내용:* Controller 관련, ViewResolver, [mvc:annotation-driven 설정](https://gmlwjd9405.github.io/2018/12/18/spring-annotation-enable.html) 등
* [Annotation 활성화](https://gmlwjd9405.github.io/2018/12/18/spring-annotation-enable.html)
  ```xml 
  <mvc:annotation-driven />
  ```     
* Component 패키지 지정 
  ```xml 
  <context:component-scan base-package="controller"/>
  ```   
  * 이 패키지를 스캔하며 annotaion이 달린 것을 bean으로 생성하여 Container에 담아둔다.
  * 참고) 이 내용은 service, dao 설정에도 필요하다.
    * ` <context:component-scan base-package="service"/>`
    * ` <context:component-scan base-package="dao"/>`
* 정적인 data 위치 mapping
  ```xml 
  <mvc:resources mapping="/resources/**" location="/resources/" />
  또는 
  <mvc:resources mapping="/static/**" location="/static/" />
  ```   
  * **web/resources/ 하위**에 정적인 데이터(css, js, img, font)가 존재 
  * Controller가 처리할 필요 없이 해당 위치의 디렉터리에서 바로 접근할 수 있다.
  * HTTP GET 요청에서의 정적인 data에 바로 매핑이 가능하다.
* ViewResolver
  ```xml 
  <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/views/"/>
    <property name="suffix" value=".jsp"/>
  </bean>
  ```           

**# applicationContext.xml**
* *주요 설정 내용:* DataSource 관련, properties 등록, SessionFactory, TransactionManager 등 
* properties 등록
  ```xml 
  <context:property-placeholder location="/WEB-INF/props/jdbc.properties" />
  동일 
  <context:property-placeholder location="classpath:props/jdbc.properties" />
  ```  
    * properties file에서 읽어와 주입한다.
* DataSource 주입 
  ```xml
  <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
		destroy-method="close">
	  <property name="driverClassName" value="${jdbc.driverClassName}" />
	  <property name="url" value="${jdbc.url}" />
	  <property name="username" value="${jdbc.username}" />
	  <property name="password" value="${jdbc.password}" />
  </bean>
  ```
* 어노테이션에 기반한 트랜잭션 동작의 설정을 활성화
  ```xml 
  <tx:annotation-driven />
  ```
* Session Factory 등록 및 Transaction Manager 설정
  ```xml
	<bean id="sessionFactory"
		class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<property name="packagesToScan">
			<list>
				<value>com.spring.model</value>
			</list>
		</property>
		<property name="hibernateProperties">
			<props>
				<prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
				<prop key="hibernate.hbm2ddl.auto">update</prop>
				<prop key="hibernate.show_sql">true</prop>
				<prop key="hibernate.format_sql">false</prop>
			</props>
		</property>
	</bean>

	<bean id="transactionManager"
		class="org.springframework.orm.hibernate5.HibernateTransactionManager">
		<property name="sessionFactory" ref="sessionFactory"></property>
	</bean>
  ```

**# service-context.xml**
* *주요 설정 내용:* Service 관련 
* Component 패키지 지정 
  * ` <context:component-scan base-package="service"/>`
  * 이 패키지를 스캔하며 annotaion이 달린 것을 bean으로 생성하여 Container에 담아둔다.

**# dao-context.xml**
* *주요 설정 내용:* DAO 관련
* Component 패키지 지정 
  * ` <context:component-scan base-package="dao"/>`
  * 이 패키지를 스캔하며 annotaion이 달린 것을 bean으로 생성하여 Container에 담아둔다.

**# security-context.xml**: 
* *주요 설정 내용:* Security 관련, BCryptPasswordEncoder 등
  ```xml
  <security:authentication-manager>
      <security:authentication-provider>
          <security:jdbc-user-service
              data-source-ref="dataSource"
              users-by-username-query="select username, password, enabled from users where username=?"
              authorities-by-username-query="select username, authority from users where username=?" />
          <security:password-encoder ref="passwordEncoder"></security:password-encoder>
      </security:authentication-provider>
  </security:authentication-manager>

  <security:http auto-config="true" use-expressions="true">
      <security:intercept-url pattern="/admin/**" access="hasRole('ROLE_ADMIN')" />
      <security:form-login login-page="/login" authentication-failure-url="/login?error" />
  </security:http>

  <bean id="passwordEncoder"
      class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder">
  </bean>
  ```

# 관련된 Post
* MVC Architecture에 대해 알고 싶으시면 [MVC Architecture 이해하기](https://gmlwjd9405.github.io/2018/11/05/mvc-architecture.html)를 참고하시기 바랍니다.
* Web Application Structure와 web.xml의 역할에 대해 알고 싶으시면 [Web Application Structure 이해하기](https://gmlwjd9405.github.io/2018/10/29/web-application-structure.html)를 참고하시기 바랍니다.
* Template Engine에 대해 알고 싶으시면 [Template Engine 이해하기](https://gmlwjd9405.github.io/2018/12/21/template-engine.html)를 참고하시기 바랍니다.

# References
> - [https://www.tutorialspoint.com/spring/spring_web_mvc_framework.htm](https://www.tutorialspoint.com/spring/spring_web_mvc_framework.htm)
> - [http://istoryful.tistory.com/5](http://istoryful.tistory.com/5)