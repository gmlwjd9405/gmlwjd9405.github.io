---
layout: post
title: '[Design Pattern] 추상 팩토리 패턴이란'
subtitle: '추상 팩토리 패턴을 이해한다.'
date: 2018-08-08
author: heejeong Kwon
cover: '/images/design-pattern-abstract-factory/abstract-factory-main.png'
tags: 디자인패턴 design-pattern abstract-factory
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 추상 팩토리 패턴의 개념을 이해한다.
> - 예시를 통해 추상 팩토리 패턴을 이해한다.

## 추상 팩토리 패턴이란
* 구체적인 클래스에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 인터페이스를 제공하는 패턴
  * 즉, 관련성 있는 여러 종류의 객체를 일관된 방식으로 생성하는 경우에 유용하다.
  * [싱글턴 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html), [팩토리 메서드 패턴](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)을 사용한다.
  * '생성(Creational) 패턴'의 하나 (*아래 참고*)
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

<mark>참고</mark>
* 생성(Creational) 패턴
  * 객체 생성에 관련된 패턴
  * 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공한다.


## 예시
### 엘리베이터 부품 업체 변경하기
* ![](/images/design-pattern-abstract-factory/abstract-factory-example.png)
* 여러 제조 업체의 부품을 사용하더라도 같은 동작을 지원하게 하는 것이 바람직하다. 즉, 엘리베이터 프로그램의 변경을 최소화해야 한다.
  * 추상 클래스로 Motor, Door를 정의
    * 제조 업체가 다른 경우 구체적인 제어 방식은 다르지만 엘리베이터 입장에서는 모터를 구동해 엘리베이터를 이동시킨다는 면에서는 동일하다.
    * 그러므로 추상 클래스로 Motor를 정의하고 여러 제조 업체의 모터를 하위 클래스로 정의할 수 있다.
  * Motor 클래스 --연관 관계--> Door
    * Motor 클래스는 이동하기 전에 문을 닫아야 한다.
    * Door 객체의 getDoorStatus() 메서드를 호출하기 위해서 연관 관계가 필요하다.
  * Motor 클래스의 핵심 기능인 이동은 move() 메서드로 정의
~~~java
public void move(Direction direction) {
  // 1) 이미 이동 중이면 무시한다.
  // 2) 만약 문이 열려 있으면 문을 닫는다.
  // 3) 모터를 구동해서 이동시킨다. -> 제조 업체에 따라 다르다.
  // 4) 모터의 상태를 이동 중으로 설정한다.
}
~~~
    * 4단계의 동작 모두 LGMotor, HyundaiMotor 클래스의 공통 기능이고, 3)단계 부분만 달라진다.
    * **템플릿 메서드 패턴 이용**
      * 즉, 일반적인 흐름에서는 동일하지만 특정 부분만 다른 동작을 하는 경우에는 *일반적인 기능을 상위클래스에 템플릿 메서드* 로서 설계할 수 있다.
      * *특정 부분에 대해서는 하위 클래스에서 오버라이드* 할 수 있도록 한다.
  * Door 클래스의 open(), close() 메서드 정의
~~~java
public void open() {
  // 1) 이미 문이 열려 있으면 무시한다.
  // 2) 문을 닫는다. -> 제조 업체에 따라 다르다.
  // 3) 문의 상태를 '닫힘'으로 설정한다.
}
~~~
      * 나머지 동작은 공통으로 필요한 기능이고, 2)단계 부분만 달라진다.
      * 즉, move() 메서드와 같이 **템플릿 메서드 패턴** 을 적용할 수 있다.

**[템플릿 메서드 패턴](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)** 을 적용한 예제 코드
* 전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 코드 중복을 최소화할 수 있다.

~~~Java
public abstract class Door {
  private DoorStatus doorStatus;

  public Door() { doorStatus = DoorStatus.CLOSED; }
  public DoorStatus getDoorStatus() { return doorStatus; }

