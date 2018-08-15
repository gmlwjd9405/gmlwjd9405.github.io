---
layout: post
title: '[Java] OOP(객체지향 프로그래밍)의 특징'
subtitle: 'OOP(객체지향 프로그래밍)의 원리를 이해한다.'
date: 2018-07-05
author: heejeong Kwon
cover: '/images/oop-features/java-main.png'
tags: java oop
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - OOP(객체지향 프로그래밍)의 4가지 특징을 이해한다.
> - 추상화를 이해할 수 있다.
> - 캡슐화를 이해할 수 있다.
> - 일반화 관계를 이해할 수 있다.
> - 다형성을 이해할 수 있다.


## 1. 추상화(Abstraction)
어떤 영역에서 필요로 하는 속성이나 행동을 추출하는 작업
* 사물들의 공통된 특징, 즉 추상적 특징을 파악해 인식의 대상으로 삼는 행위를 말한다.
* 구체적인 사물들의 공통적인 특징을 파악해서 이를 하나의 개념(집합)으로 다루는 수단을 말한다.
* ![](/images/oop-features/abstract.png)
<!-- {: width="230" height="100"} -->

각 개체의 구체적인 개념에 의존하지 말고 추상적 개념에 의존해야 설계를 유연하게 변경할 수 있다.
* 구체적인 개념에 의존하는 경우
~~~java
switch(자동차 종류)
  case 아우디: // 아우디 엔진 오일을 교환하는 과정을 기술
  case 벤츠: // 벤츠 엔진 오일을 교환하는 과정을 기술
  case BMW: // BMW 엔진 오일을 교환하는 과정을 기술
  ... 새로운 종류의 자동차가 나오면 계속해서 추가해야 한다.
end switch
~~~
* 추상적인 개념에 의존하는 경우
  ~~~java
  void changeEngineOil(Car c) {
    c.changeEngineOil(); // 추상 메서드
  }
  ~~~
  * changeEngineOil의 인자로 아우디, 벤츠의 추상화 개념인 "Car"을 사용한다.
  * 이 코드는 어떤 새로운 종류의 자동차가 나와도 **변경할 필요가 없다.**
  * 뒤에서 다룰 '다형성'의 원리에 따라 각 구체적인 클래스에서 오버라이드된 메서드(changeEngineOil)를 호출한다.

## 2. 캡슐화(Encapsulation)
<mark>참고</mark> SW 공학에서 요구사항 변경에 대처하는 고전적인 설계 원리
* 높은 응집도와 낮은 결합도를 유지할 수 있도록 설계해야 요구사항을 변경할 때 유연하게 대처할 수 있다.

1. 응집도(Cohesion)
* 클래스나 모듈 안의 요소들이 얼마나 밀접하게 관련되어 있는지를 나타낸다.
2. 결합도(Coupling)
* 어떤 기능을 실행하는 데 다른 클래스나 모듈들에 얼마나 의존적인지를 나타낸다.

<br>
캡슐화는 **낮은 결합도** 를 유지할 수 있도록 해주는 객체지향 설계 원리다.
* 캡슐화는 **정보 은닉** 을 통해 높은 응집도와 낮은 결합도를 갖도록 한다.
  * 정보 은닉(information hiding)
    * 필요가 없는 정보는 외부에서 접근하지 못하도록 제한하는 것
    * private 키워드
  * 정보 은닉이 왜 필요할까?
    * SW는 결합이 많을수록 문제가 많이 발생한다.
    * 한 클래스가 변경이 발생하면 변경된 클래스의 비밀에 의존하는 다른 클래스들도 변경해야 할 가능성이 커진다는 뜻이다.
