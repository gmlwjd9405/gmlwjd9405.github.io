---
layout: post
title: '[Java] java static 멤버와 static 메서드'
subtitle: 'java static의 활용 및 사용법을 이해한다.'
date: 2018-08-04
author: heejeong Kwon
cover: '/images/oop-solid/java-main.png'
tags: java static
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - non-static 멤버와 static 멤버의 차이를 이해할 수 있다.
> - static 멤버의 사용법을 이해할 수 있다.
> - static의 활용과 static 메서드의 제약 조건을 이해할 수 있다.

## static 멤버의 선언
멤버 선언 시 앞에 static이라고 붙인다.
~~~java
class StaticSample {
  int n; // non-static 필드
  void g() {...} // non-static 메서드

  static int m; // static 필드
  static void f() {...} // static 메서드
}
~~~

## non-static 멤버 VS static 멤버
non-static 멤버
* 공간적 특성: **멤버는 객체마다 별도로 존재한다.**
  * ***인스턴스 멤버*** 라고 부른다.
* 시간적 특성: **객체 생성 시에 멤버가 생성된다.**
  * 객체가 생길 때 멤버도 생성된다.
  * 객체 생성 후 멤버 사용이 가능하다.
  * 객체가 사라지면 멤버도 사라진다.
* 공유의 특성: **공유되지 않는다.**
  * 멤버는 객체 내에 각각의 공간을 유지한다.

<br>
static 멤버
* 공간적 특성: **멤버는 클래스당 하나가 생성된다.**
  * 멤버는 객체 내부가 아닌 별도의 공간에 생성된다.
  * ***클래스 멤버*** 라고 부른다.
* 시간적 특성: **클래스 로딩 시에 멤버가 생성된다.**
  * 객체가 생기기 전에 이미 생성된다.
  * 객체가 생기기 전에도 사용이 가능하다. (즉, 객체를 생성하지 않고도 사용할 수 있다.)
  * 객체가 사라져도 멤버는 사라지지 않는다.
  * 멤버는 프로그램이 종료될 때 사라진다.
* 공유의 특성: **동일한 클래스의 모든 객체들에 의해 공유된다.**


## static 멤버의 사용법
### 1. 객체.static 멤버
static 멤버도 역시 멤버이기 때문에 일반적인 멤버 사용법과 다를 바 없다.
~~~java
객체.static 멤버
객체.static 메서드
~~~

* static 필드는 클래스의 모든 객체에 공통으로 사용되는 변수가 된다.
* C/C++의 전역 변수(global variable)와 유사하다고 볼 수 있다.
* 클래스의 어떤 멤버도 static 멤버를 접근할 수 있다.

**static 멤버의 생성과 공유**
~~~java
class StaticSample {
  public int n;
  public void g() { m = 20; }
  public void h() { m = 30; }
  public static int m;
  public static void f() { m = 5; }
}
~~~
* static 멤버가 생성되는 시점은 main() 메서드가 실행을 시작한 후 StaticSample이 등장하는 시점이다.
* 다음 코드가 실행되기 이전부터 static 멤버 m과 f()는 이미 존재하며 사용이 가능하다.

~~~java
public class Main {
  public static void main(String args[]) {
    StaticSample s1, s2;
    s1 = new StaticSample();
    s1.n = 5;
    s1.g();
    s1.m = 50; // static

    s2 = new StaticSample();
    s2.n = 8;
    s2.h();
    s2.f(); // static
    System.out.println(s1.m); // result: 5
  }
}
~~~
* static 멤버 m과 f()는 객체 s1, s2가 생성되기 이전에 이미 생성되어 있으며,
  * s1, s2가 생성될 때 static 멤버가 별도로 생성되는 것은 아니다.
* s1 = new StaticSample();와 s2 = new StaticSample();가 실행되면,
  * 각 객체마다(s1, s2) 인스턴스 멤버인 n, g(), h()가 생성된다.
* 객체 s1, s2는 static 멤버를 공유하고 있다.
  * 즉, s1, s2 모두 자신의 멤버라고 생각하여 g(), h() 메서드에서 static 멤버 m을 공유하여 사용하고 있다.


### 2. 클래스명.static 멤버
static 멤버는 클래스당 하나만 있기 때문에 클래스 이름으로 바로 접근할 수 있다.
~~~java
클래스명.static 멤버
~~~

* new 에 의해 객체가 생기기 전에 static 멤버에 접근할 수 있다.