  // primitive 또는 hook 메서드
  protected abstract void doClose();
  // 템플릿 메서드: 문을 닫는 기능 수행
  public void close() {
    // 이미 문이 닫혀 있으면 아무 동작을 하지 않음
    if(doorStatus == DoorStatus.CLOSED)
      return;
    // primitive 또는 hook 메서드. 하위 클래스에서 오버라이드
    doClose(); // 실제로 문을 닫는 동작을 수행
    doorStatus = DoorStatus.CLOSED; // 문의 상태를 닫힘으로 기록
  }

  // primitive 또는 hook 메서드
  protected abstract void doOpen();
  // 템플릿 메서드: 문을 여는 기능 수행
  public void open() {
    // 이미 문이 열려 있으면 아무 동작을 하지 않음
    if(doorStatus == DoorStatus.OPENED)
      return;
    // primitive 또는 hook 메서드. 하위 클래스에서 오버라이드
    doOpen(); // 실제로 문을 여는 동작을 수행
    doorStatus = DoorStatus.OPENED; // 문의 상태를 열림으로 기록
  }
}
~~~
~~~java
public class LGDoor extends Door {
  protected void doClose() { System.out.println("close LG Door"); }
  protected void doOpen() { System.out.println("open LG Door"); }
}
~~~
~~~java
public class HyundaiDoor extends Door {
  protected void doClose() { System.out.println("close Hyundai Door"); }
  protected void doOpen() { System.out.println("open Hyundai Door"); }
}
~~~
* 엘리베이터 입장에서는 특정 제조 업체의 모터와 문을 제어하는 클래스가 필요하다.
  * 예를 들어 LGMotor, LGDoor 객체가 필요하다.
  * **팩토리 메서드 패턴 이용**


**[팩토리 메서드 패턴](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)** 을 적용한 예제 코드
* 객체 생성 처리를 서브 클래스로 분리하여 캡슐화함으로써 객체 생성의 변화에 대비할 수 있다.
* ![](/images/design-pattern-abstract-factory/abstract-factory-example1.png)

~~~Java
public enm VendorID { LG, HYUNDAI }
~~~
~~~java
public class MotorFactory {
  // vendorID에 따라 LGMotor 또는 HyundaiMotor 객체를 생성함
  public static Motor createMotor(VendorID vendorID) {
    Motor motor = null;

    switch (vendorID) {
      case LG:
        motor = new LGMotor();
        break;
      case HYUNDAI:
        motor = new HyundaiMotor();
        break;
    }
    return motor;
  }
}
~~~
  * MotorFactory 클래스의 createMotor() 메서드는 인자로 주어진 VendorID에 따라 LGMotor 객체 또는 HyundaiMotor 객체를 생성한다.

~~~java
public class DoorFactory {
  // vendorID에 따라 LGDoor 또는 HyundaiDoor 객체를 생성함
  public static Door createDoor(VendorID vendorID) {
    Door door = null;

    switch (vendorID) {
      case LG:
        door = new LGDoor();
        break;
      case HYUNDAI:
        door = new HyundaiDoor();
        break;
    }
    return door;
  }
}
~~~
  * DoorFactory 클래스의 createDoor() 메서드는 인자로 주어진 VendorID에 따라 LGDoor 객체 또는 HyundaiDoor 객체를 생성한다.

~~~java
public class Client {
  public static void main(String[] args) {
    Door lgDoor = DoorFactory.createDoor(VendorID.LG); // 팩토리 메서드 호출
    Motor lgMotor = MotorFactory.createMotor(VendorID.LG); // 팩토리 메서드 호출
    lgMotor.setDoor(lgDoor);

    lgDoor.open();
    lgMotor.move(Direction.UP);
  }
}
~~~
  * 제조 업체에 따라 모터와 문에 해당하는 구체적인 클래스를 생성해야 한다.
  * LGDoor와 LGMotor 객체를 활용하므로 move() 메서드를 호출하기 전 open() 메서드에 의해 문이 열려있는 상태다.
  * 그러므로 move() 메서드는 문을 먼저 닫고 이동을 시작한다.
  * 이때 LGDoor와 LGMotor 객체가 모두 사용된다.
