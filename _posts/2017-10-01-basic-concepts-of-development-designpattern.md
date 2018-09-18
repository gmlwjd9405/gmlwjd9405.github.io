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
<br> [Do-Hee의 tech-interview](https://github.com/Do-Hee/tech-interview) - 이 Github 페이지와 동기화되어 있습니다.

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
* 예시
  * 로봇 만들기

> - 관련 POST
> - [https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)


### 싱글턴 패턴
* 전역 변수를 사용하지 않고 **객체를 하나만 생성** 하도록 하며, 생성된 객체를 **어디에서든지 참조할 수 있도록** 하는 패턴
  * '생성(Creational) 패턴'의 하나
* ![](/images/design-pattern-singleton/singleton-example.png){: width="150" height="100"}
* 역할이 수행하는 작업
  * Singleton
    * 하나의 인스턴스만을 생성하는 책임이 있으며 getInstance 메서드를 통해 모든 클라이언트에게 동일한 인스턴스를 반환하는 작업을 수행한다.
* 예시
  * 프린터 관리자 만들기

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
* 예시
  * 만능 버튼 만들기

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
* 예시
  * 여러 가지 방식으로 성적 출력하기

> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html](https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html)

### 데커레이터 패턴
* **객체의 결합** 을 통해 **기능을 동적으로 유연하게 확장** 할 수 있게 해주는 패턴
  * 즉, 기본 기능에 추가할 수 있는 기능의 종류가 많은 경우에 **각 추가 기능을 Decorator 클래스로 정의** 한 후 필요한 Decorator 객체를 조합함으로써 **추가 기능의 조합을 설계** 하는 방식이다.
    * Ex) 기본 도로 표시 기능에 차선 표시, 교통량 표시, 교차로 표시, 단속 카메라 표시의 4가지 추가 기능이 있을 때 추가 기능의 모든 조합은 15가지가 된다.
    * -> 데코레이터 패턴을 이용하여 필요 추가 기능의 조합을 동적으로 생성할 수 있다.
  * '구조(Structural) 패턴'의 하나
* ![](/images/design-pattern-decorator/decorator-pattern.png){: width="500" height="300"}
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
    * ConcreteDecorator 클래스는 ConcreteComponent 객체에 대한 참조가 필요한데, 이는 Decorator 클래스에서 Component 클래스로의 '합성(composition) 관계'를 통해 표현됨
* 예시
  * 도로 표시 방법 조합하기

> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)

### 템플릿 메서드 패턴
* 어떤 작업을 처리하는 일부분을 **서브 클래스로 캡슐화해** 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴
  * 즉, **전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 코드 중복을 최소화** 할 때 유용하다.
  * 다른 관점에서 보면 동일한 기능을 상위 클래스에서 정의하면서 확장/변화가 필요한 부분만 서브 클래스에서 구현할 수 있도록 한다.
  * 예를 들어, 전체적인 알고리즘은 상위 클래스에서 구현하면서 다른 부분은 하위 클래스에서 구현할 수 있도록 함으로써 전체적인 알고리즘 코드를 재사용하는 데 유용하도록 한다.
  * '행위(Behavioral) 패턴'의 하나
* ![](/images/design-pattern-template-method/template-method-pattern.png){: width="370" height="250"}
* 역할이 수행하는 작업
  * AbstractClass
    * 템플릿 메서드를 정의하는 클래스
    * 하위 클래스에 공통 알고리즘을 정의하고 하위 클래스에서 구현될 기능을 primitive 메서드 또는 hook 메서드로 정의하는 클래스
  * ConcreteClass
    * 물려받은 primitive 메서드 또는 hook 메서드를 구현하는 클래스
    * 상위 클래스에 구현된 템플릿 메서드의 일반적인 알고리즘에서 하위 클래스에 적합하게 primitive 메서드나 hook 메서드를 오버라이드하는 클래스
* 예시
  * 여러 회사의 모터 지원하기

> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)


### 팩토리 메서드 패턴
* **객체 생성 처리를 서브 클래스로 분리** 해 처리하도록 캡슐화하는 패턴
  * 즉, 객체의 생성 코드를 별도의 클래스/메서드로 분리함으로써 객체 생성의 변화에 대비하는 데 유용하다.
  * 특정 기능의 구현은 개별 클래스를 통해 제공되는 것이 바람직한 설계다.
    * 기능의 변경이나 상황에 따른 기능의 선택은 해당 객체를 생성하는 코드의 변경을 초래한다.
    * 상황에 따라 적절한 객체를 생성하는 코드는 자주 중복될 수 있다.
    * 객체 생성 방식의 변화는 해당되는 모든 코드 부분을 변경해야 하는 문제가 발생한다.
  * [스트래티지 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html), [싱글턴 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html), [템플릿 메서드 패턴](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)을 사용한다.
  * '생성(Creational) 패턴'의 하나