**static 멤버의 생성과 공유**
~~~java
class StaticSample {
  위와 동일
}
~~~
~~~java
public class Main {
  public static void main(String args[]) {
    StaticSample.m = 10;

    StaticSample s1;
    s1 = new StaticSample();
    System.out.println(s1.m); // result: 10

    /* static 메서드 사용 */
    s1.f(); // 1. 객체 레퍼런스로 static 멤버 f() 호출
    StaticSample.f(); // 2. 클래스명을 이용하여 static 멤버 f() 호출
  }
}
~~~
* StaticSample.h();, StaticSample.g();
  * h(), g() 메서드는 non-static 이므로 오류

<mark>주의</mark>
* 실제 static 멤버의 생성 시점은 JVM(자바 가상 기계)에 따라 다르다.
* 그러나 일반적으로 static 멤버가 포함된 클래스가 로딩하는 시점에 static 멤버가 생성된다고 볼 수 있다.
* JVM은 많은 경우 처음부터 필요한 대부분의 클래스를 로딩하기 때문에 static 멤버의 생성 시점은 JVM이 시작되는 시점이라고 할 수 있다.


## static의 활용
### 1. 전역 변수와 전역 함수를 만들 때 활용
* 캡슐화 원칙
  * 자바에서는 C/C++와 달리 어떤 변수나 함수도 클래스 바깥에 존재할 수 없으며 클래스의 멤버로 존재하여야 한다.
* 그러나 응용프로그램 작성 시 모든 클래스에서 공유하는 전역 변수(global variable)나 모든 클래스에서 언제든지 호출할 수 있는 전역 함수(global function)를 만들어 사용할 필요가 생긴다.
  * static은 이런 문제의 해결책이다.
  * Ex) java.lang.Math 클래스
    * JDK와 함께 배포되는 클래스
    * 객체를 생성하지 않고 바로 호출할 수 있는 수학 계산용 상수와 메서드를 제공
~~~java
public class Math {
  public static int abs(int a);
  public static double cos(double a);
  ... // 모든 멤버가 static
}
~~~
~~~java
/* 잘못된 사용법 */
Math m = new Math(); // 현재 생성자 Math()는 private으로 선언되어 있어 객체 생성 안 됨
int n = m.abs(-5);
/* 올바른 사용법 */
int n = Math.abs(-5);
~~~

### 2. 공유 멤버를 만들고자 할 때 활용
static으로 선언된 필드나 메서드는 모두 이 클래스의 각 객체들의 공통 멤버가 되며 객체들 사이에서 공유된다.


## static 메서드의 제약 조건
### 1. static 메서드는 오직 static 멤버만 접근할 수 있다.
static 메서드는 객체가 생성되지 않은 상황에서도 사용이 가능하므로 객체에 속한 인스턴스 메소드, 인스턴스 변수 등을 사용할 수 없다.
* static 멤버들만 사용이 가능하다.
* 그러나 인스턴스 메서드는 static 멤버들을 모두 사용할 수 있다.

~~~java
class StaticMethod {
  int n;
  void f1(int x) { n = x; } // 정상
  void f2(int x) { n = x; } // 정상

  static int m;
  static void s1(int x) { n = x; } // 컴파일 오류. static 메서드는 non-static 필드 사용 불가
  static void s2(int x) { f1(3); } // 컴파일 오류. static 메서드는 non-static 메서드 사용 불가

  static void s3(int x) { m = x; } // 정상. static 메서드는 static 필드 사용 가능
  static void s4(int x) { s3(3); } // 정상. static 메서드는 static 메서드 호출 가능
}
~~~

### 2. static 메서드에서는 this 키워드를 사용할 수 없다.
this는 호출 당시 실행 중인 객체를 가리키는 레퍼런스이다.
* 따라서 객체가 생성되지 않은 상황에서도 클래스 이름을 이용하여 호출이 가능한 static 메서드는 this를 사용할 수 없다.

~~~java
class StaticAndThis {
  int n;
  static int m;
  void f1(int x) { this.n = x; } // 정상
  void f2(int x) { this.m = x; } // non-static 메서드에서는 static 멤버 접근 가능
  static void s1(int x) { this.n = x; } // 컴파일 오류. static 메서드는 this 사용 불가
}
~~~


<!-- # 관련된 Post
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다. -->


# References
> - [명품 Java Programming](https://www.booksr.co.kr/html/book/book.asp?seq=696811)
