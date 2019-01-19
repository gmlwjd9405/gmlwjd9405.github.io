---
layout: post
title: '[Design Pattern] MVC Architecture'
subtitle: 'MVC Architecture를 이해한다.'
date: 2018-11-05
author: heejeong Kwon
cover: '/images/web/web-mvc-architecture-main.png'
tags: mvc architecture design-pattern 디자인패턴 structure web
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - MVC Architecture를 이해한다.
> - RequestDispatcher의 역할과 동작과정을 이해한다. 

## MVC Architecture란
Model, View, Controller를 분리한 디자인 패턴
* Model
    * 애플리케이션의 상태(data)를 나타낸다.
    * **Java Beans**
* View
    * 디스플레이 데이터 또는 프리젠테이션 
    * **JSP**
* Controller
    * View와 Model 사이의 인터페이스 역할
    * Model/View에 대한 사용자 입력 및 명령을 수신하여 그에 따라 적절하게 변경
    * **Servlet**

### JSP와 Servlet을 모두 이용하는 모델 (MVC Architecture)
![](/images/web/servlet-jsp-model2.png)
* JSP와 Servlet을 모두 사용하여 프레젠테이션 로직(View)과 비즈니스 로직(Controller)을 분리한다.
    * **View**(보여지는 부분)는 HTML이 중심이 되는 JSP를 사용
    * **Controller**(다른 자바 클래스에 데이터를 넘겨주는 부분)는 Java 코드가 중심이 되는 Servlet을 사용
    * **Model**은 Java Beans로, DAO를 통해 Mysql과 같은 Data Storage에 접근
* 동작 과정
    * 1) 클라이언트(브라우저)는 Servlet으로 요청을 보낸다.
    * 2-1) Servlet은 DB와 연결된 Java Bean 객체를 생성한다.
    * 2-2) Java Bean은 DB에서 적절한 정보를 가져와 저장한다. 
    * 2-3) Servlet에서 추가적인 비지니스 로직 과정을 수행한다.
    * 3) Servlet은 JSP 페이지와 통신한다. 
    * 4) JSP 페이지는 Java Bean과 통신한다.
    * 5) JSP 페이지가 클라이언트(브라우저)에 응답한다.

### 구체적인 MVC Flow of Control(Annotated)
![](/images/web/mvc-flow-of-control.png)
* 1) @WebServlet 또는 web.xml의 url-pattern에 해당하는 Servlet으로 부터 
* 2-1) 사용자가 입력한 form data를 Service에 인자들로 넘긴다.
* **Service**
    * DAO와 DB를 거쳐 결과값을 가져온다.
    * Business Logic을 처리한다.
* 2-2) 결과값을 bean 객체에 저장한다.
* **Controller**
    * request 객체에 결과값(bean)을 저장한다.
* 3) 적절한 JSP 페이지로 전달한다. 
* 4) bean 객체로부터 data를 추출하고 output을 생성한다. 
* **View**
    * JSP Action Tag나 JSP EL(Expression Language)를 사용하여 bean 객체에 적절한 property를 넣는다.
    * 적절한 동적인 페이지를 만들어 반환한다.
* 5) 동적인 페이지 결과를 클라이언트(브라우저)에 전달한다. 

---

## RequestDispatcher
### Implementing MVC with RequestDispatcher
1. Define beans to represent result data
    * 결과 데이터를 표현하기 위해 beans를 정의한다. 
    * 하나 이상의 getXXX 메서드가 있는 Java 클래스
2. Use a servlet to handle requests
    * 사용자 요청을 다루기 위해 Servlet을 사용한다. 
    * Servlet은 요청 파라미터를 읽어 누락된 데이터와 잘못된 데이터를 확인하고 비즈니스 로직을 호출한다. 
3. Perform business logic and get a bean that contains result
    * 비즈니스 로직을 수행하고 결과를 포함하는 bean을 가져온다. 
    * Servlet은 비즈니스 로직(application-specific code) 또는 데이터 접근 코드를 호출하여 결과를 가져온다. 
4. Store the bean in the request, session, or servlet context
    * Servlet은 요청의 결과를 나타내는 bean에 대한 참조를 저장하기 위해 request, session, 또는 servlet context에 대해 setAttribute를 호출한다. 
5. Forward the request to a JSP page
    * JSP 페이지에 요청을 전달한다. 
    * Servlet은 상황에 적합한 JSP 페이지를 결정하고 RequestDispatcher의 forward 메서드를 사용하여 해당 JSP 페이지로 제어를 전송한다. 
6. Extract the data from the beans
    * bean으로부터 data를 추출한다. 
    * JSP 1.2 (이전 버전)
        * JSP 페이지는 `jsp:useBean` 및 4단계의 location와 일치하는 scope가 있는 bean에 접근한다. 
        * 그런 다음 `jsp:getProperty`를 사용하여 bean properties를 출력한다. 
    * JSP 2.0 (선호!) : **EL(Expression Language)**
        * JSP 페이지는 `${nameFromServlet.property}`를 사용하여 bean properties를 출력한다. 
    * 어느 쪽이든, JSP 페이지는 bean을 생성하거나 수정하지 않는다. 
    * Servlet이 생성한 데이터를 단순히 추출하여 표시한다. 

