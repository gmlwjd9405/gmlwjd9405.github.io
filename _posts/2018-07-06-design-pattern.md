---
layout: post
title: '[Design Pattern] 디자인 패턴 종류'
subtitle: '디자인 패턴의 개념과 종류를 이해한다.'
date: 2018-07-06
author: heejeong Kwon
cover: '/images/design-pattern/design-pattern-main.png'
tags: 디자인패턴 design-pattern
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 디자인 패턴의 개념을 이해한다.
> - 디자인 패턴의 구조를 이해한다.
> - 디자인 패턴의 분류를 이해한다.
> - 디자인 패턴의 종류를 이해한다.

## 디자인 패턴이란
* 소프트웨어를 설계할 때 특정 맥락에서 자주 발생하는 고질적인 문제들이 또 발생했을 때 재사용할 할 수있는 훌륭한 해결책
* "바퀴를 다시 발명하지 마라(Don't reinvent the wheel)"
* 이미 만들어져서 잘 되는 것을 처음부터 다시 만들 필요가 없다는 의미이다.
* 패턴이란
  * 각기 다른 소프트웨어 모듈이나 기능을 가진 다양한 응용 소프트웨어 시스템들을 개발할 때도 서로 간에 공통되는 설계 문제가 존재하며 이를 처리하는 해결책 사이에도 공통점이 있다. 이러한 유사점을 패턴이라 한다.
  * 패턴은 공통의 언어를 만들어주며 팀원 사이의 의사 소통을 원활하게 해주는 아주 중요한 역할을 한다.  

## 디자인 패턴 구조
* 콘텍스트(context)
  * 문제가 발생하는 여어 상황을 기술한다. 즉, 패턴이 적용될 수 있는 상황을 나타낸다.
  * 경우에 따라서는 패턴이 유용하지 못한 상황을 나타내기도 한다.
* 문제(problem)
  * 패턴이 적용되어 해결될 필요가 있는 여러 디자인 이슈들을 기술한다.
  * 이때 여러 제약 사항과 영향력도 문제 해결을 위해 고려해야 한다.
* 해결(solution)
  * 문제를 해결하도록 설계를 구성하는 요소들과 그 요소들 사이의 관계, 책임, 협력 관계를 기술한다.
  * 해결은 반드시 구체적인 구현 방법이나 언어에 의존적이지 않으며 다양한 상황에 적용할 수 있는 일종의 템플릿이다.

## 디자인 패턴의 종류
* GoF 디자인 패턴
  * GoF(Gang of Fout)라 불리는 사람들
    * 에리히 감마(Erich Gamma), 리차드 헬름(Richard Helm), 랄프 존슨(Ralph Johnson), 존 블리시디스(John Vissides)
    * 소프트웨어 개발 영역에서 디자인 패턴을 구체화하고 체계화한 사람들
    * 23가지의 디자인 패턴을 정리하고 각각의 디자인 패턴을 생성(Creational), 구조(Structural), 행위(Behavioral) 3가지로 분류했다.

* GoF 디자인 패턴의 분류
  * ![](/images/design-pattern/types-of-designpattern.png)

1. 생성(Creational) 패턴
* 객체 생성에 관련된 패턴
* 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공한다.
2. 구조(Structural) 패턴
* 클래스나 객체를 조합해 더 큰 구조를 만드는 패턴
* 예를 들어 서로 다른 인터페이스를 지닌 2개의 객체를 묶어 단일 인터페이스를 제공하거나 객체들을 서로 묶어 새로운 기능을 제공하는 패턴이다.
3. 행위(Behavioral)
* 객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴
* 한 객체가 혼자 수행할 수 없는 작업을 여러 개의 객체로 어떻게 분배하는지, 또 그렇게 하면서도 객체 사이의 결합도를 최소화하는 것에 중점을 둔다.


* GoF 디자인 패턴의 종류
  1. 생성(Creational) 패턴
  * 추상 팩토리(Abstract Factory)
    * 구제적인 클래스에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 인터페이스를 제공하는 패턴
  * 팩토리 메서드(Factory Method)
    * 객체 생성 처리를 서브 클래스로 분리해 처리하도록 캡슐화하는 패턴
  * 싱글턴(Singleton)
    * 전역 변수를 사용하지 않고 객체를 하나만 생성하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 패턴
  2. 구조(Structural) 패턴
  * 컴퍼지트(Composite)
    * 여러 개의 객체들로 구성된 복합 객체와 단일 객체를 클라이언트에서 구별 없이 다루게 해주는 패턴
  * 데커레이터(Decorator)
    * 객체의 결합을 통해 기능을 동적으로 유연하게 확장할 수 있게 해주는 패턴
  3. 행위(Behavioral) 패턴
  * 옵서버(Observer)
    * 한 객체의 상태 변화에 따라 다른 객체의 상태도 연동되도록 일대다 객체 의존 관계를 구성하는 패턴
  * 스테이트(State)
    * 객체의 상태에 따라 객체의 행위 내용을 변경해주는 패턴
  * 스트래티지(Strategy)
    * 행위를 클래스로 캡슐화해 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴
  * 템플릿 메서드(Template Method)
    * 어떤 작업을 처리하는 일부분을 서브 클래스로 캡슐화해 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴
  * 커맨드(Command)
    * 실행될 기능을 캡슐화함으로써 주어진 여러 기능을 실행할 수 있는 재사용성이 높은 클래스를 설계하는 패턴



# 관련된 Post
* 스트래티지(Strategy) 패턴에 대해 알고 싶으시면 [스트래티지(Strategy) 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)을 참고하시기 바랍니다.
* 싱글턴(Singleton) 패턴에 대해 알고 싶으시면 [싱글턴(Singleton) 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)을 참고하시기 바랍니다.
* 커맨드(Command) 패턴에 대해 알고 싶으시면 [커맨드(Command) 패턴](https://gmlwjd9405.github.io/2018/07/07/command-pattern.html)을 참고하시기 바랍니다.
* 옵저버(Observer) 패턴에 대해 알고 싶으시면 [옵저버(Observer) 패턴](https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html)을 참고하시기 바랍니다.
* 데코레이터(Decorator) 패턴에 대해 알고 싶으시면 [데코레이터(Decorator) 패턴](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)을 참고하시기 바랍니다.
* 템플릿 메서드(Template Method) 패턴에 대해 알고 싶으시면 [템플릿 메서드(Template Method) 패턴](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)을 참고하시기 바랍니다.
* 팩토리 메서드(Factory Method) 패턴에 대해 알고 싶으시면 [팩토리 메서드(Factory Method) 패턴](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)을 참고하시기 바랍니다.
* 추상 팩토리(Abstract Factory) 패턴에 대해 알고 싶으시면 [추상 팩토리(Abstract Factory) 패턴](https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html)을 참고하시기 바랍니다.
* 컴퍼지트(Composite) 패턴에 대해 알고 싶으시면 [컴퍼지트(Composite) 패턴에](https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html)을 참고하시기 바랍니다.


# References
> - [JAVA 객체지향 디자인 패턴, 한빛미디어](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968480911&orderClick=JAj)
