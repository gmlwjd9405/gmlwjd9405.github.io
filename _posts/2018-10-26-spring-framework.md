---
layout: post
title: '[Spring] Spring Framework란'
subtitle: 'Spring Framework의 개념과 특징에 대해 이해한다.'
date: 2018-10-26
author: heejeong Kwon
cover: '/images/spring-framework/spring-framework-main.png'
tags: spring framework
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 프레임워크(Framework)란
> - 스프링 프레임워크(Spring Framework)란
> - 스프링 프레임워크의 특징 

## 프레임워크(Framework)란
"소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 형태로 클래스들을 제공하는 것" ***- 랄프 존슨(Ralph Johnson) -***

1. Framework는 구조 품질을 보장한다.
    * 구조 품질: "Great Software is well-designed, well-coded, and easy to maintain, reuse, and extend."
    * **Framework = Design Pattern + Class Library**
    * 구체적이며 확장 가능한 기반 코드를 가지고 있다.
        * 소프트웨어의 구조 및 기반이 되는 클래스를 제공한다.
    * 설계자가 의도하는 여러 디자인 패턴의 집합으로 구성되어 있다.
2. Framework는 반제품이다.
    * Application의 틀과 구조를 결정한다.
    * Application 코드의 상당 부분을 제공한다.
        * Application 코드는 Framework에 설계되어 있는 제어 흐름에 따라 동작한다.
        * 즉, Framework 코드가 그 위에 개발된 개발자의 User 코드를 호출하고 제어한다.
    * 개발자는 Application의 핵심 로직에만 집중할 수 있다.
3. Framework의 장점
    * 생산성
        * business losic에만 집중할 수 있어 생산성이 증대
    * 코드 품질
        * 코드의 재사용 및 유지 보수 용이
        * 확장성을 가진 코드 설계 가능

<mark>프레임워크 vs 라이브러리</mark>
* 프레임워크란
    * 소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 형태로 클래스들을 제공하는 것
    * Ex) 자동차의 프레임, 즉 기본적으로 구성하고 있는 뼈대
* 라이브러리란 
    * 자주 사용되는 로직을 재사용하기 편리하도록 잘 정리한 일련의 코드들의 집합
    * Ex) 자동차의 기능을 하는 부품


## 스프링 프레임워크(Spring Framework)란
### 스프링(Spring)의 개념
자바 엔터프라이즈 개발을 편하게 해주는 경량급 오픈소스 애플리케이션 프레임워크

* **Lightweight Java Applicaion Framework**
    * 목표: **POJO 기반의** Enterprise Application 개발을 쉽고 편하게 할 수 있도록 한다.
    * Java Application을 개발하는데 필요한 하부구조(Infrastructure)를 포괄적으로 제공한다.
    * Spring이 하부구조를 처리하기 때문에 개발자는 Application 개발에 집중할 수 있다.
* 자바 플랫폼을 위한 오픈소스 애플리케이션 프레임워크
* 간단히 *스프링(Spring)*이라고도 불린다. 
* 동적인 웹 사이트를 개발하기 위한 여러 가지 서비스를 제공한다.
* 대한민국 공공기관의 웹 서비스 개발 시 사용을 권장하고 있는 전자 정부 표준 프레임워크의 기반 기술

<mark>POJO와 EJB</mark>
* POJO
    * Plain Old Java Object
    * 상속, 인터페이스가 필요없는 아주 단순하고 가벼운 객체를 의미한다.
    * 원하는 business logic만 넣을 수 있도록 돕는다.
* EJB
    * Enterprise JavaBeans
    * 기업 환경의 시스템을 구현하기 위한 서버 측 컴포넌트 모델이다. 
    * 즉, EJB는 애플리케이션의 업무 로직을 가지고 있는 서버 애플리케이션으로 특정 환경에 종속적이고 무겁다.

### 스프링(Spring)의 주요 특징
![](/images/spring-framework/spring-feature.png)
1. DI(Dependency Injection)
    * **의존 관계 주입**
    * 각각의 계층이나 서비스들 간에 의존성이 존재할 경우 Spring이 서로 연결시켜준다.
    * POJO 객체들 사이의 의존 관계를 Spring이 알아서 연관성을 맺어준다.
    * Ex) 다양한 DB 사용이 가능
2. AOP(Aspect Orientated Programming)
    * **관점 중심 프로그래밍**
    * Spring은 핵심적인 비즈니스 로직과 관련이 없으나 여러 곳에서 공통적으로 쓰이는 기능들을 분리(공통 관심사를 분리)하여 개발하고 실행 시에 서로 조합할 수 있는 AOP를 지원한다. 
    * 이를 통해 코드를 단순하고 깔끔하게 작성할 수 있다.
    * ![](/images/oop-solid/aop-problem.png)
    * 횡단 관심을 수행하는 코드(Logging, Security, Transaction 등)는 aspect라는 특별한 객체로 모듈화하고 weaving이라는 작업을 통해 모듈화한 코드를 핵심 기능에 끼워넣을 수 있다.
    * 참고: [https://gmlwjd9405.github.io/2018/07/05/oop-solid.html](https://gmlwjd9405.github.io/2018/07/05/oop-solid.html)
3. Portable Service Abstraction
    * **이식 가능한 서비스 추상화**
    * Spring은 완성도가 높은 라이브러리와 연결할 수 있는 인터페이스를 제공한다.
    * 즉, 다른 프레임워크들과의 통합을 지원한다.
    * ![](/images/spring-framework/spring-feature-3.png)

### 스프링(Spring)의 구성 요소
![](/images/spring-framework/spring-structure.png)
* Core Container 중 Bean Container는 POJO 객체를 관리한다.
* Spring에서 제공하는 다양한 기능 중 필요한 것을 선택적으로 사용한다.
* 참고: [https://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/overview.html](https://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/overview.html)



# References
> - [wikipedia - 스프링프레임워크](https://ko.wikipedia.org/wiki/%EC%8A%A4%ED%94%84%EB%A7%81_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC)
> - [http://moolgogiheart.tistory.com/87](http://moolgogiheart.tistory.com/87)
> - [https://www.quora.com/What-is-the-history-of-The-Spring-Framework](https://www.quora.com/What-is-the-history-of-The-Spring-Framework)
> - [https://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/overview.html](https://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/overview.html)
> - [https://12bme.tistory.com/157](https://12bme.tistory.com/157)