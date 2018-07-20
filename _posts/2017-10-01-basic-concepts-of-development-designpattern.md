---
layout: post
title: '[기술 면접 질문] 기술 면접 예상 질문 대비하기 - 디자인패턴(Design Pattern)편'
subtitle: '기술 면접 예상 질문 대비하기 - 디자인패턴(Design Pattern)편'
date: 2017-10-01
author: heejeong Kwon
cover: '/images/basic-concepts-of-development/basic-concepts-of-development-main.png'
tags: 면접 기본개념 디자인패턴 design-pattern
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## <mark>디자인패턴(Design Pattern)편</mark>  
<span style="color:#4d0000">디자인패턴(design pattern)과 관련된 기본적인 개념을 이해하고 기술 면접에 대비하자!</span>  

<br> <span style="background-color: #e1e1e1">계속해서 추가할 예정입니다!<span>

### 클래스 다이어그램 작성법
> - 관련 POST
> - [https://gmlwjd9405.github.io/2018/07/04/class-diagram.html](https://gmlwjd9405.github.io/2018/07/04/class-diagram.html)

### 디자인 패턴의 이해
* 디자인 패턴이란
  * 소프트웨어를 설계할 때 특정 맥락에서 자주 발생하는 고질적인 문제들이 또 발생했을 때 재사용할 할 수있는 훌륭한 해결책
  * "바퀴를 다시 발명하지 마라(Don't reinvent the wheel)"
  * 이미 만들어져서 잘 되는 것을 처음부터 다시 만들 필요가 없다는 의미이다.
* GoF 디자인 패턴의 분류
  * GoF(Gang of Fout)라 불리는 사람들이 23가지의 디자인 패턴을 정리하고 각각의 디자인 패턴을 생성(Creational), 구조(Structural), 행위(Behavioral) 3가지로 분류했다.
  * 에리히 감마(Erich Gamma), 리차드 헬름(Richard Helm), 랄프 존슨(Ralph Johnson), 존 블리시디스(John Vissides)
* GoF 디자인 패턴의 종류
![](/images/design-pattern/types-of-designpattern.png)
> - 관련 POST
> - [https://gmlwjd9405.github.io/2018/07/06/design-pattern.html](https://gmlwjd9405.github.io/2018/07/06/design-pattern.html)

### 스트래티지 패턴
* **행위를 클래스로 캡슐화해** 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴
  * 같은 문제를 해결하는 여러 알고리즘이 클래스별로 캡슐화되어 있고 이들이 필요할 때 교체할 수 있도록 함으로써 동일한 문제를 다른 알고리즘으로 해결할 수 있게 하는 디자인 패턴
  * '행위(Behavioral) 패턴'의 하나
    * 아래 참고
* 즉, **전략을 쉽게 바꿀 수 있도록** 해주는 디자인 패턴이다.
  * 전략이란
    * 어떤 목적을 달성하기 위해 일을 수행하는 방식, 비즈니스 규칙, 문제를 해결하는 알고리즘 등
* 특히 게임 프로그래밍에서 게임 캐릭터가 자신이 처한 상황에 따라 공격이나 행동하는 방식을 바꾸고 싶을 때 스트래티지 패턴은 매우 유용하다.
* ![](/images/design-pattern-strategy/strategy-pattern.png)
* 역할이 수행하는 작업
  * Strategy
    * 인터페이스나 추상 클래스로 외부에서 동일한 방식으로 알고리즘을 호출하는 방법을 명시
  * ConcreteStrategy
    * 스트래티지 패턴에서 명시한 알고리즘을 실제로 구현한 클래스
  * Context
    * 스트래티지 패턴을 이용하는 역할을 수행한다.
    * 필요에 따라 동적으로 구체적인 전략을 바꿀 수 있도록 setter 메서드('집약 관계')를 제공한다.
> - 관련 POST
> - [https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)


### 싱글턴 패턴
* 전역 변수를 사용하지 않고 **객체를 하나만 생성** 하도록 하며, 생성된 객체를 **어디에서든지 참조할 수 있도록** 하는 패턴
  * '생성(Creational) 패턴'의 하나
* ![](/images/design-pattern-singleton/singleton-example.png){: width="150" height="100"}
* 역할이 수행하는 작업
  * Singleton
    * 하나의 인스턴스만을 생성하는 책임이 있으며 getInstance 메서드를 통해 모든 클라이언트에게 동일한 인스턴스를 반환하는 작업을 수행한다.
> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)


### 커맨드 패턴
* **실행될 기능을 캡슐화함으로써** 주어진 여러 기능을 실행할 수 있는 재사용성이 높은 클래스를 설계하는 패턴
  * 즉, 이벤트가 발생했을 때 실행될 기능이 다양하면서도 변경이 필요한 경우에 이벤트를 발생시키는 클래스를 변경하지 않고 재사용하고자 할 때 유용하다.
  * '행위(Behavioral) 패턴'의 하나
* ![](/images/design-pattern-command/command-pattern.png){: width="370" height="200"}
* 실행될 기능을 캡슐화함으로써 기능의 실행을 요구하는 호출자(Invoker) 클래스와 실제 기능을 실행하는 수신자(Receiver) 클래스 사이의 의존성을 제거한다.
* 따라서 **실행될 기능의 변경에도 호출자 클래스를 수정 없이 그대로 사용** 할 수 있도록 해준다.
* 역할이 수행하는 작업
  * Command
    * 실행될 기능에 대한 인터페이스
    * 실행될 기능을 execute 메서드로 선언함
  * ConcreteCommand
    * 실제로 실행되는 기능을 구현
    * 즉, Command라는 인터페이스를 구현함
  * Invoker
    * 기능의 실행을 요청하는 호출자 클래스
  * Receiver
    * ConcreteCommand에서 execute 메서드를 구현할 때 필요한 클래스
    * 즉, ConcreteCommand의 기능을 실행하기 위해 사용하는 수신자 클래스
> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/07/07/command-pattern.html](https://gmlwjd9405.github.io/2018/07/07/command-pattern.html)


### 옵서버 패턴
* 한 객체의 상태 변화에 따라 다른 객체의 상태도 연동되도록 **일대다 객체 의존 관계를 구성** 하는 패턴
  * 데이터의 변경이 발생했을 경우 **상대 클래스나 객체에 의존하지 않으면서 데이터 변경을 통보하고자 할 때** 유용하다.
    * Ex) 새로운 파일이 추가되거나 기존 파일이 삭제되었을 때 탐색기는 다른 탐색기에게 즉시 변경을 통보해야 한다.
    * Ex) 차량 연료량 클래스는 연료량이 부족한 경우 연료량에 관심을 가지는 구체적인 클래스(연료량 부족 경고 클래스, 주행 가능 거리 출력 클래스)에 직접 의존하지 않는 방식으로 연료량 변화를 통보해야 한다.
  * '행위(Behavioral) 패턴'의 하나
