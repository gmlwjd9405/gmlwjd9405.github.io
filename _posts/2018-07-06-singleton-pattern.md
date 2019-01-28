---
layout: post
title: '[Design Pattern] 싱글턴 패턴이란'
subtitle: '싱글턴 패턴을 이해한다.'
date: 2018-07-06
author: heejeong Kwon
cover: '/images/design-pattern-singleton/singleton-main.png'
tags: 디자인패턴 design-pattern singleton
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 싱글턴 패턴의 개념을 이해한다.
> - 예시를 통해 싱글턴 패턴을 이해한다.


## 싱글턴 패턴이란
* 전역 변수를 사용하지 않고 **객체를 하나만 생성** 하도록 하며, 생성된 객체를 **어디에서든지 참조할 수 있도록** 하는 패턴
  * '생성(Creational) 패턴'의 하나 (*아래 참고*)
* ![](/images/design-pattern-singleton/singleton-example.png){: width="150" height="100"}
* 역할이 수행하는 작업
  * Singleton
    * 하나의 인스턴스만을 생성하는 책임이 있으며 getInstance 메서드를 통해 모든 클라이언트에게 동일한 인스턴스를 반환하는 작업을 수행한다.


<mark>참고</mark>
* 생성(Creational) 패턴
  * 객체 생성에 관련된 패턴
  * 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공한다.


## 예시
### 프린터 관리자 만들기
* 프린트 하나를 10명이 공유해서 사용한다고 하자.
~~~Java
public class Printer {
    public Printer() { }
    public void print(Resource r) { ... }
}
~~~
* 그러나 Printer 클래스를 사용해 프린터를 이용하려면 Client 프로그램에서 new Printer()가 반드시 한 번만 호출되도록 주의해야 한다. (프린터는 하나이기 때문에)
* 이를 해소하는 방법은 생성자를 외부에서 호출할 수 없게 하는 것이다.
  * Printer 클래스의 생성자를 private으로 선언
~~~Java
public class Printer {
      private Printer() { } // Printer 생성자를 외부에서 사용 불가
      public void print(Resource r) { ... }
}
~~~
* 자기 자신 프린터에 대한 인스턴스를 하나 만들어 외부에 제공해줄 메서드가 필요하다.
  * static 메서드 / static 변수
    * 구체적인 인스턴스에 속하는 영역이 아니고 클래스 자체에 속한다.
    * 클래스의 인스턴스를 통하지 않고서도 메서드를 실행할 수 있고 변수를 참조할 수 있다.
  * 만약 new Printer()가 호출되기 전이면 인스턴스 메서드인 print() 메서드는 호출할 수 없다.
~~~Java
public class Printer {
      // 외부에 제공할 자기 자신의 인스턴스
      private static Printer printer = null;
      private Printer() { }
      // 자기 자신의 인스턴스를 외부에 제공
      public static Printer getPrinter(){
        if (printer == null) {
          // Printer 인스턴스 생성
          printer = new Printer();
        }
        return printer;
      }
      public void print(String str) {
        System.out.println(str);
      }
}
~~~

* Client에서의 사용
~~~java
public class User {
    private String name;
    public User(String name) { this.name = name; }
    public void print() {
      Printer printer = printer.getPrinter();
      printer.print(this.name + " print using " + printer.toString());
    }
}
public class Client {
    private static final int USER_NUM = 5;
    public static void main(String[] args) {
      User[] user = new User[USER_NUM];
      for (int i = 0; i < USER_NUM; i++) {
        // User 인스턴스 생성
        user[i] = new User((i+1))
        user[i].print();
      }
    }
}
~~~


### 문제점
**다중 스레드에서** Printer 클래스를 이용할 때 인스턴스가 1개 이상 생성되는 경우가 발생할 수 있다.
* **경합 조건(Race Condition)** 을 발생시키는 경우
  1. Printer 인스턴스가 아직 생성되지 않았을 때 스게드 1이 getPrinter 메서드의 if문을 실행해 이미 인스턴스가 생성되었는지 확인한다. 현재 printer 변수는 null인 상태다.
  2. 만약 스레드 1이 생성자를 호출해 인스턴스를 만들기 전 스레드 2가 if문을 실행해 printer 변수가 null인지 확인한다. 현재 printer 변수는 null이므로 인스턴스를 생성하는 생성자를 호출하는 코드를 실행하게 된다.
  3. 스레드 1도 스레드 2와 마찬가지로 인스턴스를 생성하는 코드를 실행하게 되면 결과적으로 Printer 클래스의 인스턴스가 2개 생성된다.
