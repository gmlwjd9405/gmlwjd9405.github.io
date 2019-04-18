---
layout: post
title: '[Network] HTTP의 동작 및 HTTP Message 형식'
subtitle: 'HTTP란 무엇인지, 각 요청/응답에서의 Message 형식을 이해한다.'
date: 2019-04-17
author: heejeong Kwon
cover: '/images/network/http-headers-main.png'
tags: network http ip port url uri header
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[Edwith 강의](https://www.edwith.org/boostcourse-web/lecture/16661/) 참고

## Goal
> - Internet이란
>   - IP 주소와 Port 번호의 개념을 이해한다.
> - HTTP란
>   - HTTP의 기본 특징 및 동작에 대해 이해한다.
>   - HTTP와 HTTPS의 차이에 대해 이해한다.
> - HTTP Message
>   - HTTP Request 형식
>   - HTTP Response 형식
> - URI와 URL의 차이에 대해 이해한다.

## 인터넷(네트워크 통신)의 이해
### 인터넷 (Internet)이란
- TCP/IP 기반의 네트워크가 전세계적으로 확대되어 하나의 연결된 네트워크들의 네트워크 (**네트워크의 결합체**)
- 인터넷 != WWW(World Wide Web)
  - 인터넷 기반의 대표 서비스 중 하나가 www라고 할 수 있다.
- 인터넷 기반의 서비스들

| 이름 | 프로토콜 | 포트 | 기능 |
|:----:|:------:|:----:|:----:|
| www | HTTP | 80 | 웹 서비스 |
| Email | SMTP/POP3/IMAP | 25/110/114 | 이메일 서비스 |
| FTP | FTP | 21 | 파일 전송 서비스 |
| DNS | TCP/UDP | 53 | 네임 서비스 |
| NEWS | NNTP | 119 | 인터넷 뉴스 서비스 |

- 물리적인 하나의 컴퓨터(IP 주소)에는 여러 개의 서버가 동작할 수 있다.
  - 각각의 서버들은 Port라는 값으로 구분돼서 동작한다.
- <mark>TIP</mark> IP 주소와 Port 번호의 개념
  - **IP 주소** 또는 **도메인 이름**
    - 하드웨어 서버(물리적인 컴퓨터)를 찾기 위해 반드시 필요한 정보
    - Ex) 집 주소
  - **Port 번호**
    - 해당 물리적인 컴퓨터 안에 존재하는 소프트웨어 서버를 찾기 위한 정보
    - 하나의 물리적인 컴퓨터에는 여러 개의 소프트웨어 프로그램이 각각의 Socket을 사용하여 데이터 통신을 하고 있기 때문에 각각의 Socket을 구분할 필요가 있다. Port 번호를 통해 각 Socket을 구분할 수 있다.
    - 0보다 큰 숫자로 구성되어 있다.
    - Ex) 집 안의 여러 방 번호

## 웹의 동작 (HTTP 프로토콜의 이해)
### HTTP (Hypertext Transfer Protocol)란
- **웹 브라우저와 웹 서버 간의 서로 통신하기 위한 규약**
  - 즉, HTTP(Hypertext Transfer Protocol)란 서버와 클라이언트가 인터넷상에서 데이터를 주고받기 위한 프로토콜(protocol)을 말한다.
  - HTTP는 계속 발전하여 HTTP/2까지 버전이 등장한 상태
  - 팀 버너스리(Tim Berners-Lee)와 그가 속한 팀은 CERN에서 HTML뿐만 아니라 웹 브라우저 및 웹 브라우저 관련 기술과 HTTP를 발명하였다.
  - 어떤 종류의 데이터도 전송할 수 있도록 설계되어있다.
- <mark>TIP</mark> HTTP와 HTTPS의 차이
  - HTTP 프로토콜
    - 웹상에서 클라이언트와 서버 간에 요청/응답으로 정보를 주고 받을 수 있는 프로토콜
  - HTTPS 프로토콜
    - 웹 통신 프로토콜인 HTTP의 보안이 강화된 버전
    - HTTPS는 소켓 통신에서 일반 텍스트를 이용하는 대신에, SSL이나 TLS 프로토콜을 통해 세션 데이터를 암호화한다. 
    - 따라서 데이터의 적절한 보호를 보장한다. 
    - HTTPS의 기본 TCP/IP포트는 443이다.

