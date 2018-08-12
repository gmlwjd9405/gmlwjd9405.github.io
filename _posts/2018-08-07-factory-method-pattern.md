---
layout: post
title: '[Design Pattern] 팩토리 메서드 패턴이란'
subtitle: '팩토리 메서드 패턴을 이해한다.'
date: 2018-08-07
author: heejeong Kwon
cover: '/images/design-pattern-factory-method/factory-method-main.png'
tags: 디자인패턴 design-pattern factory-method
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 팩토리 메서드 패턴의 개념을 이해한다.
> - 예시를 통해 팩토리 메서드 패턴을 이해한다.

## 팩토리 메서드 패턴이란
* **객체 생성 처리를 서브 클래스로 분리** 해 처리하도록 캡슐화하는 패턴
  * 즉, 객체의 생성 코드를 별도의 클래스/메서드로 분리함으로써 객체 생성의 변화에 대비하는 데 유용하다.
  * 특정 기능의 구현은 개별 클래스를 통해 제공되는 것이 바람직한 설계다.
    * 기능의 변경이나 상황에 따른 기능의 선택은 해당 객체를 생성하는 코드의 변경을 초래한다.
    * 상황에 따라 적절한 객체를 생성하는 코드는 자주 중복될 수 있다.
    * 객체 생성 방식의 변화는 해당되는 모든 코드 부분을 변경해야 하는 문제가 발생한다.
  * [스트래티지 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html), [싱글턴 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html), [템플릿 메서드 패턴](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)을 사용한다.
  * '생성(Creational) 패턴'의 하나 (*아래 참고*)
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
    * 스트래티지 패턴과 싱글턴 패턴을 이용한다.
    * 해당 Post에서는 이 방법을 기준으로 팩토리 메서드 패턴을 적용한다.
  2. **상속 이용**: 하위 클래스에서 적합한 클래스의 객체를 생성
    * 스트래티지 패턴, 싱글턴 패턴과 템플릿 메서드 패턴을 이용한다.
    * 해당 Post의 맨 하단에 '다른 방법으로 팩토리 메서드 패턴 적용하기'를 확인한다.

<mark>참고</mark>
* 생성(Creational) 패턴
  * 객체 생성에 관련된 패턴
  * 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공한다.


## 예시
### 여러 가지 방식의 엘리베이터 스케줄링 방법 지원하기
* ![](/images/design-pattern-factory-method/factory-method-example.png)
* 작업 처리량(Throughput)을 기준으로 한 스케줄링에 따른 엘리베이터 관리
* **스케줄링**: 주어진 요청(목적지 충과 방향)을 받았을 때 여러 대의 엘리베이터 중 하나를 선택하는 것을 말한다.
  * 예를 들어 엘리베이터 내부에서 버튼(ElevatorButton)을 눌렀을 때는 해당 사용자가 탄 엘리베이터를 이동시킨다.
  * 그러나 사용자가 엘리베이터 외부, 즉 건물 내부의 층에서 버튼(FloorButton)을 누른 경우에는 여러 대의 엘리베이터 중 하나를 선택해 이동시켜야 한다.

~~~Java
public class ElevatorManager {
  private List<ElevatorController> controllers;
  private ThroughputScheduler scheduler;

  // 주어진 수만큼의 ElevatorController를 생성함
  public ElevatorManager(int controllerCount) {
    // 엘리베이터의 이동을 책임지는 ElevatorController 객체 생성
    controllers = new ArrayList<ElevatorController>(controllerCount);
    for (int i=0; i<controllerCount; i++) {
      ElevatorController controller = new ElevatorController(1);
      controllers.add(controller);
    }
    // 엘리베이터를 스케줄링(엘리베이터 선택)하기 위한 ThroughputScheduler 객체 생성
    scheduler = new ThroughputScheduler();
  }
  // 요청에 따라 엘리베이터를 선택하고 이동시킴
  void requestElevator(int destination, Direction direction) {
    // ThroughputScheduler를 이용해 엘리베이터를 선택함
    int selectedElevator = scheduler.selectElevator(this, destination, direction);
    // 선택된 엘리베이터를 이동시킴
    controllers.get(selectElevator).gotoFloor(destination);
  }
}
~~~

