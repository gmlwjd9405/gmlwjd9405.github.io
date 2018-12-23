---
layout: post
title: '[Design Pattern] 데코레이터 패턴이란'
subtitle: '데코레이터 패턴을 이해한다.'
date: 2018-07-09
author: heejeong Kwon
cover: '/images/design-pattern-decorator/decorator-main.png'
tags: 디자인패턴 design-pattern decorator
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 데코레이터 패턴의 개념을 이해한다.
> - 예시를 통해 데코레이터 패턴을 이해한다.

## 데코레이터 패턴이란
* **객체의 결합** 을 통해 **기능을 동적으로 유연하게 확장** 할 수 있게 해주는 패턴
  * 즉, 기본 기능에 추가할 수 있는 기능의 종류가 많은 경우에 **각 추가 기능을 Decorator 클래스로 정의** 한 후 필요한 Decorator 객체를 조합함으로써 **추가 기능의 조합을 설계** 하는 방식이다.
    * Ex) 기본 도로 표시 기능에 차선 표시, 교통량 표시, 교차로 표시, 단속 카메라 표시의 4가지 추가 기능이 있을 때 추가 기능의 모든 조합은 15가지가 된다.
    * -> 데코레이터 패턴을 이용하여 필요 추가 기능의 조합을 동적으로 생성할 수 있다.
  * '구조(Structural) 패턴'의 하나 (*아래 참고*)
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

<mark>참고</mark>
* 구조(Structural) 패턴
  * 클래스나 객체를 조합해 더 큰 구조를 만드는 패턴
  * 예를 들어 서로 다른 인터페이스를 지닌 2개의 객체를 묶어 단일 인터페이스를 제공하거나 객체들을 서로 묶어 새로운 기능을 제공하는 패턴이다.
* 합성 관계
  * 생성자에서 필드에 대한 객체를 생성하는 경우
  * 전체 객체의 라이프타임과 부분 객체의 라이프 타임은 의존적이다.
  * 즉, 전체 객체가 없어지면 부분 객체도 없어진다.


## 예시
### 도로 표시 방법 조합하기
* ![](/images/design-pattern-decorator/decorator-example.png){: width="280" height="200"}
* 내비게이션 SW에서 도로를 표시하는 기능
  * 도로를 간단한 선으로 표시하는 기능(기본 기능)
  * 내비게이션 SW에 따라 도로의 차선을 표시하는 기능(추가 기능)
~~~Java
// 기본 도로 표시 클래스
public class RoadDisplay {
        public void draw() { System.out.println("기본 도로 표시"); }
}
~~~
~~~Java
// 기본 도로 표시 + 차선 표시 클래스
public class RoadDisplayWithLane extends RoadDisplay {
      public void draw() {
          super.draw(); // 상위 클래스, 즉 RoadDisplay 클래스의 draw 메서드를 호출해서 기본 도로 표시
          drawLane(); // 추가적으로 차선 표시
      }
      private void drawLane() { System.out.println("차선 표시"); }
}
~~~
~~~java
public class Client {
      public static void main(String[] args) {
          RoadDisplay road = new RoadDisplay();
          road.draw(); // 기본 도로만 표시

          RoadDisplay roadWithLane = new RoadDisplayWithLane();
          roadWithLane.draw(); // 기본 도로 표시 + 차선 표시
      }
}
~~~
* RoadDisplay 클래스에는 기본 도로 표시 기능을 실행하기 위한 draw 메서드를 구현한다.
* RoadDisplayWithLane 클래스에는 차선 표시 기능을 추가하기 위해 상속받은 draw 메서드를 오버라이드한다.
  * 기본 도로 표시 기능: 상위 클래스(RoadDisplay)의 draw 메서드 호출
  * 차선 표시 기능: 자신의 drawLane 메서드 호출

### 문제점
1. 또 다른 도로 표시 기능을 추가로 구현하는 경우
  * 기본 도로 표시에 교통량을 표시하고 싶다면?
  * ![](/images/design-pattern-decorator/decorator-problem1.png)
~~~java
// 기본 도로 표시 + 교통량 표시 클래스
public class RoadDisplayWithTraffic extends RoadDisplay {
	  public void draw() {
		    super.draw(); // 상위 클래스, 즉 RoadDisplay 클래스의 draw 메서드를 호출해서 기본 도로 표시
		    drawTraffic(); // 추가적으로 교통량 표시
	  }
	  private void drawTraffic() { System.out.println("교통량 표시"); }
}
~~~
  <!-- * 설명 추가 -->