~~~Java
// 실행 결과
open LG Door
close LG Door
move LG Motor
~~~


### 문제점
1. 다른 제조 업체의 부품을 사용해야 하는 경우
  * LG의 부품 대신 현대의 부품(HyundaiMotor, HyundaiDoor 클래스)을 사용해야 한다면?
~~~Java
public class Client {
  public static void main(String[] args) {
    Door hyundaiDoor = DoorFactory.createDoor(VendorID.HYUNDAI); // 팩토리 메서드 호출
    Motor hyundaiMotor = MotorFactory.createMotor(VendorID.HYUNDAI); // 팩토리 메서드 호출
    hyundaiMotor.setDoor(lgDoor);
    // 문을 먼저 연다.
    hyundaiDoor.open();
    hyundaiMotor.move(Direction.UP);
  }
}
~~~
* 총 10개의 부품을 사용해야 한다면 각 Factory 클래스를 구현하고 이들의 Factory 객체를 각각 생성해야 한다.
* 즉, 부품의 수가 많아지면 특정 업체별 부품을 생성하는 코드의 길이가 길어지고 복잡해진다.

2. 새로운 제조 업체의 부품을 지원해야 하는 경우
  * 삼성의 부품(SamsungMotor, SamsungDoor 클래스)을 지원해야 한다면?
~~~java
public class DoorFactory {
  // vendorID에 따라 LGDoor 또는 HyundaiDoor 객체를 생성함
  public static Door createDoor(VendorID vendorID) {
    Door door = null;
    // 삼성의 부품을 추가
    switch (vendorID) {
      case LG:
        door = new LGDoor();
        break;
      case HYUNDAI:
        door = new HyundaiDoor();
        break;
      case SAMSUNG:
        door = new SamsungDoor();
        break;
    }
    return door;
  }
}
~~~
* DoorFactory 클래스뿐만 아니라 나머지 9개의 부품과 연관된 Factory 클래스에서도 마찬가지로 삼성의 부품을 생성하도록 변경해야 한다.
* 또한, 위와 마찬가지로 특정 업체별 부품을 생성하는 코드에서 삼성의 부품을 생성하도록 모두 변경해야 한다.

* 즉, 결과적으로 기존의 **팩토리 메서드 패턴** 을 이용한 객체 생성은 관련 있는 여러 개의 객체를 일관성 있는 방식으로 생성하는 경우에 **많은 코드 변경이 발생** 하게 된다는 것이다.



### 해결책
**여러 종류의 객체를 생성할 때 객체들 사이의 관련성이 있는 경우** 라면 각 종류별로 별도의 Factory 클래스를 사용하는 대신 <span style="background-color: #e1e1e1">관련 객체들을 일관성 있게 생성하는 Factory 클래스를 사용</span> 하는 것이 편리할 수 있다.
* ![](/images/design-pattern-abstract-factory/abstract-factory-solution1.png)
  * 예를 들어 MotorFactory, DoorFactory 클래스와 같이 ***부품별로*** Factory 클래스를 만드는 대신 LGElevatorFactory나 HyundaiElevatorFactory 클래스와 같이 ***제조 업체별로*** Factory 클래스를 만들 수도 있다.
  * LGElevatorFactory: LGMotor, LGDoor 객체를 생성하는 팩토리 클래스
  * HyundaiElevatorFactory: HyundaiMotor, HyundaiDoor 객체를 생성하는 팩토리 클래스

