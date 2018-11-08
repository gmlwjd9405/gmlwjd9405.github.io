---
layout: post
title: '[Web] Servlet과 JSP의 차이와 관계'
subtitle: 'Servlet과 JSP의 차이와 관계를 이해한다.'
date: 2018-11-04
author: heejeong Kwon
cover: '/images/web/web-servlet-vs-jsp-main.png'
tags: spring web servlet jsp
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Servlet과 JSP의 개념을 이해한다.
> - Servlet과 JSP의 차이를 이해한다.
> - Servlet과 JSP의 관계를 이해한다.


## Servlet과 JSP의 개념
<span style="background-color: #e1e1e1">기능의 차이는 없고 역할의 차이만 있다.</span> (하는 일은 동일)

### Servlet이란
* 웹 기반의 요청에 대한 동적인 처리가 가능한 Server Side에서 돌아가는 Java Program 
* ***Java 코드*** 안에 HTML 코드 (하나의 클래스)
* 웹 개발을 위해 만든 표준
* 구체적인 내용은 [https://gmlwjd9405.github.io/2018/10/28/servlet.html](https://gmlwjd9405.github.io/2018/10/28/servlet.html) 참고

### JSP란
* Java 언어를 기반으로 하는 Server Side 스크립트 언어
* ***HTML 코드*** 안에 Java 코드 
* Servlet를 보완하고 기술을 확장한 스크립트 방식 표준
    * Servlet의 모든 기능 + 추가적인 기능
    * ![](/images/web/jsp-definition.png)
* 구체적인 내용은 [https://gmlwjd9405.github.io/2018/11/03/jsp.html](https://gmlwjd9405.github.io/2018/11/03/jsp.html) 참고

## Servlet과 JSP의 차이
### Servlet
* ***Java 코드*** 안에 HTML 코드 (하나의 클래스)
* **data processing(Controller)**에 좋다.
* 즉 DB와의 통신, Business Logic 호출, 데이터를 읽고 확인하는 작업 등에 유용하다.
* Servlet이 수정된 경우 Java 코드를 컴파일(.class 파일 생성)한 후 동적인 페이지를 처리하기 때문에 전체 코드를 업데이트하고 다시 컴파일한 후 재배포하는 작업이 필요하다. **(개발 생산성 저하)**

### JSP
* ***HTML 코드*** 안에 Java 코드 
* **presentation(View)**에 좋다. 
* 즉 요청 결과를 나타내는 HTML 작성하는데 유용하다.
* JSP가 수정된 경우 재배포할 필요가 없이 WAS가 알아서 처리한다. **(쉬운 배포)**

<!-- 쉬운 관리
    * Presentation Logic(View)과 Business Logic의 분리 -->

## Servlet과 JSP의 예시 
![](/images/web/servlet-jsp-result.png)
### Servlet
```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public ThreeParams extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        
        response.setContentType("text/html");
        printWriter out = response.getWriter();
        
        String title = "Reading Three Request Parameters";
        String docType = "<!DOCTYPE html PUBLIC \"-//W3C//DTD HTML 4.01 Transitional//EN\">\n";
        
        out.println(docType + 
            "<HTML>\n" +
            "<HEAD><TITLE>" + title + "</TITLE></HEAD>\n" +
            "<BODY BGCOLOR=\"#FDF5E6\">\n" +  
            "<H1 ALIGN=\"CENTER\">" + title + "</H1>\n" + 
            "<UL>\n" + 
            "<LI><B>param1</B>: " + request.getParameter("param1") + "\n" +
            "<LI><B>param2</B>: " + request.getParameter("param2") + "\n" +
            "<LI><B>param3</B>: " + request.getParameter("param3") + "\n" +
            "</UL>\n" +
            "</BODY></HTML>");
        )
    }
}
```

### JSP
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<HTML>
<HEAD>
<TITLE>Reading Three Request Parameters</TITLE>
<LINK REL=STYLESHEET HREF="JSP-Styles.css" TYPE="text/css">
</HEAD>

<BODY>
<H1>Reading Three Request Parameters</H1>
<UL>
    <LI><B>param1</B>: <%= request.getParameter("param1") %>
    <LI><B>param2</B>: <%= request.getParameter("param2") %>
    <LI><B>param3</B>: <%= request.getParameter("param3") %>
</UL>
</BODY>
</HTML>
```

## Servlet과 JSP의 관계
### JSP만을 이용하는 모델
![](/images/web/servlet-jsp-model1.png)
* JSP가 사용자의 요청을 받아 Java Bean(DTO, DAO)을 호출하여 적절한 동적인 페이지를 생성한다.
* 동작 과정
    * JSP로 작성된 프로그램은 내부적으로 WAS에서 Servlet 파일로 변환
    * JSP 태그를 분해하고 추출하여 다시 순수한 HTML 웹 페이지로 변환 
    * 클라이언트로 응답
* 특징 
    * 개발 속도가 빠르다.
    * 배우기 쉽다.
    * 프레젠테이션 로직(View)과 비즈니스 로직(Controller)이 혼재한다.
    * JSP 코드가 복잡해져 유지 보수가 어려워진다.

### JSP와 Servlet을 모두 이용하는 모델 (MVC Architecture)
![](/images/web/servlet-jsp-model2.png)
* JSP와 Servlet을 모두 사용하여 프레젠테이션 로직(View)과 비즈니스 로직(Controller)을 분리한다.
* **View**(보여지는 부분)는 HTML이 중심이 되는 JSP를 사용
* **Controller**(다른 자바 클래스에 데이터를 넘겨주는 부분)는 Java 코드가 중심이 되는 Servlet을 사용
* **Model**은 Java Beans로, DTO와 DAO를 통해 Mysql과 같은 Data Storage에 접근
* 구체적인 MVC 패턴은 [MVC-Architecture](https://gmlwjd9405.github.io/2018/11/05/mvc-architecture.html) 참고

# 관련된 Post
* Web Service의 기본적인 동작 과정과 Servlet에 대해 알고 싶으시면 [Servlet 이란](https://gmlwjd9405.github.io/2018/10/28/servlet.html)을 참고하시기 바랍니다.
* JSP의 개념과 기본 문법에 대해 알고 싶으시면 [JSP 란](https://gmlwjd9405.github.io/2018/11/03/jsp.html)을 참고하시기 바랍니다.
* Web Server와 WAS의 개념과 차이에 대해 알고 싶으시면 [Web Server VS WAS](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)를 참고하시기 바랍니다.
* Web Application Structure과 web.xml 설정 내용, 역할 및 간단한 예시에 대해 알고 싶으시면 [Web Application Structure 이해하기](https://gmlwjd9405.github.io/2018/10/29/web-application-structure.html)를 참고하시기 바랍니다.


# References
> - [http://anster.tistory.com/128](http://anster.tistory.com/128)