2. 여러 가지 추가 기능을 조합해야 하는 경우
  * 기본 도로 표시에 차선 표시 기능과 교통량 표시 기능을 함께 제공하고 싶다면?
  * ![](/images/design-pattern-decorator/decorator-problem2.png)
  * ![](/images/design-pattern-decorator/decorator-problem3.png)
  * 위와 같이 상속을 통해 조합의 각 경우를 설계한다면 각 조합별로 하위 클래스(7개)를 구현해야 한다.
  * 즉, 다양한 기능의 조합을 고려해야 하는 경우 **상속을 통한 기능의 확장은 각 기능별로 클래스를 추가해야 한다** 는 단점이 있다.


### 해결책
문제를 해결하기 위해서는 <span style="background-color: #e1e1e1">각 추가 기능별로 개별적인 클래스를 설계하고 기능을 조합할 때 각 클래스의 객체 조합을 이용</span>하면 된다.
* ![](/images/design-pattern-decorator/decorator-solution.png)
  * 도로를 표시하는 기본 기능만 필요한 경우 RoadDisplay 객체를 이용한다.
  * 차선을 표시하는 추가 기능도 필요한 경우 RoadDisplay 객체와 LaneDecorator 객체를 이용한다.
    1. LaneDecorator에서는 차선 표시 기능만 직접 제공: drawLane()
    2. 도로 표시 가능은 RoadDisplay 클래스의 draw 메서드를 호출: super.draw()
      * (DisplayDecorator 클래스에서 Display 클래스로의 합성(composition) 관계를 통해 RoadDisplay 객체에 대한 참조)

* Display 클래스
~~~Java
public abstract class Display { public abstract void draw(); }
~~~
* RoadDisplay 클래스
~~~java
/* 기본 도로 표시 클래스 */
public class RoadDisplay extends Display {
    @Override
    public void draw() { System.out.println("기본 도로 표시"); }
}
~~~
* DisplayDecorator 클래스
~~~Java
/* 다양한 추가 기능에 대한 공통 클래스 */
public abstract class DisplayDecorator extends Display {
	private Display decoratedDisplay;
    // '합성(composition) 관계'를 통해 RoadDisplay 객체에 대한 참조
	public DisplayDecorator(Display decoratedDisplay) {
		this.decoratedDisplay = decoratedDisplay;
	}
	@Override
	public void draw() { decoratedDisplay.draw(); }
}
~~~
* LaneDecorator, TrafficDecorator 클래스
~~~Java
/* 차선 표시를 추가하는 클래스 */
public class LaneDecorator extends DisplayDecorator {
	// 기존 표시 클래스의 설정
	public LaneDecorator(Display decoratedDisplay) { super(decoratedDisplay); }
	@Override
	public void draw() {
		super.draw(); // 설정된 기존 표시 기능을 수행
		drawLane(); // 추가적으로 차선을 표시
	}
    // 차선 표시 기능만 직접 제공
	private void drawLane() { System.out.println("\t차선 표시"); }
}
/* 교통량 표시를 추가하는 클래스 */
public class TrafficDecorator extends DisplayDecorator {
	// 기존 표시 클래스의 설정
	public TrafficDecorator(Display decoratedDisplay) { super(decoratedDisplay); }
	@Override
	public void draw() {
		super.draw(); // 설정된 기존 표시 기능을 수행
		drawTraffic(); // 추가적으로 교통량을 표시
	}
    // 교통량 표시 기능만 직접 제공
	private void drawTraffic() { System.out.println("\t교통량 표시"); }
}
~~~
* 클라이언트에서의 사용
~~~Java
public class Client {
	public static void main(String[] args) {
		Display road = new RoadDisplay();
		road.draw(); // 기본 도로 표시
		Display roadWithLane = new LaneDecorator(new RoadDisplay());
		roadWithLane.draw(); // 기본 도로 표시 + 차선 표시
		Display roadWithTraffic = new TrafficDecorator(new RoadDisplay());
		roadWithTraffic.draw(); // 기본 도로 표시 + 교통량 표시
	}
}
~~~
* 각 road, roadWithLane, roadWithTraffic 객체의 접근이 모두 Display 클래스를 통해 이루어 진다.
* 즉, 어떤 기능을 추가하느냐에 관계없이 Client 클래스는 동일한 Display 클래스만을 통해 **일관성 있는 방식으로** 도로 정보를 표시할 수 있다.
* 이렇게 Decorator 패턴을 이용하면 추가 기능 조합별로 별도의 클래스를 구현하는 대신 각 추가 기능에 해당하는 클래스의 객체를 조합해 추가 기능의 조합을 구현할 수 있게 된다.
  * 이 설계는 추가 기능의 수가 많을수록 효과가 크다.
