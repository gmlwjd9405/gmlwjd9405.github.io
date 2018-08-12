---
layout: post
title: '[Design Pattern] 템플릿 메서드 패턴이란'
subtitle: '템플릿 메서드 패턴을 이해한다.'
date: 2018-07-13
author: heejeong Kwon
cover: '/images/design-pattern-template-method/template-method-main.png'
tags: 디자인패턴 design-pattern template-method
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 템플릿 메서드 패턴의 개념을 이해한다.
> - 예시를 통해 템플릿 메서드 패턴을 이해한다.

## 템플릿 메서드 패턴이란
* 어떤 작업을 처리하는 일부분을 **서브 클래스로 캡슐화해** 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴
  * 즉, **전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 코드 중복을 최소화** 할 때 유용하다.
  * 다른 관점에서 보면 동일한 기능을 상위 클래스에서 정의하면서 확장/변화가 필요한 부분만 서브 클래스에서 구현할 수 있도록 한다.
  * 예를 들어, 전체적인 알고리즘은 상위 클래스에서 구현하면서 다른 부분은 하위 클래스에서 구현할 수 있도록 함으로써 전체적인 알고리즘 코드를 재사용하는 데 유용하도록 한다.
  * '행위(Behavioral) 패턴'의 하나 (*아래 참고*)
* ![](/images/design-pattern-template-method/template-method-pattern.png){: width="370" height="250"}
* 역할이 수행하는 작업
  * AbstractClass
    * 템플릿 메서드를 정의하는 클래스
    * 하위 클래스에 공통 알고리즘을 정의하고 하위 클래스에서 구현될 기능을 primitive 메서드 또는 hook 메서드로 정의하는 클래스
  * ConcreteClass
    * 물려받은 primitive 메서드 또는 hook 메서드를 구현하는 클래스
    * 상위 클래스에 구현된 템플릿 메서드의 일반적인 알고리즘에서 하위 클래스에 적합하게 primitive 메서드나 hook 메서드를 오버라이드하는 클래스

<mark>참고</mark>
* 행위(Behavioral) 패턴
  * 객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴
  * 한 객체가 혼자 수행할 수 없는 작업을 여러 개의 객체로 어떻게 분배하는지, 또 그렇게 하면서도 객체 사이의 결합도를 최소화하는 것에 중점을 둔다.


## 예시
### 여러 회사의 모터 지원하기
* ![](/images/design-pattern-template-method/template-method-example.png)
* 엘리베이터 제어 시스템에서 모터를 구동시키는 기능
  * 예를 들어 현대 모터를 이용하는 제어 시스템이라면 HyundaiMotor 클래스에 move 메서드를 정의할 수 있다.
  * HyundaiMotor Class --연관 관계--> Door 클래스
    * move 메서드를 실행할 때 안전을 위해 Door가 닫혀 있는지 확인하기 위해 연관 관계를 정의한다.
  * Enumeration Interface
    * 모터의 상태 (정지 중, 이동 중)
    * 문의 상태 (닫혀 있는 중, 열려 있는 중)
    * 이동 방향 (위로, 아래로)
    * ![](/images/design-pattern-template-method/template-method-example2.png)

~~~Java
public enum DoorStatus { CLOSED, OPENED }
public enum MotorStatus { MOVING, STOPPED }
~~~
~~~Java
public class Door {
  private DoorStatus doorStatus;

  public Door() { doorStatus = DoorStatus.CLOSED; }
  public DoorStatus getDoorStatus() { return doorStatus; }
  public void close() { doorStatus = DoorStatus.CLOSED; }
  public void open() { doorStatus = DoorStatus.OPENED; }
}
~~~
~~~Java
public class HyundaiMotor {
  private Door door;
  private MotorStatus motorStatus;

  public HyundaiMotor() {
    this.door = door;
    motorStatus = MotorStatus.STOPPED; // 초기: 멈춘 상태
  }
  private void moveHyundaiMotor(Direction direction) {
    // Hyundai Motor를 구동시킴
  }
  public MotorStatus getMotorStatus() { return motorStatus; }
  private void setMotorStatus() { this.motorStatus = motorStatus; }

  /* 엘리베이터 제어 */
  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    // 이미 이동 중이면 아무 작업을 하지 않음
    if (motorStatus == MotorStatus.MOVING) return;

    DoorStatus doorStatus = door.getDoorStatus();
    // 만약 문이 열려 있으면 우선 문을 닫음
    if (doorStatus == DoorStatus.OPENED) door.close();

