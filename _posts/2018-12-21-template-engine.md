---
layout: post
title: '[Template Engine] 템플릿 엔진(Template Engine)이란'
subtitle: '템플릿 엔진의 개념과 종류 및 필요성에 대해 이해한다.'
date: 2018-12-21
author: heejeong Kwon
cover: '/images/etc/template-engine-main.png'
tags: springMVC mvc architecture structure web
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - 템플릿 엔진(Template Engine)이란
> - 템플릿 엔진(Template Engine)의 종류
  - 레이아웃 템플릿 엔진 vs 텍스트 템플릿 엔진
  - 서버 사이드 템플릿 엔진 vs 클라이언트 사이드 템플릿 엔진
  - Spring MVC 템플릿 엔진 vs Spring Boot 템플릿 엔진
> - 템플릿 엔진(Template Engine)의 필요성
  - Server Side Rendering vs Client Side Rendering

## 템플릿 엔진(Template Engine)이란
템플릿 양식과 특정 데이터 모델에 따른 입력 자료를 합성하여 결과 문서를 출력하는 소프트웨어(또는 소프트웨어 컴포넌트)를 말한다.
* 그 중 **웹 템플릿 엔진(web template engine)**이란 웹 문서가 출력되는 템플릿 엔진을 말한다. 
    * 즉, 웹 템플릿 엔진은 웹 템플릿들(web templates)과 웹 컨텐츠 정보(content information)를 처리하기 위해 설계된 소프트웨어이다. 
    * 웹 템플릿 엔진은 view code(html)와 data logic code(db connection)를 분리해주는 기능을 한다.
    * [웹 템플릿 시스템](https://nesoy.github.io/articles/2017-03/web-template) 참고
    * ![](/images/etc/template-engine-system-web.png)

## 템플릿 엔진(Template Engine)의 종류
* [템플릿 엔진의 종류](https://en.wikipedia.org/wiki/Comparison_of_web_template_engines) 참고 
    * ![](/images/etc/template-engine-system.png)

### 레이아웃 템플릿 엔진 vs 텍스트 템플릿 엔진
* **레이아웃** 템플릿 엔진
    * 중복되는 `include` 코드를 사용하지 않고도 지정된 페이지 레이아웃에 따라 페이지 타일을 조합하여 완전한 페이지로 만들어준다. 
    * Ex) Apache Tiles, Sitemesh 등
* **텍스트** 템플릿 엔진
    * 템플릿 양식에 적절한 특정 데이터를 넣어 결과 문서를 출력한다.
    * Ex) Freemarker, Thymeleaf, JSP(Java Server Pages) 등
* 둘은 역할이 다르며 섞어서 사용하는 것이지 서로 배타적인 것이 아니다. [[참고]](https://blog.outsider.ne.kr/969)

### 서버 사이드 템플릿 엔진 vs 클라이언트 사이드 템플릿 엔진
* **Server** Side Template Engine
    * <span style="background-color: #e1e1e1">서버에서 DB 혹은 API에서 가져온 데이터를 미리 정의된 Template에 넣어 html을 그려서 클라이언트에 전달해주는 역할을 한다.</span>
    * 즉, HTML 코드에서 고정적으로 사용되는 부분은 템플릿으로 만들어두고 동적으로 생성되는 부분만 템플릿 특정 장소에 끼워넣는 방식으로 동작할 수 있도록 해준다.
    * ![](/images/etc/template-engine-server-side.png)
    * 과정
        * 1) 클라이언트의 요청을 받아서 
        * 2) 필요한 데이터(DB에서 가져오거나 API에서 가져오거나)가져온다.
        * 3) 미리 정의된 Template에 해당 데이터를 적절하게 넣는다.
        * 4) 서버에서 HTML(데이터가 반영된 Template)을 그린다.
        * 5) 해당 HTML을 클라이언트에 전달한다.
    * Ex) [javascript template engine](https://colorlib.com/wp/top-templating-engines-for-javascript/)
        * EJS(Embedded JavaScript Templates), Jade(Pug), Handlebars(Handlebars.js) 등
    * Ex) [java template engine](http://kwonnam.pe.kr/wiki/java/template_engine)
        * Freemarker, Thymeleaf, Groovy, Velocity, jade4j, Handlebars(Handlebars.java), Mustache, JSP 등
        
    <!-- * 서버 사이드 템플릿 엔진의 필요성 
        * ```js``` -->

* **Client** Side Template Engine
    *  <span style="background-color: #e1e1e1">html 형태로 코드를 작성할 수 있으며, 동적으로 [DOM](https://developer.mozilla.org/ko/docs/Gecko_DOM_Reference/%EC%86%8C%EA%B0%9C)을 그리게 해주는 역할을 한다.</span> 
    * 즉, 데이터 받아서 DOM 객체에 동적으로 그려주는 프로세스를 담당한다.
    * 예를 들어 웹 페이지에서 여러 카테고리 중 탭을 선택할 때마다 같은 형식의 프레임에 내용만 바뀌어 변경되는 것을 볼 수 있다. 이런 공통적인 프레임을 미리 제작한 'template'이라고 부른다. 클라이언트에서는 이런 template을 매번 입력하거나 바꿀 수 없으므로 script 타입을 template으로 미리 만들어 사용한다. (안의 내용은 replace를 사용하여 바꾼다.)
    * ![](/images/etc/template-engine-client-side.png)
    * 과정
        * 1) 클라이언트에서 공통적인 프레임을 미리 template으로 만든다.
        * 2) 서버에서 필요한 데이터를 받는다.
        * 3) 데이터를 template을 적절한 위치에 replace하고 DOM 객체에 동적으로 그려준다.
    * Ex) Mustache, Squirrelly, Handlebars(Handlebars.js)
    * 클라이언트 사이드 템플릿 엔진의 필요성 
        * **Javascript 라이브러리로 랜더링이 끝난뒤 (즉, HTML Dom이 다 그려진 뒤)에 서버 통신 없이 화면 변경이 필요할 경우**
        * 계속해서 페이지를 이동하여 서버 쪽으로 호출이 발생한다면 서버 사이드 템플릿 엔진을 이용하면 되는데, 단일 화면에서 특정 이벤트에 따라 화면이 계속 변경되어야 하는 경우는 javascript로 html을 렌더링하는 경우가 많다.
        * 즉 이렇게 단일 화면에서의 화면 변경에서는 서버 쪽을 사용하지 않고 화면을 그리기(이하 렌더링)위해 javascript 안에 html 코드를 작성해야 하는데, 이때 클라이언트 사이드 템플릿 엔진을 사용하지 않고 아래와 같이 javascript로 html을 렌더링하는 경우에는 여러 문제가 있다.
        * ```js```
        1. html 코드(문자열)의 오타를 찾기 어렵다. (태그 하나가 빠지거나, attrubute가 오타가 나도 IDE에서 감지할 수 없다.)
        2. 렌더링 해야 할 코드가 늘어나면 늘어날수록 수정하기 어려워진다. (Dom 형태를 파악하기 어렵다.)