~~~java
/* 추상 부품을 생성하는 추상 팩토리 클래스 */
public abstract class ElevatorFactory {
  public abstract Motor createMotor();
  public abstract Door createDoor();
}
~~~
~~~java
/* LG 부품을 생성하는 팩토리 클래스 */
public class LGElevatorFactory extends ElevatorFactory {
  public Motor createMotor() { return new LGMotor(); }
  public Door createDoor() { return new LGDoor(); }
}
/* Hyundai 부품을 생성하는 팩토리 클래스 */
public class HyundaiElevatorFactory extends ElevatorFactory {
  public Motor createMotor() { return new HyundaiMotor(); }
  public Door createDoor() { return new HyundaiDoor(); }
}
~~~
~~~java
/* 주어진 업체의 이름에 따라 부품을 생성하는 Client 클래스 */
public class Client {
  public static void main(String[] args) {
    ElevatorFactory factory = null;
    String vendorName = args[0];

    // 인자에 따라 LG 또는 Hyundai 팩토리를 생성
    if(vendorName.equalsIgnoreCase("LG"))
      factory = new LGElevatorFactory();
    else
      factory = new HyundaiElevatorFactory();

    Door door = factory.createDoor(); // 해당 업체의 Door 생성
    Motor motor = factory.createMotor(); // 해당 업체의 Motor 생성
    motor.setDoor(door);

    door.open();
    motor.move(Direction.UP);
  }
}
~~~
* 인자로 주어진 업체 이름에 따라 적절한 부품 객체를 생성한다.
  * 즉, 문제점 1과 같이 다른 제조 업체의 부품으로 변경하는 경우에도 Client 코드를 변경할 필요가 없다.
* 제조 업체별로 Factory 클래스를 정의했으므로 제조 업체별 부품 객체를 간단히 생성할 수 있다.
  * 즉, 문제점 2와 같이 새로운 제조 업체의 부품을 지원하는 경우에도 해당 제조 업체의 부품을 생성하는 Factory 클래스만 새롭게 만들면 된다.
* ![](/images/design-pattern-abstract-factory/abstract-factory-solution2.png)

~~~java
/* Samsung 부품을 생성하는 팩토리 클래스 */
public class SamsungElevatorFactory extends ElevatorFactory {
  public Motor createMotor() { return new SamsungMotor(); }
  public Door createDoor() { return new SamsungDoor(); }
}
/* Samsung Door 클래스 */
public class SamsungDoor extends Door {
  protected void doClose() { System.out.println("close Samsung Door"); }
  protected void doOpen() { System.out.println("open Samsung Door"); }
}
/* Samsung Motor 클래스 */
public class SamsungDoor extends Door {
  protected void doClose() { System.out.println("close Samsung Motor"); }
  protected void doOpen() { System.out.println("open Samsung Motor"); }
}
~~~
~~~java
/* 주어진 업체의 이름에 따라 부품을 생성하는 Client 클래스 */
public class Client {
  public static void main(String[] args) {
    ElevatorFactory factory = null;
    String vendorName = args[0];

    // 인자에 따라 LG 또는 Samsung 또는 Hyundai 팩토리를 생성
    if(vendorName.equalsIgnoreCase("LG"))
      factory = new LGElevatorFactory();
    else if(vendorName.equalsIgnoreCase("Samsung")) // 추가
      factory = new SamsungElevatorFactory();
    else
      factory = new HyundaiElevatorFactory();

    동일
  }
}
~~~

### 추가 보완 해결책(정확한 추상 팩토리 패턴 적용)
![](/images/design-pattern-abstract-factory/abstract-factory-solution3.png)

***과정 1***<br>
**"팩토리 메서드 패턴":** 제조 업체별 Factory 객체를 생성하는 방식을 캡슐화한다.<br>
* ElevatorFactoryFactory 클래스: vendorID에 따라 해당 제조 업체의 Factory 객체를 생성
* ElevatorFactoryFactory 클래스의 getFactory() 메서드: 팩토리 메서드

~~~java
/* 팩토리 클래스에 팩토리 메서드 패턴을 적용 */
public class ElevatorFactoryFactory {
  public static ElevatorFactory getFectory(VendorID vendorID) {
    ElevatorFactory factory = null;

    switch (vendorID) {
      case LG: // LG 팩토리 생성
        factory = new LGElevatorFactory.getInstance();
        break;
      case HYUNDAI: // Hyundai 팩토리 생성
        factory = new HyundaiElevatorFactory.getInstance();
        break;
      case SAMSUNG: // Samsung 팩토리 생성
        factory = new SamsungElevatorFactory.getInstance();
        break;
    }
    return factory;
  }
}
~~~