### HTTP 통신 과정 
- HTTP는 서버/클라이언트 모델을 따른다.
  - 클라이언트 -> **요청** -> 서버 -> **응답** -> 클라이언트
  - **특징**
    - 비연결 지향(Connectionless)
      - 클라이언트가 request를 서버에 보내고, 서버가 클라이언트에 요청에 맞는 response를 보내면 바로 연결을 끊는다.
    - 무상태(Stateless)
      - 연결을 끊는 순간 클라이언트와 서버의 통신은 끝나며 상태 정보를 유지하지 않는다.
  - **장점**
    - 불특정 다수를 대상으로 하는 서비스에는 적합하다.
    - 클라이언트와 서버가 계속 연결된 형태가 아니기 때문에 클라이언트와 서버 간의 최대 연결 수보다 훨씬 많은 요청과 응답을 처리할 수 있다.
  - **단점**
    - 연결을 끊어버리기 때문에, 클라이언트의 이전 상황을 알 수가 없다.
    - 이러한 특징을 무상태(Stateless)라고 말한다.
    - 이러한 특징 때문에 정보를 유지하기 위해서 Cookie와 같은 기술이 등장하게 되었다.
- ![](/images/network/http-network-connect.png)
  - 클라이언트가 원하는 서버에 접속
  - 클라이언트가 서버에 요청
  - 요청에 따른 응답 결과를 다시 클라이언트에 응답
  - 응답이 끝나고 나면 서버와 클라이언트의 연결은 끊긴다.

## [HTTP 메시지 형식](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages)
![](/images/network/http-example.png)

### HTTP 요청 메시지 형식
HTTP Request Message = **Request Header + 빈 줄 + Request Body**
- Header
  - 첫 번째 줄 (start-line)
    - 요청 메서드 + 요청 URI + HTTP 프로토콜 버전 
    - `GET /background.png HTTP/1.0`
    - `POST / HTTP 1.1`
  - 두 번째 줄 ~ (http headers)
    - Header 정보들 ('Header Name: Header Value' 형태)
    - 각 줄은 Line Feed(LF)와 Carriage Return(CR)으로 구분된다.
- 빈 줄 (empty-line)
  - 요청에 대한 모든 메타 정보가 전송되었음을 알린다.
- Body 
  - POST, PUT의 경우에만 존재  
  - 요청과 관련된 내용 (HTML 폼 콘텐츠 등)
- Ex
![](/images/network/http-request.png)

### HTTP 응답 메시지 형식
HTTP Response Message = **Response Header + 빈 줄 + Response Body**
- Header
  - 첫 번째 줄 (status-line)
    - HTTP 프로토콜 버전 + 응답 코드 + 응답 메시지 
    - `HTTP/1.1 404 Not Found.`
  - 두 번째 줄 ~ (http headers)
    - Header 정보들 ('Header Name: Header Value' 형태)
      - 날짜, 웹서버 이름, 웹서버 버전, 콘텐츠 타입, 콘텐츠 길이, 캐시 제어 방식 등
    - 각 줄은 Line Feed(LF)와 Carriage Return(CR)으로 구분된다.
- 빈 줄 (empty-line)
  - 요청에 대한 모든 메타 정보가 전송되었음을 알린다.
- Body
  - 실제 응답 리소스 데이터
  - 201, 204와 같은 상태 코드를 가진 응답에는 보통 body가 존재하지 않는다.
- Ex
![](/images/network/http-response.png)