* 경합 조건이란?
  * 메모리와 같은 동일한 자원을 2개 이상의 스레드가 이용하려고 경합하는 현상
* 스레드 스케줄링을 고의로 변경하여 경합 조건을 만들어보자.
~~~Java
public class Printer {
      // 외부에 제공할 자기 자신의 인스턴스
      private static Printer printer = null;
      private Printer() { }
      // 자기 자신의 인스턴스를 외부에 제공
      public static Printer getPrinter(){
        // 조건 검사 구문 (문제의 원인!)
        if (printer == null) {
          try {
            // 스레드 스케줄링 변경(스레드 실행 1ms동안 정지)
            Thread.sleep(1);
          } catch (InterruptedException e) { }

          // Printer 인스턴스 생성
          printer = new Printer();
        }
        return printer;
      }
      public void print(String str) {
        System.out.println(str);
      }
}
~~~
~~~Java
public class UserThread extends Thread{
    public UserThread(String name) { super(name); }
    public void run() {
      Printer printer = printer.getPrinter();
      printer.print(Thread.currentThread().getName() + " print using " + printer.toString());
    }
}
public class Client {
    private static final int THREAD_NUM = 5;
    public static void main(String[] args) {
      UserThread[] user = new UserThread[THREAD_NUM];
      for (int i = 0; i < THREAD_NUM; i++) {
        // UserThread 인스턴스 생성
        user[i] = new UserThread((i+1));
        user[i].start();
      }
    }
}
~~~


### 해결책
프린터 관리자(Lazy Initialization)는 사실 **다중 스레드 애플리케이션이 아닌 경우에는 아무런 문제가 되지 않는다.**
* 다중 스레드 애플리케이션에서 발생하는 문제를 해결하는 방법
1. 정적 변수에 인스턴스를 만들어 바로 초기화하는 방법 (Eager Initialization)
2. 인스턴스를 만드는 메서드에 동기화하는 방법 (Thread-Safe Initialization)

1. 정적 변수에 인스턴스를 만들어 바로 초기화하는 방법
~~~Java
public class Printer {
      // static 변수에 외부에 제공할 자기 자신의 인스턴스를 만들어 초기화
      private static Printer printer = new Printer();
      private Printer() { }
      // 자기 자신의 인스턴스를 외부에 제공
      public static Printer getPrinter(){
        return printer;
      }
      public void print(String str) {
        System.out.println(str);
      }
}
~~~
* static 변수
  * 객체가 생성되기 전 클래스가 메모리에 로딩될 때 만들어져 초기화가 한 번만 실행된다.
  * 프로그램 시작~종료까지 없어지지 않고 메모리에 계속 상주하며 클래스에서 생성된 모든 객체에서 참조할 수 있다.
2. 인스턴스를 만드는 메서드에 동기화하는 방법
~~~Java
public class Printer {
      // 외부에 제공할 자기 자신의 인스턴스
      private static Printer printer = null;
      private int counter = 0;
      private Printer() { }
      // 인스턴스를 만드는 메서드 동기화 (임계 구역)
      public synchronized static Printer getPrinter(){
        if (printer == null) {
          printer = new Printer(); // Printer 인스턴스 생성
        }
        return printer;
      }
      public void print(String str) {
        // 오직 하나의 스레드만 접근을 허용함 (임계 구역)
        // 성능을 위해 필요한 부분만을 임계 구역으로 설정한다.
        synchronized(this) {
          counter++;
          System.out.println(str + counter);
        }
      }
}
~~~
* 인스턴스를 만드는 메서드를 **임계 구역으로 변경**
  * 다중 스레드 환경에서 동시에 여러 스레드가 getPrinter 메서드를 소유하는 객체에 접근하는 것을 방지한다.
