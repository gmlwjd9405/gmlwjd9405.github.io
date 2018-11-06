---
layout: post
title: '[Web] Web Server와 WAS의 차이와 웹 서비스 구조'
subtitle: 'Web Server와 WAS의 차이를 이해한다.'
date: 2018-10-27
author: heejeong Kwon
cover: '/images/web/web-webserver-vs-was-main.png'
tags: spring web architecture was webserver
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Static Pages와 Dynamic Pages 과정을 이해한다.
> - Web Server와 WAS의 차이를 이해한다.
> - Web 서비스 구조(Web Service Architecture)에 대해 이해한다.

## Static Pages와 Dynamic Pages
![](/images/web/static-vs-dynamic.png)
1. Static Pages
  * Web Server는 파일 경로 이름을 받아 경로와 일치하는 file contents를 반환한다.
  * 항상 동일한 페이지를 반환한다.
2. Dynamic Pages
  * 인자의 내용에 맞게 동적인 contents를 반환한다.
  * Servlet: WAS 위에서 돌아가는 Java Program
    * 개발자는 Servlet에 doGet()을 구현한다.

## Web Server와 WAS의 차이
![](/images/web/webserver-vs-was1.png)
### Web Server
* Web Server의 개념
  * 소프트웨어와 하드웨어로 구분된다.
  * 1) 하드웨어
    * Web 서버가 설치되어 있는 컴퓨터
  * 2) 소프트웨어
    * <span style="background-color: #e1e1e1">웹 브라우저 클라이언트로부터 HTTP 요청을 받아 **정적인 컨텐츠(.html .jpeg .css 등)**를 제공하는 컴퓨터 프로그램</span>
* Web Server의 기능
  * **HTTP 프로토콜을 기반으로 하여 웹 브라우저의 요청을 서비스 하는 기능**을 담당한다.
  * 요청에 따라 아래의 두 가지 기능 중 적절하게 선택하여 수행한다.
  * 기능 1)
    * 정적인 컨텐츠 제공
    * WAS를 거치지 않고 바로 자원을 제공한다.
  * 기능 2)
    * 동적인 컨텐츠 제공을 위한 요청 전달
    * 클라이언트의 요청(Request)을 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 전달(응답, Response)한다.
* Web Server의 예
  * Ex) Apache Server, Nginx, IIS(Windows 전용 Web 서버) 등

### WAS(Web Application Server)
* WAS의 개념
  * <span style="background-color: #e1e1e1">DB 조회나 다양한 로직 처리를 요구하는 **동적인 컨텐츠**를 제공하기 위해 만들어진 Application Server</span>
  * HTTP를 통해 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어(소프트웨어 엔진)이다.
  * **"웹 컨테이너(Web Container)" 혹은 "서블릿 컨테이너(Servlet Container)"**라고도 불린다.
    * Container란 JSP, Servlet을 실행시킬 수 있는 소프트웨어를 말한다.
    * 즉, WAS는 JSP, Servlet 구동 환경을 제공한다.
* WAS의 기능
  * **WAS = Web Server + Web Container**
  * Web Server 기능들을 구조적으로 분리하여 처리하고자하는 목적으로 제시되었다.
    * 분산 트랜잭션, 보안, 메시징, 쓰레드 처리 등의 기능을 처리하는 분산 환경에서 사용된다.
    * 주로 DB 서버와 같이 수행된다. 
* WAS의 예
  * Ex) Tomcat, JBoss, Jeus, Web Sphere 등

### Web Server와 WAS를 구분하는 이유
![](/images/web/webserver-vs-was2.png)
* **Web Server가 필요한 이유?**
  * 클라이언트(웹 브라우저)에 이미지 파일(정적 컨텐츠)을 보내는 과정을 생각해보자.
    * 이미지 파일과 같은 정적인 파일들은 웹 문서(HTML 문서)가 클라이언트로 보내질 때 함께 가는 것이 아니다.
    * 클라이언트는 HTML 문서를 먼저 받고 그에 맞게 필요한 이미지 파일들을 다시 서버로 요청하면 그때서야 이미지 파일을 받아온다.
    * Web Server를 통해 정적인 파일들을 Application Server까지 가지 않고 앞단에서 빠르게 보내줄 수 있다.
  * 따라서 Web Server에서는 정적 컨텐츠만 처리하도록 기능을 분배하여 서버의 부담을 줄일 수 있다.

