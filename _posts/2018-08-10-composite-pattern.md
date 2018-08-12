---
layout: post
title: '[Design Pattern] 컴퍼지트 패턴이란'
subtitle: '컴퍼지트 패턴을 이해한다.'
date: 2018-08-10
author: heejeong Kwon
cover: '/images/design-pattern-composite/composite-main.png'
tags: 디자인패턴 design-pattern composite
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 컴퍼지트 패턴의 개념을 이해한다.
> - 예시를 통해 컴퍼지트 패턴을 이해한다.

## 컴퍼지트 패턴이란
* 여러 개의 객체들로 구성된 복합 객체와 단일 객체를 클라이언트에서 구별 없이 다루게 해주는 패턴
  * 즉, **전체-부분의 관계(Ex. Directory-File)를 갖는 객체들** 사이의 관계를 정의할 때 유용하다.
  * 또한 클라이언트는 전체와 부분을 구분하지 않고 **동일한 인터페이스** 를 사용할 수 있다.
  * '구조(Structural)'의 하나 (*아래 참고*)
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

<mark>참고</mark>
* 구조(Structural) 패턴
  * 클래스나 객체를 조합해 더 큰 구조를 만드는 패턴
  * 예를 들어 서로 다른 인터페이스를 지닌 2개의 객체를 묶어 단일 인터페이스를 제공하거나 객체들을 서로 묶어 새로운 기능을 제공하는 패턴이다.


## 예시
### 컴퓨터에 추가 장치 지원하기
* ![](/images/design-pattern-composite/composite-example.png)
* 컴퓨터(Computer 클래스) 모델링
  * 키보드(Keyboard 클래스): 데이터를 입력받는다.
  * 본체(Body 클래스): 데이터를 처리한다.
  * 모니터(Monitor 클래스): 처리 결과를 출력한다.
* Computer 클래스 --'합성 관계'-- 구성 장치

<mark>참고</mark>
* 합성 관계
  * 생성자에서 필드에 대한 객체를 생성하는 경우
  * 전체 객체의 라이프타임과 부분 객체의 라이프 타임은 의존적이다.
  * 즉, 전체 객체(마름모가 표시된 클래스)가 없어지면 부분 객체도 없어진다.

~~~Java
public class Keyboard {
  private int price;
  private int power;
  public Keyboard(int power, int price) {
    this.power = power;
    this.price = price;
  }
  public int getPrice() { return price; }
  public int getPower() { return power; }
}
public class Body { 동일한 구조 }
public class Monitor { 동일한 구조 }
~~~
~~~Java
public class Computer {
  private Keyboard Keyboard;
  private Body body;
  private Monitor monitor;

  public addKeyboard(Keyboard keyboard) { this.keyboard = keyboard; }
  public addBody(Body body) { this.body = body; }
  public addMonitor(Monitor monitor) { this.monitor = monitor; }

  public int getPrice() {
    int keyboardPrice = keyboard.getPrice();
    int bodyPrice = body.getPrice();
    int monitorPrice = monitor.getPrice();
    return keyboardPrice + bodyPrice + monitorPrice;
  }
  public int getPower() {
    int keyboardPower = keyboard.getPower();
    int bodyPower = body.getPower();
    int monitorPower = monitor.getPower();
    return keyboardPower + bodyPower + monitorPower;
  }
}
~~~
~~~Java
public class Client {
  public static void main(String[] args) {
    // 컴퓨터의 부품으로 Keyboard, Body, Monitor 객체를 생성
    Keyboard keyboard = new Keyboard(5, 2);
    Body body = new Body(100, 70);
    Monitor monitor = new Monitor(20, 30);

    // Computer 객체를 생성하고 부품 객체들을 설정
    Computer computer = new Computer();
    computer.addKeyboard(keyboard);
    computer.addBody(body);
    computer.addMonitor(monitor);

    // 컴퓨터의 가격과 전력 소비량을 구함
    int computerPrice = computer.getPrice();
    int computerPower = computer.getPower();
    System.out.println("Computer Price: " + computerPrice + "만원");
    System.out.println("Computer Power: " + computerPower + "W");
  }
}