~~~Java
public class ElevatorController {
  private int id; // 엘리베이터 ID
  private int curFloor; // 현재 층

  public ElevatorController(int id) {
    this.id = id;
    curFloor = 1;
  }
  public void gotoFloor(int destination) {
    System.out.print("Elevator [" + id + "] Floor: " + curFloor);

    // 현재 층 갱신, 즉 주어진 목적지 층(destination)으로 엘리베이터가 이동함
    curFloor = destination;
    System.out.println(" ==> " + curFloor);
  }
}
~~~

~~~Java
/* 엘리베이터 작업 처리량을 최대화시키는 전략의 클래스 */
public class ThroughputScheduler {
  public int selectElevator(ElevatorManager manager, int destination, Direction direction) {
    return 0; // 임의로 선택함
  }
}
~~~
* ElevatorManager 클래스
  * 이동 요청을 처리하는 클래스
  * 엘리베이터를 스케줄링(엘리베이터 선택)하기 위한 ThroughputScheduler 객체를 갖는다.
  * 각 엘리베이터의 이동을 책임지는 ElevatorController 객체를 복수 개 갖는다.
* requestElevator() 메서드
  1. 요청(목적지 층, 방향)을 받았을 때 우선 ThroughputScheduler 클래스의 selectElevator() 메서드를 호출해 적정한 엘리베이터를 선택한다.
  2. 선택된 엘리베이터에 해당하는 ElevatorController 객체의 gotoFloor() 메서드를 호출해 엘리베이터를 이동시킨다.


## 문제점 1
1. **다른 스케줄링 전략** 을 사용하는 경우
  * 엘리베이터 작업 처리량을 최대화(ThroughputScheduler 클래스)시키는 전략이 아닌 사용자의 대기 시간을 최소화하는 엘리베이터 선택 전략을 사용해야 한다면?
2. 프로그램 실행 중에 스케줄링 전략을 변경, 즉 **동적 스케줄링을 지원** 해야하는 경우
  * 오전에는 대기 시간 최소화 전략을 사용하고, 오후에는 처리량 최대화 전략을 사용해야 한다면?

* 문제점 1 해결 방법
~~~Java
public class ElevatorManager {
  private List<ElevatorController> controllers;

  // 주어진 수만큼의 ElevatorController를 생성함
  public ElevatorManager(int controllerCount) {
    // 엘리베이터의 이동을 책임지는 ElevatorController 객체 생성
    controllers = new ArrayList<ElevatorController>(controllerCount);
    for (int i=0; i<controllerCount; i++) {
      ElevatorController controller = new ElevatorController(i + 1); // 변경
      controllers.add(controller);
    }
  }
  // 요청에 따라 엘리베이터를 선택하고 이동시킴
  void requestElevator(int destination, Direction direction) {
    ElevatorScheduler scheduler; // 인터페이스

    // 0..23
    int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);

    // 오전에는 ResponseTimeScheduler, 오후에는 ThroughputScheduler
    if (hour < 12)
      scheduler = new ResponseTimeScheduler();
    else
      scheduler = new ThroughputScheduler();

    // ElevatorScheduler 인터페이스를 이용해 엘리베이터를 선택함
    int selectedElevator = scheduler.selectElevator(this, destination, direction);
    // 선택된 엘리베이터를 이동시킴
    controllers.get(selectElevator).gotoFloor(destination);
  }
}
~~~
~~~Java
/* 사용자의 대기 시간을 최소화시키는 전략의 클래스 */
public class ResponseTimeScheduler {
  public int selectElevator(ElevatorManager manager, int destination, Direction direction) {
    return 1; // 임의로 선택함
  }
}
~~~
* **<span style="background-color: #e1e1e1">[스트래티지 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)</span>** 을 활용한 엘리베이터 스케줄링 전략을 설계
  * ![](/images/design-pattern-factory-method/factory-method-solution1.png)
  * requestElevator() 메서드가 실행될 때마다 현재 시간에 따라 적절한 스케줄링 객체를 생성해야 한다.
  * ElevatorManager 클래스의 입장에서는 여러 스케줄링 전략이 있기 때문에 **ElevatorScheduler 라는 인터페이스** 를 사용하여 여러 전략들을 캡슐화하여 동적으로 선택할 수 있게 한다.