<!-- 클라이언트 측 브라우저는 HTML 템플릿, JSON/XML 데이터 및 템플릿 엔진 라이브러리를 서버에서 로드합니다. 
템플릿 엔진은 클라이언트의 브라우저에서 템플릿과 데이터를 사용하여 최종 HTML을 생성합니다. 
그러나 일부 HTML 템플릿은 데이터를 처리하고 서버 측에서 최종 HTML 페이지를 생성합니다. -->


### Spring MVC 템플릿 엔진 vs Spring Boot 템플릿 엔진
Java Object에서 데이터를 생성하여 Template에 넣어주면 템플릿 엔진에서 Template에 맞게 변환하여 html 파일을 생성하는 역할을 한다.
* [Spring Template Engine 참고](https://www.baeldung.com/spring-template-engines)
    * JSP, Thymeleaf, Groovy, Freemarker, Jade4j, JMustache, Pebble, Handlebars
    * **추천** 
        * Thymeleaf
    * 비추천
        * Velocity는 Spring 버전 4.3부터 사용을 중단한다.
* [Spring Boot Template Engine 공식 지원 템플릿](https://docs.spring.io/spring-boot/docs/2.0.0.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-template-engines)
    * Mustache, Thymeleaf, Groovy, Freemarker
    * **추천** 
        * [Handlebars](https://jojoldu.tistory.com/255)
        * Mustache
    * 비추천
        * Freemarker는 몇 년 동안 업데이트가 안되고 있어, SpringBoot에선 권장하지 않는 템플릿 엔진이다. 
        * Velocity는 SpringBoot에서 지원하지 않는다.
        * JSP는 임베디드 서블릿 컨테이너를 사용하는 SpringBoot에서 [제한 사항](https://docs.spring.io/spring-boot/docs/2.0.0.RELEASE/reference/htmlsingle/#boot-features-jsp-limitations)이 있다.


## 템플릿 엔진(Template Engine)의 필요성
1. 많은 코드를 줄일 수 있다
* 대부분의 Template Engine은 기존의 HTML에 비해서 간단한 문법을 사용한다.
2. 재사용성이 높다
* 웹페이지 혹은 웹앱을 만들 때 똑같은 디자인의 페이지에 보이는 데이터만 바뀌는 경우가 굉장히 많다.
3. 유지보수에 용이하다
* 하나의 Template을 만들어 여러 페이지를 렌더링하는 작업에는 또 다른 이점이 있다.

### Server Side Rendering vs Client Side Rendering
* Q. 서버 측에서는 API만 제공하고 클라이언트에서 동적으로 웹 페이지를 구성하면 되지 않나? <br>
(즉, 서버는 API만 제공하고 모든 웹 페이지 구성은 클라이언트가 처리한다.)
* Q. 굳이 서버단 렌더링까지 할 필요가 있느냐, 렌더링은 클라이언트에서만 하도록 하면 뷰단 로직이 분산되지 않고 좋지 않나? <br>
(위와 같은 의미의 질문)

* A. [https://www.slipp.net/questions/368#answer-1312](https://www.slipp.net/questions/368#answer-1312) 참고
* A. [https://www.clien.net/service/board/park/5699595](https://www.clien.net/service/board/park/5699595) 참고


# 관련된 Post
* Spring MVC Framework에 대해 알고 싶으시면 [Spring MVC 이해하기](http://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html)를 참고하시기 바랍니다.


# References
> - [https://en.wikipedia.org/wiki/Web_template_system](https://en.wikipedia.org/wiki/Web_template_system)
> - [https://nesoy.github.io/articles/2017-03/web-template](https://nesoy.github.io/articles/2017-03/web-template)
> - [https://opentutorials.org/course/2136/11915](https://opentutorials.org/course/2136/11915)
> - [https://jojoldu.tistory.com/23](https://jojoldu.tistory.com/23)
> - [http://ingorae.tistory.com/402](http://ingorae.tistory.com/402)
> - [http://show-me-the-money.tistory.com/56](http://show-me-the-money.tistory.com/56)
> - [https://jqueryhouse.com/top-5-best-javascript-template-engines/](https://jqueryhouse.com/top-5-best-javascript-template-engines/)
> - [https://ian90.tistory.com/35](https://ian90.tistory.com/35)
> - [http://new93helloworld.tistory.com/139](http://new93helloworld.tistory.com/139)
> - [http://kwonnam.pe.kr/wiki/java/template_engine](http://kwonnam.pe.kr/wiki/java/template_engine)