* 공유 변수에 접근하는 부분을 **임계 구역으로 변경**
  * 여러 개의 스레드가 하나뿐인 counter 변수 값에 동시에 접근해 갱신하는 것을 방지한다.
* getInstance()에 Lock을 하는 방식이라 속도가 느리다.

## 정적 클래스
정적 메서드로만 이루어진 정적 클래스를 사용하면 싱글턴과 동일한 효과를 얻을 수 있다.
~~~Java
public class Printer {
      private static int counter = 0;
      // 메서드 동기화 (임계 구역)
      public synchronized static void print(String str) {
        counter++;
        System.out.println(str + counter);
      }
}
~~~
~~~Java
public class UserThread extends Thread{
    // 스레드 생성
    public UserThread(String name) { super(name); }
    // 현재 스레드 이름 출력
    public void run() {
      Printer.print(Thread.currentThread().getName());
    }
}
public class Client {
    private static final int THREAD_NUM = 5;
    public static void main(String[] args) {
      UserThread[] user = new UserThread[THREAD_NUM];
      for (int i = 0; i < THREAD_NUM; i++) {
        // UserThread 인스턴스 생성
        user[i] = new UserThread((i+1));
        user[i].start();
      }
    }
}
~~~
* 차이점
  * 정적 클래스를 이용하면 객체를 전혀 생성하지 않고 메서드를 사용한다.
  * 정적 메서드를 사용하므로 일반적으로 실행할 때 바인딩되는(컴파일 타임에 바인딩되는) 인스턴스 메서드를 사용하는 것보다 성능 면에서 우수하다.
* 정적 클래스를 사용할 수 없는 경우
  * 인터페이스를 구현해야 하는 경우, 정적 메서드는 인터페이스에서 사용할 수 없다.
* 인터페이스를 사용하는 주된 이유?
  * 대체 구현이 필요한 경우
  * 예를 들어 Mock 객체를 사용해 단위 테스트를 수행하는 경우

## Enum 클래스 
```java
public enum SingletonTest {
	INSTANCE;
  
	public static SingletonTest getInstance() {		
		return INSTANCE;
	}
}
```
* Thread-safety와 Serialization이 보장된다.
* Reflection을 통한 공격에도 안전하다.
* 따라서 Enum을 이용해서 Singleton을 구현하는 것이 가장 좋은 방법이다.


# 관련된 Post
* 스트래티지(Strategy) 패턴에 대해 알고 싶으시면 [스트래티지(Strategy) 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)을 참고하시기 바랍니다.
* 커맨드(Command) 패턴에 대해 알고 싶으시면 [커맨드(Command) 패턴](https://gmlwjd9405.github.io/2018/07/07/command-pattern.html)을 참고하시기 바랍니다.
* 옵저버(Observer) 패턴에 대해 알고 싶으시면 [옵저버(Observer) 패턴](https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html)을 참고하시기 바랍니다.
* 데코레이터(Decorator) 패턴에 대해 알고 싶으시면 [데코레이터(Decorator) 패턴](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)을 참고하시기 바랍니다.
* 템플릿 메서드(Template Method) 패턴에 대해 알고 싶으시면 [템플릿 메서드(Template Method) 패턴](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)을 참고하시기 바랍니다.
* 팩토리 메서드(Factory Method) 패턴에 대해 알고 싶으시면 [팩토리 메서드(Factory Method) 패턴](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)을 참고하시기 바랍니다.
* 추상 팩토리(Abstract Factory) 패턴에 대해 알고 싶으시면 [추상 팩토리(Abstract Factory) 패턴](https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html)을 참고하시기 바랍니다.
* 컴퍼지트(Composite) 패턴에 대해 알고 싶으시면 [컴퍼지트(Composite) 패턴에](https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html)을 참고하시기 바랍니다.

# References
> - [JAVA 객체지향 디자인 패턴, 한빛미디어](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968480911&orderClick=JAj)
> - [https://blog.vjvj.net/2017/04/effective-java-3-private-enum-singleton.html](https://blog.vjvj.net/2017/04/effective-java-3-private-enum-singleton.html)