[출력 결과]
Computer Price: 102만원
Computer Power: 120W
~~~


### 문제점
* 다른 부품이 추가되는 경우
  * Computer 클래스의 부품으로 Speaker 클래스 또는 Mouse 클래스를 추가한다면?
  * ![](/images/design-pattern-composite/composite-problem.png)

~~~Java
public class Speaker {
  private int price;
  private int power;
  public Speaker(int power, int price) {
    this.power = power;
    this.price = price;
  }
  public int getPrice() { return price; }
  public int getPower() { return power; }
}
~~~
~~~Java
public class Computer {
  . . .
  private Speaker speaker; // 추가

  . . .
  public addMonitor(Monitor monitor) { this.monitor = monitor; } // 추가

  public int getPrice() {
    . . .
    int speakerPrice = speaker.getPrice(); // 추가
    return keyboardPrice + bodyPrice + monitorPrice + speakerPrice;
  }
  public int getPower() {
    . . .
    int speakerPower = speaker.getPower(); // 추가
    return keyboardPower + bodyPower + monitorPower + speakerPower;
  }
}
~~~
  * 위와 같은 방식의 설계는 확장성이 좋지 않다. 즉, **OCP를 만족하지 않는다.**
  * 새로운 부품을 추가할 때마다 Computer 클래스를 아래와 같이 수정해야 한다.
    1. 새로운 부품에 대한 참조를 필드로 추가한다.
    2. 새로운 부품 객체를 설정하는 setter 메서드로 addDevice와 같은 메서드를 추가한다.
    3. getPrice, getPower 등과 같이 컴퓨터의 부품을 이용하는 모든 메서드에서는 새롭게 추가된 부품 객체를 이용할 수 있도록 수정한다.
  * 문제점의 핵심은 **Computer 클래스에 속한 부품의 구체적인 객체를 가리키면** OCP를 위반하게 된다는 것이다.


### 해결책
<span style="background-color: #e1e1e1">구체적인 부품들을 일반화한 클래스를 정의</span>하고 이를 Computer 클래스가 가리키도록 설계한다.
![](/images/design-pattern-composite/composite-solution1.png)
1. 구체적인 부품들을 일반화한 ComputerDevice 클래스를 정의
  * ComputerDevice 클래스는 구체적인 부품 클래스의 공통 기능만 가지며 실제로 존재하는 구체적인 부품이 될 수는 없다. (즉, ComputerDevice 객체를 실제로 생성할 수 없다.)
  * 그러므로 ComputerDevice 클래스는 추상 클래스가 된다.
2. 구체적인 부품 클래스들(Keyboard, Body 등)은 ComputerDevice의 하위 클래스로 정의
3. Computer 클래스는 복수 개( 0..* )의 ComputerDevice 객체를 갖음
4. Computer 클래스도 ComputerDevice 클래스의 하위 클래스로 정의
  * 즉, Computer 클래스도 ComputerDevice 클래스의 일종
  * ComputerDevice 클래스를 이용하면 Client 프로그램은 Keyboard, Body 등과 마찬가지로 Computer를 상용할 수 있다.

~~~java
public abstract class ComputerDevice {
  public abstract int getPrice();
  public abstract int getPower();
}
~~~
~~~Java
public class Keyboard extends ComputerDevice {
  private int price;
  private int power;
  public Keyboard(int power, int price) {
    this.power = power;
    this.price = price;
  }
  public int getPrice() { return price; }
  public int getPower() { return power; }
}
public class Body { 동일한 구조 }
public class Monitor { 동일한 구조 }
~~~
~~~java
public class Computer extends ComputerDevice {
  // 복수 개의 ComputerDevice 객체를 가리킴
  private List<ComputerDevice> components = new ArrayList<ComputerDevice>();

  // ComputerDevice 객체를 Computer 클래스에 추가
  public addComponent(ComputerDevice component) { components.add(component); }
  // ComputerDevice 객체를 Computer 클래스에서 제거
  public removeComponent(ComputerDevice component) { components.remove(component); }