    // Hyundai 모터를 주어진 방향으로 이동시킴
    moveHyundaiMotor(direction);
    // 모터 상태를 이동 중으로 변경함
    setMotorStatus(MotorStatus.MOVING);
  }
}
~~~
~~~Java
public class Client {
  public static void main(String[] args) {
    Door door = new Door();
    HyundaiMotor hyundaiMotor = new HyundaiMotor(door);
    hyundaiMotor.move(Direction.UP); // 위로 올라가도록 엘리베이터 제어
  }
}
~~~
* HyundaiMotor 클래스의 move 메서드는 우선 getMotorStatus 메서드를 호출해 모터의 상태를 조회한다.
  * 모터가 이미 동작 중이면 move 메서드의 실행을 종료한다.
* Door 클래스의 getDoorStatus 메서드를 호출해 문의 상태를 조회한다.
  1. 문이 열려 있는 상태면 Door 클래스의 close 메서드를 호출해 문을 닫는다.
  2. 그리고 moveHyundaiMotor 메서드를 호출해 모터를 구동시킨다.
  3. setMotorStatus를 호출해 모터의 상태를 MOVING으로 기록한다.


### 문제점
* 다른 회사의 모터를 제어해야 하는 경우
  * HyundaiMotor 클래스는 현대 모터를 구동시킨다. 만약 LG 모터를 구동시키려면?

~~~Java
public class LGMotor {
  private Door door;
  private MotorStatus motorStatus;

  public LGMotor() {
    this.door = door;
    motorStatus = MotorStatus.STOPPED; // 초기: 멈춘 상태
  }
  private void moveLGMotor(Direction direction) {
    // LG Motor를 구동시킴
  }
  public MotorStatus getMotorStatus() { return motorStatus; }
  private void setMotorStatus() { this.motorStatus = motorStatus; }

  /* 엘리베이터 제어 */
  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    // 이미 이동 중이면 아무 작업을 하지 않음
    if (motorStatus == MotorStatus.MOVING) return;

    DoorStatus doorStatus = door.getDoorStatus();
    // 만약 문이 열려 있으면 우선 문을 닫음
    if (doorStatus == DoorStatus.OPENED) door.close();

    // LG 모터를 주어진 방향으로 이동시킴
    moveLGMotor(direction); // (이 부분을 제외하면 HyundaiMotor의 move 메서드와 동일)
    // 모터 상태를 이동 중으로 변경함
    setMotorStatus(MotorStatus.MOVING);
  }
}
~~~
  * HyundaiMotor와 LGMotor에는 여러 개의 메서드가 동일하게 구현되어 있다.
    * 즉, 2개의 클래스는 많은 **중복 코드** 를 가진다.
      * Door 클래스와의 연관관계
      * motorStatus 필드
      * getMotorStatus, setMotorStatus 메서드
    * 중복 코드는 **유지보수성을 악화** 시키므로 바람직하지 않다.

### 해결책
***방법 1***<br>
2개 이상의 클래스가 유사한 기능을 제공하면서 중복된 코드가 있는 경우에는 <span style="background-color: #e1e1e1">상속을 이용해서</span> 코드 중복 문제를 피할 수 있다.
* ![](/images/design-pattern-template-method/template-method-solution1.png)

~~~java
/* HyundaiMotor와 LGMotor의 공통적인 기능을 구현하는 클래스 */
public abstract class Motor {
  protected Door door;
  private MotorStatus motorStatus; // 공통 2. motorStatus 필드

  public Motor(Door door) { // 공통 1. Door 클래스와의 연관관계
    this.door = door;
    motorStatus = MotorStatus.STOPPED;
  }
  // 공통 3. etMotorStatus, setMotorStatus 메서드
  public MotorStatus getMotorStatus() { return MotorStatus; }
  protected void setMotorStatus(MotorStatus motorStatus) { this.motorStatus = motorStatus; }
}
~~~
~~~java
/* Motor를 상속받아 HyundaiMotor 클래스를 구현 */
public class HyundaiMotor extends Motor{
  public HyundaiMotor(Door door) { super(door); }
  private void moveHyundaiMotor(Direction direction) {
    // Hyundai Motor를 구동시킴
  }
  public void move(Direction direction) {
    위와 동일
  }
}
~~~
~~~java
/* Motor를 상속받아 LGMotor 클래스를 구현 */
public class LGMotor extends Motor{
  public LGMotor(Door door) { super(door); }
  private void moveLGMotor(Direction direction) {
    // LG Motor를 구동시킴
  }
  public void move(Direction direction) {
    위와 동일
  }
}
~~~
  * Motor 클래스를 상위 클래스로 정의함으로써 중복 코드를 제거할 수 있다.
    1. Door 클래스와의 연관관계
    2. motorStatus 필드
    3. getMotorStatus, setMotorStatus 메서드
  * 그러나 HyundaiMotor와 LGMotor의 move 메서드는 대부분이 비슷하다.
    * 즉, move 메서드에는 여전히 **코드 중복 문제가 있다.**