* 그러나, 문제는 여전히 남아 있다.

## 문제점 2
* 엘리베이터 스케줄링 전략이 추가되거나 동적 스케줄링 방식으로 전략을 선택하도록 변경되면
  1. 해당 스케줄링 전략을 지원하는 **구체적인 클래스를 생성** 해야할 뿐만 아니라
  2. ElevatorManager 클래스의 **requestElevator() 메서드도 수정** 할 수밖에 없다.
    * requestElevator() 메서드의 책임: 1. 엘리베이터 선택, 2. 엘리베이터 이동
    * 즉, 엘리베이터를 선택하는 전략의 변경에 따라 requestElevator()가 변경되는 것은 바람직하지 않다.
* 예를 들어
  * 새로운 스케줄링 전략이 추가되는 경우
    * 엘리베이터 노후화 최소화 전략
  * 동적 스케줄링 방식이 변경되는 경우
    * 오전: 대기 시간 최소화 전략, 오후: 처리량 최대화 전략 -> 두 전략의 사용 시간을 서로 바꾸는 경우


## 해결책
### 과정 1
주어진 기능을 실제로 제공하는 적절한 <span style="background-color: #e1e1e1">클래스 생성 작업을 별도의 클래스/메서드로 분리</span>시켜야 한다.
* ![](/images/design-pattern-factory-method/factory-method-solution2.png)
* 엘리베이터 스케줄링 전략에 일치하는 클래스를 생성하는 코드를 requestElevator 메서드에서 분리해 별도의 클래스/메서드를 정의한다.
  * 변경 전: ElevatorManager 클래스가 직접 ThroughputScheduler 객체와 ResponseTimeScheduler 객체를 생성
  * 변경 후: SchedulerFactory 클래스의 getScheduler() 메서드가 스케줄링 전략에 맞는 객체를 생성

~~~java
public enum SchedulingStrategyID { RESPONSE_TIME, THROUGHPUT, DYNAMIC }
~~~
~~~java
public class SchedulerFactory {
  // 스케줄링 전략에 맞는 객체를 생성
  public static ElevatorScheduler getScheduler(SchedulingStrategyID strategyID) {
    switch (strategyID) {
      case RESPONSE_TIME: // 대기 시간 최소화 전략
        scheduler = new ResponseTimeScheduler();
        break;
      case THROUGHPUT: // 처리량 최대화 전략
        scheduler = new ThroughputScheduler();
        break;
      case DYNAMIC: // 동적 스케줄링
        // 0..23
        int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
        // 오전: 대기 시간 최소화, 오후: 처리량 최대화
        if (hour < 12)
          scheduler = new ResponseTimeScheduler();
        else
          scheduler = new ThroughputScheduler();
        break;
    }
    return scheduler;
  }
}
~~~
* 이제 ElevatorManager 클래스의 requestElevator() 메서드에서는 SchedulerFactory 클래스의 getScheduler 메서드를 호출하면 된다.

~~~Java
public class ElevatorManager {
  private List<ElevatorController> controllers;
  private SchedulingStrategyID strategyID;