  // 전체 가격을 포함하는 각 부품의 가격을 합산
  public int getPrice() {
    int price = 0;
    for(ComputerDevice component : components) {
      price += component.getPrice();
    }
    return price;
  }
  // 전체 소비 전력량을 포함하는 각 부품의 소비 전력량을 합산
  public int getPower() {
    int power = 0;
    for(ComputerDevice component : components) {
      price += component.getPower();
    }
    return power;
  }
}
~~~
~~~Java
public class Client {
  public static void main(String[] args) {
    // 컴퓨터의 부품으로 Keyboard, Body, Monitor 객체를 생성
    Keyboard keyboard = new Keyboard(5, 2);
    Body body = new Body(100, 70);
    Monitor monitor = new Monitor(20, 30);

    // Computer 객체를 생성하고 addComponent()를 통해 부품 객체들을 설정
    Computer computer = new Computer();
    computer.addComponent(keyboard);
    computer.addComponent(body);
    computer.addComponent(monitor);

    // 컴퓨터의 가격과 전력 소비량을 구함
    int computerPrice = computer.getPrice();
    int computerPower = computer.getPower();
    System.out.println("Computer Price: " + computerPrice + "만원");
    System.out.println("Computer Power: " + computerPower + "W");
  }
}
~~~
* Computer 클래스
  * ComputerDevice의 하위 클래스면서 복수 개의 ComputerDevice를 갖도록 설계했다.
  * addComponent() 메서드를 통해 구체적인 부품인 Keyboard, Body 등을 Computer 클래스의 부품으로 설정했다.
* Client  
  * addComponent() 메서드를 통해 부품 종류에 관계 없이 동일한 메서드로 부품을 추가할 수 있다.
* 이제, **Computer 클래스는 OCP를 준수** 한다.
  * 새로운 부품을 추가한다면 ComputerDevice 클래스의 하위 클래스로 구현하면 된다.
* 컴퍼지트 패턴을 이용하면 부분 객체의 추가나 삭제 등이 있어도 전체 객체의 클래스 코드를 변경하지 않아도 된다.
  * 즉, **전체-부분 관계(Ex. Directory-File)를 갖는 객체들 사이의 관계를 정의할 때 유용하다.**
* ![](/images/design-pattern-composite/composite-solution2.png)
  * **"Component"**: ComputerDevice 클래스
  * **"Leaf"**:  Keyboard 클래스, Body 클래스, Monitor 클래스, Speaker 클래스
  * **"Composite"**: Computer 클래스


<!-- ## 추가 예시 -->


# 관련된 Post
* 스트래티지(Strategy) 패턴에 대해 알고 싶으시면 [스트래티지(Strategy) 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)을 참고하시기 바랍니다.
* 싱글턴(Singleton) 패턴에 대해 알고 싶으시면 [싱글턴(Singleton) 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)을 참고하시기 바랍니다.
* 커맨드(Command) 패턴에 대해 알고 싶으시면 [커맨드(Command) 패턴](https://gmlwjd9405.github.io/2018/07/07/command-pattern.html)을 참고하시기 바랍니다.
* 옵저버(Observer) 패턴에 대해 알고 싶으시면 [옵저버(Observer) 패턴](https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html)을 참고하시기 바랍니다.
* 데코레이터(Decorator) 패턴에 대해 알고 싶으시면 [데코레이터(Decorator) 패턴](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)을 참고하시기 바랍니다.
* 템플릿 메서드(Template Method) 패턴에 대해 알고 싶으시면 [템플릿 메서드(Template Method) 패턴](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)을 참고하시기 바랍니다.
* 팩토리 메서드(Factory Method) 패턴에 대해 알고 싶으시면 [팩토리 메서드(Factory Method) 패턴](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)을 참고하시기 바랍니다.
* 추상 팩토리(Abstract Factory) 패턴에 대해 알고 싶으시면 [추상 팩토리(Abstract Factory) 패턴](https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html)을 참고하시기 바랍니다.


# References
> - [JAVA 객체지향 디자인 패턴, 한빛미디어](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968480911&orderClick=JAj)