* 예시
  * **강합 결합**
  ~~~java
  /* 자료구조 int[](배열)를 사용하여 Stack을 구현한 클래스 */
  public class ArrayStack {
      // 외부에 공개되어 있다.(public)
      public int top;
      public int[] itemArray;
      public int stackSize;
      // 생성자
      public ArrayStack(int stackSize){}
      // Stack 관련 메서드들
      public boolean isEmpty(){}
      public boolean isFull(){}
      public void push(int item){}
      public int pop(){}
      public int peek(){}
    }
  ~~~
  ~~~java
  /* 은닉 내용(자료구조 형태)을 직접 사용한 클래스 */
  public class StackClient {
      public static void main(String[] args) {
        ArrayStack st = new ArrayStack(10);
        st.itemArray[++st.top] = 20;
        System.out.println(st.itemArray[st.top]);
      }
  }
  ~~~
    * 만약 Stack을 구현한 클래스의 자료구조가 배열에서 ArrayList로 바뀐다면
    ~~~java
    /* 자료구조 ArrayList를 사용하여 Stack을 구현한 클래스 */
    public class ArrayListStack {
        // 외부에 공개되어 있다.(public)
        public ArrayList<Integer> items; // 자료구조 변경!
        public int stackSize;
        ...
      }
    ~~~
    ~~~java
    /* 은닉 내용(자료구조 형태)을 직접 사용한 클래스 */
    public class StackClient {
        public static void main(String[] args) {
          ArrayListStack st = new ArrayListStack(10);
          st.items.add(new Integer(10));
          System.out.println(st.items.get(st.items.size() - 1));
        }
    }
    ~~~
    * 이렇게 변경했더라도 자료구조는 필요에 따라 계속 변경될 수 있고 이는 자료구조가 변경될 때마다 코드도 계속 변경해야 한다는 의미다.
    * 즉, 오류를 수정하려고 코드를 변경하는 일이 오류를 발생하게 하는 원인이 될 수도 있다.
  * **약한 결합**
    * 변경되는 곳을 파악해 이를 은닉(private)해야 한다.
    * 자료구조와 같이 변경될 가능성이 큰 것은 외부에서 접근하지 못하도록 private 키워드를 붙여 은닉한다.
    ~~~java
    /* 자료구조 int[](배열)를 사용하여 Stack을 구현한 클래스 */
    public class ArrayStack {
        // 은닉되어 있다.(private)
        private int top;
        private int[] itemArray;
        private int stackSize;
        ...
    }
    ~~~
    ~~~java
    /* 은닉 내용(자료구조 형태)을 직접 사용한 클래스 */
    public class StackClient {
        public static void main(String[] args) {
          ArrayListStack st = new ArrayListStack(10);
          st.push(20);
          System.out.println(st.peek());
        }
    ~~~
    * 이후에는 push, pop, peek 메서드의 연산으로만 스택을 사용할 수 있다.
    * 외부에서는 push, pop, peek 메서드 등이 어떤 방식으로 어떤 자료구조를 사용해 작업을 실행하는지는 알 수 없다.
    * 즉, 스택과 이를 사용하는 코드의 결합이 낮아지는 것이다. ***(낮은 결합력!)***
  * 변하기 쉬운 것과 변하기 어려운 것
    * private
      * 변하기 쉬운 것은 감춘다!
      * 외부에서 변해도 영향을 받지 않는다.
      * Ex) 멤버 변수, 자료구조
    * public
      * 변하기 어려운 것은 드러낸다!
      * 변하기 어려우므로 외부에서 사용하는데 변경될 일이 적다.
      * Ex) Stack의 관련 메서드들 (push, pop의 기능)


## 3. 일반화 관계(Generalization)
일반화는 여러 개체들이 가진 공통된 특성을 부각시켜 하나의 개념이나 법칙으로 성립시키는 과정이다.
* 일반화 관계는 객체지향 프로그래밍 관점에서는 ***상속 관계*** 라 한다.
* 따라서 속성이나 기능의 재사용만 강조해서 사용하는 경우가 많다.
* 하지만 이는 일반화 관계를 극히 한정되게 바라보는 시각이다.


### 일반화는 또 다른 캡슐화
일반화 관계는 자식 클래스를 외부로부터 은닉하는 캡슐화의 일종이다.
* 예시
  <!--  * "딴 딴 따따따" 관계 -->
  * ![](/images/oop-features/generalization.png)
  * '사람'클래스 관점에서는 구체적인 자동차의 종류가 숨겨져 있다.
  * 즉, 대리 운전자가 자동차의 종류에 따라 운전에 영향을 받지는 않을 것이다.
  * 이와 같이 새로운 자동차를 운전해야 하는 경우에도 '사람'클래스는 영향을 받지 않는다.