  // 주어진 수만큼의 ElevatorController를 생성함
  public ElevatorManager(int controllerCount, SchedulingStrategyID strategyID) {
    // 엘리베이터의 이동을 책임지는 ElevatorController 객체 생성
    controllers = new ArrayList<ElevatorController>(controllerCount);
    for (int i=0; i<controllerCount; i++) {
      ElevatorController controller = new ElevatorController(i + 1);
      controllers.add(controller);
    }
  }
  // 실핼 중에 다른 스케줄링 전략으로 지정 가능
  public setStrategyID(SchedulingStrategyID strategyID) {
    this.strategyID = strategyID;
  }
  // 요청에 따라 엘리베이터를 선택하고 이동시킴
  void requestElevator(int destination, Direction direction) {
    // 주어진 전략 ID에 해당되는 ElevatorScheduler를 사용함 (변경)
    ElevatorScheduler scheduler = SchedulerFactory.getScheduler(strategyID);
    System.out.println(scheduler);

    // 주어진 전략에 따라 엘리베이터를 선택함
    int selectedElevator = scheduler.selectElevator(this, destination, direction);
    // 선택된 엘리베이터를 이동시킴
    controllers.get(selectElevator).gotoFloor(destination);
  }
}
~~~
~~~java
public class Client {
  public static void main(String[] args) {
    ElevatorManager emWithResponseTimeScheduler = new ElevatorManager(2, SchedulingStrategyID.RESPONSE_TIME);
    emWithResponseTimeScheduler.requestElevator(10, Direction.UP);

    ElevatorManager emWithThroughputScheduler = new ElevatorManager(2, SchedulingStrategyID.THROUGHPUT);
    emWithThroughputScheduler.requestElevator(10, Direction.UP);

    ElevatorManager emWithDynamicScheduler = new ElevatorManager(2, SchedulingStrategyID.DYNAMIC);
    emWithDynamicScheduler.requestElevator(10, Direction.UP);
  }
}
~~~
~~~java
출력 결과
ResponseTimeScheduler@2d74e4b3
Elevator [2] Floor: 1 ==> 10
ThroughputScheduler@500c05c2
Elevator [1] Floor: 1 ==> 10
ThroughputScheduler@5e6a1140
Elevator [1] Floor: 1 ==> 10
~~~
* Client 클래스에서는 총 3개의 ElevatorManager 객체를 사용하는데, 세 객체 모두 10층으로 이동 요청을 하지만 서로 다른 엘리베이터가 선택될 수 있다.

### 과정 2
동적 스케줄링 방식(DynamicScheduler)이라고 하면 여러 번 스케줄링 객체를 생성하지 않고 <span style="background-color: #e1e1e1">한 번 생성한 것을 계속해서 사용하는 것이 바람직</span>할 수 있다.
* **<span style="background-color: #e1e1e1">[싱글턴 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)</span>** 을 활용한 엘리베이터 스케줄링 전략을 설계
  * ![](/images/design-pattern-factory-method/factory-method-solution3.png)
  * 스케줄링 기능을 제공하는 ResponseTimeScheduler 클래스와 ThroughputScheduler 클래스는 오직 하나의 객체만 생성해서 사용하도록 한다.
  * 즉, 생성자를 통해 직접 객체를 생성하는 것이 허용되지 않아야 한다.
    * 이를 위해 각 생성자를 private으로 정의한다.
    * 대신 getInstance() 라는 정적 메서드로 객체 생성을 구현한다.

~~~java
public class SchedulerFactory {
  // 스케줄링 전략에 맞는 객체를 생성
  public static ElevatorScheduler getScheduler(SchedulingStrategyID strategyID) {
    ElevatorScheduler scheduler = null; // 각 전략에 의해 할당됨

    switch (strategyID) {
      case RESPONSE_TIME: // 대기 시간 최소화 전략
        scheduler = ResponseTimeScheduler.getInstance();
        break;
      case THROUGHPUT: // 처리량 최대화 전략
        scheduler = ThroughputScheduler.getInstance();
        break;
      case DYNAMIC: // 동적 스케줄링
        // 0..23
        int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
        // 오전: 대기 시간 최소화, 오후: 처리량 최대화
        if (hour < 12)
          scheduler = ResponseTimeScheduler.getInstance();
        else
          scheduler = ThroughputScheduler.getInstance();
        break;
    }
    return scheduler;
  }
}
~~~
~~~java
/* 싱글턴 패턴으로 구현한 ThroughputScheduler 클래스 */
public class ThroughputScheduler {
  private static ElevatorScheduler scheduler;
  // 생성자를 private으로 정의
  private ThroughputScheduler() {}
  // 정적 메서드로 객체 생성을 구현 (싱글턴 패턴)
  public static ElevatorScheduler getInstance() {
    if(scheduler == null)
      scheduler = new ThroughputScheduler();
    return scheduler;
  }
  public int selectElevator(ElevatorManager manager, int destination, Direction direction) {
    return 0; // 임의로 선택함
  }
}
~~~
~~~java
/* 싱글턴 패턴으로 구현한 ResponseTimeScheduler 클래스 */
public class ResponseTimeScheduler {
  private static ElevatorScheduler scheduler;
  // 생성자를 private으로 정의
  private ResponseTimeScheduler() {}
  // 정적 메서드로 객체 생성을 구현 (싱글턴 패턴)
  public static ElevatorScheduler getInstance() {
    if(scheduler == null)
      scheduler = new ResponseTimeScheduler();
    return scheduler;
  }
  public int selectElevator(ElevatorManager manager, int destination, Direction direction) {
    return 1; // 임의로 선택함
  }
}
~~~
~~~java
출력 결과
ResponseTimeScheduler@5878ae82
Elevator [2] Floor: 1 ==> 10
ThroughputScheduler@5552bb15 // 동일 객체
Elevator [1] Floor: 1 ==> 10
ThroughputScheduler@5552bb15 // 동일 객체
Elevator [1] Floor: 1 ==> 10
~~~
* 이제 단 1개의 ThroughputScheduler와 ResponseTimeScheduler 객체를 사용할 수 있다.
* ![](/images/design-pattern-factory-method/factory-method-pattern-concepts.png){: width="370" height="250"}
  * 다음과 같이 객체 생성을 전담하는 별도의 **Factory 클래스** 를 분리하여 객체 생성의 변화에 대비할 수 있다.
  * 이 방법은 스트래티지 패턴과 싱글턴 패턴을 이용하여 팩토리 메서드 패턴을 적용한다.