***과정 2***<br>
**"싱글턴 패턴":** 제조 업체별 Factory 객체는 각각 1개만 있으면 된다.<br>
* 3개의 제조 업체별 Factory 클래스를 싱글턴으로 설계

~~~java
/* 싱글턴 패턴을 적용한 LG 팩토리 */
public class LGElevatorFactory extends ElevatorFactory {
  public static ElevatorFactory getFectory(VendorID vendorID) {
    private static ElevatorFactory factory;
    private LGElevatorFactory() { } // 생성자를 private로

    public static ElevatorFactory getInstance() {
      if(factory == null)
        factory = new LGElevatorFactory();

      return factory;
    }

    public Motor createMotor() { return new SamsungMotor(); }
    public Door createDoor() { return new SamsungDoor(); }
  }
}
/* 싱글턴 패턴을 적용한 Hyundai 팩토리 */
동일한 방식
/* 싱글턴 패턴을 적용한 Samsung 팩토리 */
동일한 방식
~~~

***추상 팩토리 패턴을 이용한 방법을 사용하는 Client***<br>
~~~java
/* 주어진 업체의 이름에 따라 부품을 생성하는 Client 클래스 */
public class Client {
  public static void main(String[] args) {
    String vendorName = args[0];
    VendorID vendorID;

    // 인자에 따라 LG 또는 Samsung 또는 Hyundai 팩토리를 생성
    if(vendorName.equalsIgnoreCase("LG"))
      vendorID = VendorID.LG;
    else if(vendorName.equalsIgnoreCase("Samsung"))
      vendorID = VendorID.SAMSUNG;
    else
      vendorID = VendorID.HYUNDAI;

    ElevatorFactory factory = ElevatorFactoryFactory.getFactory(vendorID);

    동일
  }
}
~~~

***추상 팩토리 패턴 최종 클래스 다이어그램***<br>
* ![](/images/design-pattern-abstract-factory/abstract-factory-solution4.png)
  * **"AbstractFactory"**: ElevatorFactory 클래스
  * **"ConcreteFactory"**:  LGElevatorFactory 클래스와 HyundaiElevatorFactory 클래스
  * **"AbstractProductA"**: Motor 클래스
  * **"ConcreteProductA"**: LGMotor 클래스와 HyundaiMotor 클래스
  * **"AbstractProductB"**: Door 클래스
  * **"ConcreteProductB"**: LGDoor 클래스와 HyundaiDoor 클래스


<!-- ## 추가 예시 -->


# 관련된 Post
* 스트래티지(Strategy) 패턴에 대해 알고 싶으시면 [스트래티지(Strategy) 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)을 참고하시기 바랍니다.
* 싱글턴(Singleton) 패턴에 대해 알고 싶으시면 [싱글턴(Singleton) 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)을 참고하시기 바랍니다.
* 커맨드(Command) 패턴에 대해 알고 싶으시면 [커맨드(Command) 패턴](https://gmlwjd9405.github.io/2018/07/07/command-pattern.html)을 참고하시기 바랍니다.
* 옵저버(Observer) 패턴에 대해 알고 싶으시면 [옵저버(Observer) 패턴](https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html)을 참고하시기 바랍니다.
* 데코레이터(Decorator) 패턴에 대해 알고 싶으시면 [데코레이터(Decorator) 패턴](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)을 참고하시기 바랍니다.
* 템플릿 메서드(Template Method) 패턴에 대해 알고 싶으시면 [템플릿 메서드(Template Method) 패턴](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)을 참고하시기 바랍니다.
* 팩토리 메서드(Factory Method) 패턴에 대해 알고 싶으시면 [팩토리 메서드(Factory Method) 패턴](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)을 참고하시기 바랍니다.
* 컴퍼지트(Composite) 패턴에 대해 알고 싶으시면 [컴퍼지트(Composite) 패턴에](https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html)을 참고하시기 바랍니다.


# References
> - [JAVA 객체지향 디자인 패턴, 한빛미디어](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968480911&orderClick=JAj)