### 예시 및 동작 과정 
```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	// 1. servlet reads request parameters
	String customerId = request.getParameter("customerId");

	// 2-1). Do business logic and get a bean that contains result
	
 	// 2-2). Store the bean in the request context
    request.setAttribute("customer", customer);
    
    // forward the request to a jsp page
    String address;
    if (customer == null) {
      request.setAttribute("badId", customerId);
      address = "/error.jsp";
    } else  {
      address = "/success.jsp";
    } 

    // 3) Servlet invokes JSP page 
	RequestDispatcher dispatcher = request.getRequestDispatcher(address);
    dispatcher.forward(request, response);
}
```
1. Servlet이 사용자 요청에 응답한다.
    * `request.getParameter`를 이용하여 form data를 가져온다. 
2. Servlet은 비즈니스 로직을 호출한다.
    * 1) 비즈니스 로직의 결과를 포함하는 bean을 가져온다. 
    * 2) 요청의 결과를 나타내는 bean을 HttpServletRequest(request), HttpSession(session) 또는 ServletContext(application)에 저장한다. 
3. Servlet은 `RequestDispatcher.forward`를 통해 JSP 페이지를 호출한다.
4. JSP 페이지는 bean에서 데이터를 읽는다.
    * JSP 2.0: `${beanName.propertyName}` 이용 
    * JSP 1.2: 적절한 scope(request, session, 또는 application)의 `jsp:useBean` 및 `jsp:getProperty` 이용 

<mark>참고</mark>  **Scopes**
* "Scopes"는 Bean이 저장되는 공간 
* 세 가지 Scope
    * **request**
        * 요청에 저장된 data는 *Servlet*과 Servlet이 전달하는 *JSP 페이지*에 표시된다. 
        * data는 다른 사용자 또는 다른 페이지에서 볼 수 없다.
        * 가장 일반적인 Scope
    * **session**
        * 요청에 저장된 data는 Servlet과 Servlet이 전달하는 *JSP 페이지*에  표시된다. 
        * 동일한 사용자인 경우 data를 다른 페이지 또는 나중에 볼 수 있다. 
        * 다른 사용자는 data를 볼 수 없다.
        * 흔하게 사용하는 Scope
    * **application** (Servlet Context)
        * Servlet Context에 저장된 데이터는 모든 사용자와 응용 프로그램의 모든 페이지에서 볼 수 있다.
        * 거의 사용하지 않는 Scope
* [Spring Bean Scope](https://gmlwjd9405.github.io/2018/11/10/spring-beans.html) 참고 

<mark>참고</mark>  **Bean 객체**
* 특정 규칙을 따르는 Java Class 
* 규칙
    * 인수가 0인 기본 생성자가 반드시 있어야 한다.
        * 기본 생성자를 명시적으로 정의하거나 
        * 모든 생성자를 생략하거나 
    * public 인스턴스 변수(필드)가 없어야 한다. 
    * Persistent values는 getXXX 및 setXXX 메서드를 통해 접근해야 한다.
* 예시 
```java
package coreservlets;
public class StringBean {
    private String message = "No message specified";
    // getter
    public String getMessage() {
        return(message);
    }
    // setter
    public void setMessage(String message) {
        this.message = message;
    }
}
```


# 관련된 Post
* Web Service의 기본적인 동작 과정과 Servlet에 대해 알고 싶으시면 [Servlet 이란](https://gmlwjd9405.github.io/2018/10/28/servlet.html)을 참고하시기 바랍니다.
* JSP의 개념과 기본 문법에 대해 알고 싶으시면 [JSP 란](https://gmlwjd9405.github.io/2018/11/03/jsp.html)을 참고하시기 바랍니다.
* Servlet과 JSP의 차이에 대해 알고 싶으시면 [Servlet과 JSP의 차이](https://gmlwjd9405.github.io/2018/11/04/servlet-vs-jsp.html)을 참고하시기 바랍니다.
* Spring MVC Framework에 대해 알고 싶으시면 [Spring MVC 이해하기](http://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html)를 참고하시기 바랍니다.
* Spring Bean의 개념과 Bean Scope에 대해 알고 싶으시면 [Spring Bean 이해하기](https://gmlwjd9405.github.io/2018/11/10/spring-beans.html)를 참고하시기 바랍니다.


# References
> - [http://anster.tistory.com/128](http://anster.tistory.com/128)
> - [https://www.javajee.com/application-request-session-and-page-scopes-in-servlets-and-jsps](https://www.javajee.com/application-request-session-and-page-scopes-in-servlets-and-jsps)