* ![](/images/design-pattern-decorator/decorator-solution2.png)

### 추가 예시
* 기본 도로 표시 + 차선 표시 + 교통량 표시
~~~Java
public class Client {
	public static void main(String[] args) {
    // 기본 도로 표시 + 차선 표시 + 교통량 표시
  	Display roadWithLaneAndTraffic =
        new TrafficDecorator(
        new LaneDecorator(
        new RoadDisplay()));
  	roadWithLaneAndTraffic.draw();
	}
}
~~~
1. 가장 먼저 생성된 RoadDisplay 객체의 draw 메서드가 실행
2. 첫 번째 추가 기능인 LaneDecorator 클래스의 drawLane 메서드가 실행
3. 두 번째 추가 기능인 TrafficDecorator 클래스의 drawTraffic 메서드가 실행

* 교차로를 표시하는 추가 기능을 지원하면서 기존의 다른 추가 기능(차선 표시와 교통량 표시)과의 조합을 지원
  * CrossingDecorator 클래스
~~~Java
/* 교차로 표시를 추가하는 클래스 */
public class CrossingDecorator extends DisplayDecorator {
      // 기존 표시 클래스의 설정
      public CrossingDecorator(Display decoratedDisplay) { super(decoratedDisplay); }
      @Override
      public void draw() {
          super.draw(); // 설정된 기존 표시 기능을 수행
          drawCrossing(); // 추가적으로 교차로를 표시
	     }
      // 교차로 표시 기능만 직접 제공
      private void drawCrossing() { System.out.println("\t교차로 표시"); }
}
~~~
  * 클라이언트에서의 사용
~~~Java
public class Client {
      public static void main(String[] args) {
        // 기본 도로 표시 + 차선 표시 + 교통량 표시 + 교차로 표시
        Display roadWithCrossingLaneAndTraffic =
          new LaneDecorator(
          new TrafficDecorator(
          new CrossingDecorator(
          new RoadDisplay())));
        roadWithCrossingLaneAndTraffic.draw();
      }
}
~~~
  * CrossingDecorator를 DisplayDecorator의 하위 클래스로 설계한다.
    * CrossingDecorator의 draw 메서드가 호출되면 우선 상위 클래스(DisplayDecorator)의 draw 메서드를 호출한 후 CrossingDecorator의 drawCrossing 메서드를 호출한다.
  * roadWithCrossingLaneAndTraffic 객체의 draw 메서드를 호출하면
  1. 가장 먼저 생성된 RoadDisplay 객체의 draw 메서드가 실행
  2. 첫 번째 추가 기능인 CrossingDecorator 클래스의 drawCrossing 메서드가 실행
  3. 두 번째 추가 기능인 TrafficDecorator 클래스의 drawTraffic 메서드가 실행
  4. 세 번째 추가 기능인 LaneDecorator 클래스의 drawLane 메서드가 실행


# 관련된 Post
* 스트래티지(Strategy) 패턴에 대해 알고 싶으시면 [스트래티지(Strategy) 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)을 참고하시기 바랍니다.
* 싱글턴(Singleton) 패턴에 대해 알고 싶으시면 [싱글턴(Singleton) 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)을 참고하시기 바랍니다.
* 커맨드(Command) 패턴에 대해 알고 싶으시면 [커맨드(Command) 패턴](https://gmlwjd9405.github.io/2018/07/07/command-pattern.html)을 참고하시기 바랍니다.
* 옵저버(Observer) 패턴에 대해 알고 싶으시면 [옵저버(Observer) 패턴](https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html)을 참고하시기 바랍니다.
* 템플릿 메서드(Template Method) 패턴에 대해 알고 싶으시면 [템플릿 메서드(Template Method) 패턴](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)을 참고하시기 바랍니다.
* 팩토리 메서드(Factory Method) 패턴에 대해 알고 싶으시면 [팩토리 메서드(Factory Method) 패턴](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)을 참고하시기 바랍니다.
* 추상 팩토리(Abstract Factory) 패턴에 대해 알고 싶으시면 [추상 팩토리(Abstract Factory) 패턴](https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html)을 참고하시기 바랍니다.
* 컴퍼지트(Composite) 패턴에 대해 알고 싶으시면 [컴퍼지트(Composite) 패턴에](https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html)을 참고하시기 바랍니다.


# References
> - [JAVA 객체지향 디자인 패턴, 한빛미디어](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968480911&orderClick=JAj)