## 다른 방법으로 팩토리 메서드 패턴 적용하기
1. Factory 클래스 이용
  * SchedulerFactory 클래스에서 3가지 방식(최대 처리량, 최소 대기 시간, 동적 선택)에 맞춰 ThroughputScheduler 객체나 ResponseTimeScheduler 객체를 생성
2. 상속 이용
  * 해당 스케줄링 전략에 따라 엘리베이터를 선택하는 클래스를 ElevatorManager 클래스의 하위 클래스로 정의

### 상속을 이용
하위 클래스에서 적합한 클래스의 객체를 생성하여 객체의 생성 코드를 분리한다.
* 이 방법은 스트래티지 패턴, 싱글턴 패턴, 템플릿 메서드 패턴을 이용하여 팩토리 메서드 패턴을 적용한다.
* ![](/images/design-pattern-factory-method/factory-method-extends1.png)

~~~java
/* 템플릿 메서드를 정의하는 클래스: 하위 클래스에서 구현될 기능을 primitive 메서드로 정의 */
public abstract class ElevatorManager {
  private List<ElevatorController> controllers;

  // 주어진 수만큼의 ElevatorController를 생성함
  public ElevatorManager(int controllerCount) {
    // 엘리베이터의 이동을 책임지는 ElevatorController 객체 생성
    controllers = new ArrayList<ElevatorController>(controllerCount);
    for (int i=0; i<controllerCount; i++) {
      ElevatorController controller = new ElevatorController(i + 1);
      controllers.add(controller);
    }
  }
  // 팩토리 메서드: 스케줄링 전략 객체를 생성하는 기능 제공
  protected abstract ElevatorScheduler getScheduler();

  // 템플릿 메서드: 요청에 따라 엘리베이터를 선택하고 이동시킴
  void requestElevator(int destination, Direction direction) {
    // 하위 클래스에서 오버라이드된 getScheduler() 메서드를 호출함 (변경)
    ElevatorScheduler scheduler = getScheduler(); // primitive 또는 hook 메서드
    System.out.println(scheduler);

    // 주어진 전략에 따라 엘리베이터를 선택함
    int selectedElevator = scheduler.selectElevator(this, destination, direction);
    // 선택된 엘리베이터를 이동시킴
    controllers.get(selectElevator).gotoFloor(destination);
  }
}
~~~
~~~java
/* 처리량 최대화 전략 하위 클래스 */
public class ElevatorManagerWithThroughputScheduling extends ElevatorManager {
  public ElevatorManagerWithThroughputScheduling(int controllerCount) {
    super(controllerCount); // 상위 클래스 생성자 호출
  }
  // primitive 또는 hook 메서드
  @Override
  protected ElevatorScheduler getScheduler() {
    ElevatorScheduler scheduler = ThroughputScheduler.getInstance();
    return scheduler;
  }
}