* 일반화 관계는 한 클래스 안에 있는 속성 및 연산들의 캡슐화에 한정되지 않는다.
  * 즉, 외부 세계에 **자식 클래스 자체를 캡슐화(또는 은닉)하는 것** 으로 확장된다.
  * 서브 클래스의 캡슐화는 외부 클라이언트가 개별적인 클래스들과 무관하게 프로그래밍을 할 수 있게 한다.

### 일반화 관계와 위임
두 자식 클래스 사이에 "is a kind of" 관계가 성립되지 않을 때 상속을 사용하면 불필요한 속성이나 연산(빚이라고 해도 될 것이다)도 물려받게 된다.
* 많은 사람들이 일반화 관계를 속성이나 기능의 상속, 즉 재사용을 위해 존재한다고 오해하고 있다. 그러나 이는 사실이 아니다!
* 예시
  ~~~Java
  public class MyStack<String> extends ArrayList<String> {
    public void push(String element) { add(element); }
    public String pop() { return remove(size() - 1); }
  }
  ~~~
  * ArrayList의 isEmpty, size, add, remove 등의 메서드를 자신이 구현하지 않고 그대로 사용할 수 있다.
  * 그러나 ArrayList 클래스에 정의된 Stack과 전혀 관련 없는 수많은 연산이나 속성도 같이 상속받게 된다.
  * 문제점
    * Stack의 무결성 조건인 LIFO(Last In First Out)에 위배된다.
    ~~~Java
    public static void main(String[] args) {
        MyStack<String> st = new MyStack<String>();

        st.push("1");
        st.push("2");
        st.set(0, "3"); // 허용되어서는 안됨. LIFO 위배!
        System.out.println(st.pop());
        System.out.println(st.pop());
    }
    ~~~
  * Stack **"is a kind of"** ArrayList 관계가 아니기 때문에 일부 기능만을 사용하기 위해 부모로 만들지 않는다.
    * ArrayList 대신 Stack을 사용할 수 없으므로 위와 같이 사용하는 것은 바람직하지 못하다.
* 어떤 클래스의 일부 기능만 재사용하고 싶은 경우
  * <span style="background-color: #e1e1e1">**위임(delegation)**</span> 을 사용한다.
    * 자신이 직접 기능을 실행하지 않고 다른 클래스의 객체가 기능을 실행하도록 위임하는 것
    * 따라서 일반화 관계는 클래스 사이의 관계지만 위임은 **객체 사이의 관계** 다.
  * 즉, 기능을 재사용할 때는 위임을 이용하라.

<mark>위임을 사용해 일반화(상속)을 대신하는 과정</mark>
1. 자식 클래스에 부모 클래스의 인스턴스를 참조하는 속성을 만든다.
  * 이 속성 필드를 this로 초기화한다.
2. 자식 클래스에 정의된 각 메서드에 1번에서 만든 위임 속성 필드를 참조하도록 변경한다.
3. 자식 클래스에서 일반화 관계 선언을 제거하고 위임 속성 필드에 부모 클래스의 객체를 생성해 대입한다.
4. 자식 클래스에서 사용된 부모 클래스의 메서드를 추가하고 해당 메서드에도 속성 필드를 참조하도록 변경한다.
5. 컴파일하고 잘 동작하는지 확인한다.

* 위의 잘못된 일반화 예시 코드를 수정하는 과정
~~~Java
public class MyStack<String> extends ArrayList<String> {
    public void push(String element) { add(element); }
    public String pop() { return remove(size() - 1); }
}
~~~
  * 1) 부모 클래스의 인스턴스를 참조하는 속성(this)을 만들고
  * 2) 위임 속성 필드를 참조하도록 변경한다.
~~~Java
public class MyStack<String> extends ArrayList<String> {
      // 1. 부모 클래스의 인스턴스를 참조하는 속성(this)
      private ArrayList<String> arrayList = this;
      // 2. arrayList.~ 추가
      public void push(String element) { arrayList.add(element); }
      public String pop() { return arrayList.remove(size() - 1); }
}
~~~
  * 3) 일반화 관계를 제거하고 슈퍼 클래스 객체를 생성 후 대입한다.
  * 4) 자식 클래스에서 사용된 부모 클래스의 메서드에도 위임 속성 필드를 참조하도록 변경한다.