* ![](/images/design-pattern-observer/observer-pattern.png)
* 옵저버 패턴은 통보 대상 객체의 관리를 Subject 클래스와 Obaserver 인터페이스로 일반화한다.
  * 이를 통해 데이터 변경을 통보하는 클래스(ConcreteSubject)는 통보 대상 클래스나 객체(ConcreteObserver)에 대한 의존성을 없앨 수 있다.
  * 결과적으로 통보 대상 클래스나 대상 객체의 변경에도 **통보하는 클래스(ConcreteSubject)를 수정 없이 그대로 사용할 수 있다.**
* 역할이 수행하는 작업
  * Observer
    * 데이터의 변경을 통보 받는 인터페이스
    * 즉, Subject에서는 Observer 인터페이스의 update 메서드를 호출함으로써 ConcreteSubject의 데이터 변경을 ConcreteObserver에게 통보한다.
  * Subject
    * ConcreteObserver 객체를 관리하는 요소
    * Observer 인터페이스를 참조해서 ConcreteObserver를 관리하므로 ConcreteObserver의 변화에 독립적일 수 있다.
  * ConcreteSubject
    * 변경 관리 대상이 되는 데이터가 있는 클래스(통보하는 클래스)
    * 데이터 변경을 위한 메서드인 setState가 있다.
    * setState 메서드에서는 자신의 데이터인 subjectState를 변경하고 Subject의 notifyObservers 메서드를 호출해서 ConcreteObserver 객체에 변경을 통보한다.
  * ConcreteObserver
    * ConcreteSubject의 변경을 통보받는 클래스
    * Observer 인터페이스의 update 메서드를 구현함으로써 변경을 통보받는다.
    * 변경된 데이터는 ConcreteSubject의 getState 메서드를 호출함으로써 변경을 조회한다.

> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html](https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html)

### 데커레이터 패턴
* **객체의 결합** 을 통해 **기능을 동적으로 유연하게 확장** 할 수 있게 해주는 패턴
  * 즉, 기본 기능에 추가할 수 있는 기능의 종류가 많은 경우에 **각 추가 기능을 Decorator 클래스로 정의** 한 후 필요한 Decorator 객체를 조합함으로써 **추가 기능의 조합을 설계** 하는 방식이다.
    * Ex) 기본 도로 표시 기능에 차선 표시, 교통량 표시, 교차로 표시, 단속 카메라 표시의 4가지 추가 기능이 있ㅇ을 때 추가 기능의 모든 조합은 15가지가 된다. 데코레이터 패턴을 이용하여 필요 추가 기능의 조합을 동적으로 생성할 수 있다.
  * '구조(Structural) 패턴'의 하나
* ![](/images/design-pattern-decorator/decorator-pattern.png)
* 기본 기능에 추가할 수 있는 많은 종류의 부가 기능에서 파생되는 다양한 조합을 동적으로 구현할 수 있는 패턴이다.
* 역할이 수행하는 작업
  * Component
    * 기본 기능을 뜻하는 ConcreteComponent와 추가 기능을 뜻하는 Decorator의 공통 기능을 정의
    * 즉, 클라이언트는 Component를 통해 실제 객체를 사용함
  * ConcreteComponent
    * 기본 기능을 구현하는 클래스
  * Decorator
    * 많은 수가 존재하는 구체적인 Decorator의 공통 기능을 제공
  * ConcreteDecoratorA, ConcreteDecoratorB
    * Decorator의 하위 클래스로 기본 기능에 추가되는 개별적인 기능을 뜻함

> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)

### 템플릿 메서드 패턴

### 팩토리 메서드 패턴

### 추상 팩토리 패턴

### 컴퍼지트 패턴


---

## 관련된 Post
* 알고리즘: [기술 면접 예상 질문 대비하기 - 알고리즘(Algorithm)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-algorithm.html) 을 참고하시기 바랍니다.
* 데이터베이스: [기술 면접 예상 질문 대비하기 - 데이터베이스(DB)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-db.html) 을 참고하시기 바랍니다.
* 디자인패턴: [기술 면접 예상 질문 대비하기 - 디자인패턴(Design Pattern)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-designpattern.html) 을 참고하시기 바랍니다.
* Java: [기술 면접 예상 질문 대비하기 - Java편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-java.html) 을 참고하시기 바랍니다.
* 운영체제: [기술 면접 예상 질문 대비하기 - 운영체제(OS)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-os.html) 을 참고하시기 바랍니다.


## References
<!-- > - [http://hahahoho5915.tistory.com/16](http://hahahoho5915.tistory.com/16) -->