* **WAS가 필요한 이유?**
  * 웹 페이지는 정적 컨텐츠와 동적 컨텐츠가 모두 존재한다.
    * 사용자의 요청에 맞게 적절한 동적 컨텐츠를 만들어서 제공해야 한다.
    * 이때, Web Server만을 이용한다면 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어 놓고 서비스를 해야 한다. 
    * 하지만 이렇게 수행하기에는 자원이 절대적으로 부족하다.
  * 따라서 WAS를 통해 요청에 맞는 데이터를 DB에서 가져와서 비즈니스 로직에 맞게 그때 그때 결과를 만들어서 제공함으로써 자원을 효율적으로 사용할 수 있다.

* **그렇다면 WAS가 Web Server의 기능도 모두 수행하면 되지 않을까?**
  1. 기능을 분리하여 서버 부하 방지
    * WAS는 DB 조회나 다양한 로직을 처리하느라 바쁘기 때문에 단순한 정적 컨텐츠는 Web Server에서 빠르게 클라이언트에 제공하는 것이 좋다.
    * WAS는 기본적으로 동적 컨텐츠를 제공하기 위해 존재하는 서버이다.
      * 만약 정적 컨텐츠 요청까지 WAS가 처리한다면 정적 데이터 처리로 인해 부하가 커지게 되고, 동적 컨텐츠의 처리가 지연됨에 따라 수행 속도가 느려진다.
      * 즉, 이로 인해 페이지 노출 시간이 늘어나게 될 것이다.
  2. 물리적으로 분리하여 보안 강화
    * SSL에 대한 암복호화 처리에 Web Server를 사용
  3. 여러 대의 WAS를 연결 가능
    * Load Balancing을 위해서 Web Server를 사용
    * fail over, fail back 처리에 유리
  4. 여러 웹 어플리케이션 서비스 가능
    * 예를 들어, 하나의 서버에서 PHP Application과 Java Application을 함께 사용하는 경우
  5. 기타
    * 접근 허용 IP 관리, 2대 이상의 서버에서의 세션 관리 등도 Web Server에서 처리하면 효율적이다.

* 즉, ***Web Server를 WAS 앞에 두고 필요한 WAS들을 Web Server에 플러그인 형태로 설정하면 더욱 효율적인 분산 처리가 가능하다.***

## Web Service Architecture
* 다양한 구조를 가질 수 있다.
  1. Client -> Web Server -> DB
  2. Client -> WAS -> DB
  3. Client -> Web Server -> WAS -> DB  

![](/images/web/web-service-architecture.png)

3번 구조의 동작과정
1. Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받는다.
2. Web Server는 클라이언트의 요청(Request)을 WAS에 보낸다.
3. WAS는 관련된 Servlet을 메모리에 올린다.
4. WAS는 web.xml을 참조하여 해당 Servlet에 대한 Thread를 생성한다. (Thread Pool 이용)
5. HttpServletRequest와 HttpServletResponse 객체를 생성하여 Servlet에 전달한다.
  * 5-1. Thread는 Servlet의 service() 메서드를 호출한다.
  * 5-2. service() 메서드는 요청에 맞게 doGet() 또는 doPost() 메서드를 호출한다.
  * `protected doGet(HttpServletRequest request, HttpServletResponse response)`
6. doGet() 또는 doPost() 메서드는 인자에 맞게 생성된 적절한 동적 페이지를 Response 객체에 담아 WAS에 전달한다.
7. WAS는 Response 객체를 HttpResponse 형태로 바꾸어 Web Server에 전달한다.
8. 생성된 Thread를 종료하고, HttpServletRequest와 HttpServletResponse 객체를 제거한다.

# 관련된 Post
* Web Application Structure과 web.xml 설정 내용, 역할 및 간단한 예시에 대해 알고 싶으시면 [Web Application Structure 이해하기](https://gmlwjd9405.github.io/2018/10/29/web-application-structure.html)를 참고하시기 바랍니다.

# References
> - [http://2dubbing.tistory.com/29](http://2dubbing.tistory.com/29)
> - [https://okky.kr/article/243427](https://okky.kr/article/243427)
> - [http://jeong-pro.tistory.com/84](http://jeong-pro.tistory.com/84)