/* 대기 시간 최소화 전략 하위 클래스 */
public class ElevatorManagerWithResponseTimeScheduling extends ElevatorManager {
  public ElevatorManagerWithResponseTimeScheduling(int controllerCount) {
    super(controllerCount); // 상위 클래스 생성자 호출
  }
  // primitive 또는 hook 메서드
  @Override
  protected ElevatorScheduler getScheduler() {
    ElevatorScheduler scheduler = ResponseTimeScheduler.getInstance();
    return scheduler;
  }
}

/* 동적 스케줄링 전략 하위 클래스 */
public class ElevatorManagerWithDynamicScheduling extends ElevatorManager {
  public ElevatorManagerWithDynamicScheduling(int controllerCount) {
    super(controllerCount); // 상위 클래스 생성자 호출
  }
  // primitive 또는 hook 메서드
  @Override
  protected ElevatorScheduler getScheduler() {
    ElevatorScheduler scheduler = null;

    // 0..23
    int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
    // 오전: 대기 시간 최소화, 오후: 처리량 최대화
    if (hour < 12)
      scheduler = ResponseTimeScheduler.getInstance();
    else
      scheduler = ThroughputScheduler.getInstance();

    return scheduler;
  }
}
~~~
* 팩토리 메서드
  * ElevatorManager 클래스의 getScheduler() 메서드
  * 스케줄링 전략 객체를 생성하는 기능 제공 (즉, 객체 생성을 분리)
  * 참고) 템플릿 메서드 패턴의 개념에 따르면, 하위 클래스에서 오버라이드될 필요가 있는 메서드는 primitive 또는 hook 메서드라고도 부른다.
* 템플릿 메서드
  * ElevatorManager 클래스의 requestElevator() 메서드
  * 공통 기능(스케줄링 전략 객체 생성, 엘리베이터 선택, 엘리베이터 이동)의 일반 로직 제공
  * 하위 클래스에서 구체적으로 정의할 필요가 있는 '스케줄링 전략 객체 생성' 부분은 하위 클래스에서 오버라이드
  * 참고) 템플릿 메서드 패턴을 이용하면 전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 코드 중복을 최소화시킬 수 있다.
* 즉, 팩토리 메서드를 호출하는 상위 클래스의 메서드는 템플릿 메서드가 된다.


### 상속을 이용한 팩토리 메서드 패턴 적용
* ![](/images/design-pattern-factory-method/factory-method-extends2.png)
  * **"Product"**: ElevatorScheduler 인터페이스
  * **"ConcreteProduct"**:  ThroughputScheduler 클래스와 ResponseTimeScheduler 클래스
  * **"Creator"**: ElevatorManager 클래스
  * **"ConcreteCreator"**: ElevatorManagerWithThroughputScheduling 클래스, ElevatorManagerWithResponseTimeScheduling 클래스, ElevatorManagerWithDynamicScheduling 클래스


<!-- ## 추가 예시 -->


# 관련된 Post
* 스트래티지(Strategy) 패턴에 대해 알고 싶으시면 [스트래티지(Strategy) 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)을 참고하시기 바랍니다.
* 싱글턴(Singleton) 패턴에 대해 알고 싶으시면 [싱글턴(Singleton) 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)을 참고하시기 바랍니다.
* 커맨드(Command) 패턴에 대해 알고 싶으시면 [커맨드(Command) 패턴](https://gmlwjd9405.github.io/2018/07/07/command-pattern.html)을 참고하시기 바랍니다.
* 옵저버(Observer) 패턴에 대해 알고 싶으시면 [옵저버(Observer) 패턴](https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html)을 참고하시기 바랍니다.
* 데코레이터(Decorator) 패턴에 대해 알고 싶으시면 [데코레이터(Decorator) 패턴](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)을 참고하시기 바랍니다.
* 템플릿 메서드(Template Method) 패턴에 대해 알고 싶으시면 [템플릿 메서드(Template Method) 패턴](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)을 참고하시기 바랍니다.
* 추상 팩토리(Abstract Factory) 패턴에 대해 알고 싶으시면 [추상 팩토리(Abstract Factory) 패턴](https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html)을 참고하시기 바랍니다.
* 컴퍼지트(Composite) 패턴에 대해 알고 싶으시면 [컴퍼지트(Composite) 패턴에](https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html)을 참고하시기 바랍니다.


# References
> - [JAVA 객체지향 디자인 패턴, 한빛미디어](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968480911&orderClick=JAj)