* ![](/images/design-pattern-factory-method/factory-method-pattern.png)
* 역할이 수행하는 작업
  * Product
    * 팩토리 메서드로 생성될 객체의 공통 인터페이스
  * ConcreteProduct
    * 구체적으로 객체가 생성되는 클래스
  * Creator
    * 팩토리 메서드를 갖는 클래스
  * ConcreteCreator
    * 팩토리 메서드를 구현하는 클래스로 ConcreteProduct 객체를 생성
* 팩토리 메서드 패턴의 개념과 적용 방법
  1. 객체 생성을 전담하는 별도의 **Factory 클래스 이용**
  2. **상속 이용**: 하위 클래스에서 적합한 클래스의 객체를 생성
* 예시
  * 여러 가지 방식의 엘리베이터 스케줄링 방법 지원하기

> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)


### 추상 팩토리 패턴
* 구체적인 클래스에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 인터페이스를 제공하는 패턴
  * 즉, 관련성 있는 여러 종류의 객체를 일관된 방식으로 생성하는 경우에 유용하다.
  * [싱글턴 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html), [팩토리 메서드 패턴](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)을 사용한다.
  * '생성(Creational) 패턴'의 하나
* ![](/images/design-pattern-abstract-factory/abstract-factory-pattern.png)
* 역할이 수행하는 작업
  * AbstractFactory
    * 실제 팩토리 클래스의 공통 인터페이스
  * ConcreteFactory
    * 구체적인 팩토리 클래스로 AbstractFactory 클래스의 추상 메서드를 오버라이드함으로써 구체적인 제품을 생성한다.
  * AbstractProduct
    * 제품의 공통 인터페이스
  * ConcreteProduct
    * 구체적인 팩토리 클래스에서 생성되는 구체적인 제품
* 예시
  * 엘리베이터 부품 업체 변경하기

> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html](https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html)


### 컴퍼지트 패턴
* 여러 개의 객체들로 구성된 복합 객체와 단일 객체를 클라이언트에서 구별 없이 다루게 해주는 패턴
  * 즉, **전체-부분의 관계(Ex. Directory-File)를 갖는 객체들** 사이의 관계를 정의할 때 유용하다.
  * 또한 클라이언트는 전체와 부분을 구분하지 않고 **동일한 인터페이스** 를 사용할 수 있다.
  * '구조(Structural)'의 하나
* ![](/images/design-pattern-composite/composite-pattern.png)
* 역할이 수행하는 작업
  * Component
    * 구체적인 부분
    * 즉 Leaf 클래스와 전체에 해당하는 Composite 클래스에 공통 인터페이스를 정의
  * Leaf
    * 구체적인 부분 클래스
    * Composite 객체의 부품으로 설정
  * Composite
    * 전체 클래스
    * 복수 개의 Component를 갖도록 정의
    * 그러므로 복수 개의 Leaf, 심지어 복수 개의 Composite 객체를 부분으로 가질 수 있음
* 예시
  * 컴퓨터에 추가 장치 지원하기

> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html](https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html)


---

## 관련된 Post
* 알고리즘: [기술 면접 예상 질문 대비하기 - 알고리즘(Algorithm)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-algorithm.html) 을 참고하시기 바랍니다.
* 데이터베이스: [기술 면접 예상 질문 대비하기 - 데이터베이스(DB)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-db.html) 을 참고하시기 바랍니다.
* 디자인패턴: [기술 면접 예상 질문 대비하기 - 디자인패턴(Design Pattern)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-designpattern.html) 을 참고하시기 바랍니다.
* Java: [기술 면접 예상 질문 대비하기 - Java편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-java.html) 을 참고하시기 바랍니다.
* 운영체제: [기술 면접 예상 질문 대비하기 - 운영체제(OS)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-os.html) 을 참고하시기 바랍니다.


## References
<!-- > - [http://hahahoho5915.tistory.com/16](http://hahahoho5915.tistory.com/16) -->