***방법 2***<br>
위의 move 메서드와 같이 부분적으로 중복되는 경우에도 <span style="background-color: #e1e1e1">상속을 활용해</span> 코드 중복을 피할 수 있다.
* move 메서드에서 moveHyundaiMotor 메서드와 moveLGMotor 메서드를 호출하는 부분만 다르다.
* 또한 moveHyundaiMotor 메서드와 moveLGMotor 메서드는 기능(모터 구동을 실제로 구현) 면에서는 동일하다.
* ![](/images/design-pattern-template-method/template-method-solution2.png)
  1. move 메서드를 상위 Motor 클래스로 이동시킨다.
  2. moveHyundaiMotor 메서드와 moveLGMotor 메서드의 호출 부분을 하위 클래스에서 오버라이드한다.

~~~java
/* HyundaiMotor와 LGMotor의 공통적인 기능을 구현하는 클래스 */
public abstract class Motor {
  ...
  위와 동일

  // HyundaiMotor와 LGMotor의 move 메서드에서 공통되는 부분만을 가짐
  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    // 이미 이동 중이면 아무 작업을 하지 않음
    if (motorStatus == MotorStatus.MOVING) return;

    DoorStatus doorStatus = door.getDoorStatus();
    // 만약 문이 열려 있으면 우선 문을 닫음
    if (doorStatus == DoorStatus.OPENED) door.close();

    // 모터를 주어진 방향으로 이동시킴
    moveMotor(direction); // (HyundaiMotor와 LGMotor에서 오버라이드 됨)
    // 모터 상태를 이동 중으로 변경함
    setMotorStatus(MotorStatus.MOVING);
  }
}
~~~
~~~java
/* Motor를 상속받아 HyundaiMotor 클래스를 구현 */
public class HyundaiMotor extends Motor{
  public HyundaiMotor(Door door) { super(door); }
  // @Override
  protected void moveMotor(Direction direction) {
    // Hyundai Motor를 구동시킴
  }
}
~~~
~~~java
/* Motor를 상속받아 LGMotor 클래스를 구현 */
public class LGMotor extends Motor{
  public LGMotor(Door door) { super(door); }
  // @Override
  protected void moveMotor(Direction direction) {
    // LG Motor를 구동시킴
  }
}
~~~
  * Motor 클래스의 move 메서드는 HyundaiMotor와 LGMotor에서 동일한 기능을 구현하면서 각 하위 클래스에서 구체적으로 정의할 필요가 있는 부분, 즉 moveMotor메서드 부분만 각 하위 클래스에서 오버라이드되도록 한다.
  * 이렇게 Template Method 패턴을 이용하면 전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 코드 중복을 최소화할 수 있다.
  * ![](/images/design-pattern-template-method/template-method-solution3.png)
    * **"AbstractClass"**: Motor 클래스
    * **"ConcreteClass"**:  HyundaiMotor 클래스와 LGMotor 클래스
    * **"템플릿 메서드"**: Motor 클래스의 move 메서드
    * **"primitive 또는 hook 메서드"**: move 메서드에서 호출되면서 하위 클래스에서 오버라이드될 필요가 있는 moveMotor 메서드


<!-- ## 추가 예시 -->


# 관련된 Post
* 스트래티지(Strategy) 패턴에 대해 알고 싶으시면 [스트래티지(Strategy) 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)을 참고하시기 바랍니다.
* 싱글턴(Singleton) 패턴에 대해 알고 싶으시면 [싱글턴(Singleton) 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)을 참고하시기 바랍니다.
* 커맨드(Command) 패턴에 대해 알고 싶으시면 [커맨드(Command) 패턴](https://gmlwjd9405.github.io/2018/07/07/command-pattern.html)을 참고하시기 바랍니다.
* 옵저버(Observer) 패턴에 대해 알고 싶으시면 [옵저버(Observer) 패턴](https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html)을 참고하시기 바랍니다.
* 데코레이터(Decorator) 패턴에 대해 알고 싶으시면 [데코레이터(Decorator) 패턴](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)을 참고하시기 바랍니다.
* 팩토리 메서드(Factory Method) 패턴에 대해 알고 싶으시면 [팩토리 메서드(Factory Method) 패턴](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)을 참고하시기 바랍니다.
* 추상 팩토리(Abstract Factory) 패턴에 대해 알고 싶으시면 [추상 팩토리(Abstract Factory) 패턴](https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html)을 참고하시기 바랍니다.
* 컴퍼지트(Composite) 패턴에 대해 알고 싶으시면 [컴퍼지트(Composite) 패턴에](https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html)을 참고하시기 바랍니다.

# References
> - [JAVA 객체지향 디자인 패턴, 한빛미디어](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968480911&orderClick=JAj)