~~~Java
// 3. 일반화 관계 제거
public class MyStack<String> {
      // 3. 슈퍼 클래스 객체를 생성 후 대입
      private ArrayList<String> arrayList = new ArrayList<String>();
      // 동일
      public void push(String element) { arrayList.add(element); }
      public String pop() { return arrayList.remove(size() - 1); }
      // 4. 사용된 메서드 추가 및 위임 속성 필드를 참조하도록 변경
      public boolean isEmpty() { return arrayList.isEmpty(); }
      public int size() { return arrayList.size(); }
}
~~~

<!-- ### 집합론 관점으로 본 일반화 관계 -->


## 4. 다형성(Polymorphism)
다형성은 서로 다른 클래스의 객체가 같은 메시지를 받았을 때 각자의 방식으로 동작하는 능력이다.
* 다형성이 상속과 연계되어 동작하면 매우 강력한 힘을 발휘한다.
* 다형성과 일반화 관계는 코드를 간결하게 할 뿐 아니라 변화에도 유연하게 대처할 수 있게 한다.
* 예시
  * 다형성을 사용하지 않는 경우
  ~~~Java
  public class Cat {
      public void meow(){ System.out.println("야옹"); }
  }
  public class Dog {
      public void bark(){ System.out.println("멍멍"); }
  }
  public class Parrot {
      public void sing(){ System.out.println("안녕"); }
  }
  ~~~
  ~~~Java
  public class Main {
      public static void main(String[] args) {
        Cat cat = new Cat();
        Dog dog = new Dog();
        Parrot parrot = new Parrot();
        // 애완동물 세 마리의 울음소리 호출
        cat.meow(); dog.bark(); parrot.sing();
      }
  }
  ~~~

  * 다형성을 사용한 경우
~~~Java
// 부모 클래스
public abstract class Pet {
      public abstract void talk();
}
// 자식 클래스
public class Cat extends Pet {
      public void talk(){ System.out.println("야옹"); }
}
public class Dog extends Pet {
      public void talk(){ System.out.println("멍멍"); }
}
public class Parrot extends Pet {
      public void talk(){ System.out.println("안녕"); }
}
~~~
~~~Java
public class Main {
      public static void main(String[] args) {
        Pet[] pets = { new Cat(), new Dog(), new Parrot() };
        // 애완동물 세 마리의 울음소리 호출
        for (int i = 0; i < 3; i++){
          // 실제 참조하는 객체에 따라 talk 메서드가 실행된다.
          pets.talk();
        }
      }
}
~~~
* 다형성을 사용하는 경우에는 구체적으로 현재 어떤 클래스 객체가 참조되는지와 무관하게 프로그래밍을 할 수 있다.
* 일반화 관계에 있을 때 부모 클래스의 참조 변수가 자식 클래스의 객체를 참조할 수 있기 때문에 새로운 자식 클래스가 추가되더라도 코드는 영향을 받지 않는다.
* 단, 부모 클래스의 참조 변수가 접근할 수 있는 것은 부모 클래스가 물려준 변수와 메서드뿐이다.



## 피터 코드의 상속 규칙(Peter Coad)
상속의 오용을 막기 위해 상속의 사용을 엄격하게 제한하는 규칙들
* 5가지 규칙 중 어느 하나라도 만족하지 않는다면 상속을 사용해서는 안된다.
1. 자식 클래스와 부모 클래스 사이는 '역할 수행'관계가 아니어야 한다.
2. 한 클래스의 인스턴스는 다른 자식 클래스의 객체로 변환할 필요가 절대 없어야 한다.
3. 자식 클래스가 부모 클래스의 책임을 무시하거나 재정의하지 않고 확장만 수행해야 한다.
4. 자식 클래스가 단지 일부 기능을 재사용할 목적으로 유틸리티 역할을 수행하는 클래스를 상속하지 않아야 한다.
5. 자식 클래스가 '역할', '트랜잭션', '디바이스' 등을 특수화해야 한다.

* ![](/images/oop-features/etc.png)

# 관련된 Post
* OOP(객체지향 프로그래밍) 설계 원칙에 대해 알고 싶으시면 [OOP 설계 원칙](https://gmlwjd9405.github.io/2018/07/05/oop-solid.html)을 참고하시기 바랍니다.

# References
> - [JAVA 객체지향 디자인 패턴, 한빛미디어](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968480911&orderClick=JAj)