### HTTP 기본 속성 개념
- 요청 메서드
  - 서버에게 요청의 종류를 알려주기 위해 사용
  - 최초의 웹 서버는 GET 방식만 지원
  - 각 메서드의 사용 목적
    - **GET:** 정보 요청 (SELECT)
    - **POST:** 정보 밀어넣기 (INSERT)
    - **PUT:** 정보 업데이트 (UPDATE)
    - **DELETE:** 정보 삭제 (DELETE)
    - **HEAD:** (HTTP) 헤더 정보만 요청하는 메서드. 해당 자원이 존재하는지 혹은 서버에 문제가 없는지를 확인
    - **OPTIONS:** 웹 서버가 지원하는 메서드의 종류를 요청
    - **TRACE:** 클라이언트의 요청을 그대로 반환하는 메서드. Ex) echo 서비스로 서버 상태를 확인하기 위한 목적으로 주로 사용
- 요청 URI
  - 요청하는 자원의 위치를 명시
- HTTP 프로토콜 버전
  - 웹 브라우저가 사용하는 프로토콜 버전
- [응답 상태 코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
  - 요청의 성공 여부 (200, 404 혹은 302)
- 응답 메시지 
  - 상태 코드에 대한 이해를 돕기 위해 짧고 간결하게 상태 코드에 대한 설명을 글로 나타낸 것

### URL (Uniform Resource Locator)란
- URL이란
  - 인터넷 상의 자원의 위치
  - 특정 웹 서버의 특정 파일에 접근하기 위한 경로 혹은 주소
- URL의 표현
  - 접근 프로토콜 :// IP 주소 또는 도메인 이름 (:포트번호) / 자원의 경로 / 자원의 이름
  - Ex) http://www.example.co.kr/test/index.html
  - Ex) http://localhost:8080

- <mark>TIP</mark> URI와 URL의 차이
  - **URI** (Uniform Resource Identifier)
    - 요청하는 자원의 식별자 (규약)
    - 자원을 고유하게 식별하고 위치를 지정할 수 있다.
    - URI의 하위 개념으로 URL이 포함된다. 즉, URI > URL
  - **URL** (Uniform Resource Locator)
    - 특정 웹 서버의 특정 자원에 대한 구체적인 위치 (규약의 형태)
    - 자원의 정확한 위치와 접근하기 위한 방법을 알려준다.
  - Ex) URL 이면서 URI인 경우
    - https://gmlwjd9405.github.io/tags.html
    - 자원의 고유한 식별자이면서 구체적인 위치를 명시한 형태
  - Ex) URL이 아니면서 URI인 경우 
    - https://gmlwjd9405.github.io/posts/132
    - https://gmlwjd9405.github.io/list?page=2
      - URL: https://gmlwjd9405.github.io/list
      - URI: URL + `?page=2`
    - 자원에 접근할 수 있는 위치는 `https://gmlwjd9405.github.io/list`이며, 이를 URL이라고 할 수 있다.
    - 하지만 원하는 자원을 얻기 위해서는 추가적인 식별자인 `page=2`가 필요하고 이를 포함한 내용까지가 URI라고 할 수 있다.


# 관련된 Post
- 쿠키와 세션의 차이에 대해 알고 싶으시면 [쿠키(Cookie)와 세션(Session) 이해하기](https://doooyeon.github.io/2018/09/10/cookie-and-session.html)를 참고하시기 바랍니다.
- 구체적인 HTTP 헤더의 내용에 대해 알고 싶으시면 [HTTP 헤더의 종류 및 항목](https://gmlwjd9405.github.io/2019/01/28/http-header-types.html)을 참고하시기 바랍니다.
- TCP 연결에 대해 알고 싶으시면 [TCP 3-way handshaking과 4-way handshaking](https://gmlwjd9405.github.io/2018/09/19/tcp-connection.html)을 참고하시기 바랍니다.


# Reference
> - [https://mygumi.tistory.com/139](https://mygumi.tistory.com/139)
> - [https://developer.mozilla.org/ko/docs/Web/HTTP/Status](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
> - [https://developer.mozilla.org/ko/docs/Web/HTTP/Messages](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages)
> - [https://joshua1988.github.io/web-development/web-protocols/](https://joshua1988.github.io/web-development/web-protocols/)
