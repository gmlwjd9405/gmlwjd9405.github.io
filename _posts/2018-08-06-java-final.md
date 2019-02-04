---
layout: post
title: '[Java] java final 키워드'
subtitle: 'java final 키워드의 활용 및 사용법을 이해한다.'
date: 2018-08-06
author: heejeong Kwon
cover: '/images/oop-solid/java-main.png'
tags: java final finally finalize
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - final 키워드의 사용법을 이해할 수 있다.
> - final, finally, finalize의 차이를 구분할 수 있다.

## final 클래스
final이 클래스 이름 앞에 사용되면 클래스를 **상속받을 수 없음** 을 지정한다.
~~~java
final class FinalClass {
  ...
}
class DerivedClass extends FinalClass { // 컴파일 오류
  ...
}
~~~
* FinalClass를 상속받아 DerivedClass를 만들 수 없다.


## final 메서드
메서드 앞에 final 속성이 붙으면 이 메서드는 더 이상 **오버라이딩할 수 없음** 을 지정한다.
~~~java
public class SuperClass {
  protected final int finalMethod() { ... }
}
class DerivedClass extends SuperClass { // SuperClass를 상속 받음
  protected int finalMethod() { ... } // 컴파일 오류
}
~~~
* 자식 클래스가 부모 클래스의 특정 메서드를 오버라이딩하지 못하게 하고 무조건 상속받아 사용하도록 하고자 한다면 final로 지정하면 된다.


## final 필드, 상수 정의
자바에서 상수를 정의하는 방법: 필드 멤버에 final 키워드를 덧붙여 상수를 정의한다.
~~~java
public class FinalFieldClass {
  final int ROWS = 10; // 상수 정의, 이때 초깃값(10)을 반드시 설정

  void f() {
    int[] intArray = new int[ROWS]; // 상수 활용
    ROWS = 30; // 컴파일 오류. final 필드 값은 상수으므로 변경할 수 없다.
  }
}
~~~
* final로 상수 필드를 정의할 때 **선언 시에 초깃값을 지정** 해야 한다.
* 상수 필드는 한 번 정의되면 값을 변경할 수 없다.
* final 키워드만을 사용하여 상수를 만들면 FinalFieldClass의 객체들만 사용할 수 있는 상수가 된다.

프로그램 전체에서 공유하여 사용할 수 있는 상수
* final 키워드 + static
* Ex) **public static final**

~~~java
class ShareClass {
  public static final double PI = 3.141592653589793;
}
/* ShareClass 내에서의 사용 */
double area = PI * radius * radius;
/* 다른 클래스에서의 사용 */
double area = ShareClass.PI * radius * radius;
~~~

## final, finally, finalize의 차이
### final 키워드
변수나 메서드 또는 클래스가 '변경 불가능'하도록 만든다.
* 원시(Primitive) 변수에 적용 시
  * 해당 변수의 값은 변경이 불가능하다.
* 참조(Reference) 변수에 적용 시
  * 참조 변수가 힙(heap) 내의 다른 객체를 가리키도록 변경할 수 없다.
* 메서드에 적용 시
  * 해당 메서드를 오버라이드할 수 없다.
* 클래스에 적용 시
  * 해당 클래스의 하위 클래스를 정의할 수 없다.

### finally 키워드
try/catch 블록이 종료될 때 항상 실행될 코드 블록을 정의하기 위해 사용한다.
* finally는 선택적으로 try 혹은 catch 블록 뒤에 정의할 때 사용한다.
* finally 블록은 예외가 발생하더라도 항상 실행된다.
  * 단, JVM이 try 블록 실행 중에 종료되는 경우는 제외한다.
* finally 블록은 종종 뒷마무리 코드를 작성하는 데 사용된다.
* finally 블록은 try와 catch 블록 다음과, 통제권이 이전으로 다시 돌아가기 전 사이에 실행된다.

~~~java
public static String lem() {
  System.out.println("lem");
  return "return from lem";
}
public static String foo() {
  int x = 0;
  int y = 5;
  try {
    System.out.println("start try");
    int b = y / x;
    System.out.println("end try");
    return "returned from try"
  } catch (Exception ex) {
    System.out.println("catch");
    return lem() + " | returned from catch";
  } finally {
    System.out.println("finally");
  }
}
public static void bar() {
  System.out.println("start bar");
  String v = foo();
  System.out.println(v);
  System.out.println("end bar");
}
// 출력
public static void main(String[] args) {
  bar();
}
~~~

~~~java
start bar
start try
catch
lem
finally
return from lem | returned from catch
end bar
~~~
* 출력 3~5번 줄까지를 보면, 함수 호출 내부에 있는 return문을 포함해서 catch 블록이 완전히 실행된 후에 실제로 반환한다.

### finalize() 메서드
쓰레기 수집기(GC, Garbage Collector)가 더 이상의 참조가 존재하지 않는 객체를 메모리에서 삭제하겠다고 결정하는 순간 호출된다.
* Object 클래스의 finalize() 메서드를 오버라이드해서 맞춤별 GC를 정의할 수 있다.
~~~java
protected void finalize() throws Throwable {
  /* 파일 닫기, 자원 반환 등등 */
}
~~~

<!-- # 관련된 Post
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다. -->


# References
> - [명품 Java Programming](https://www.booksr.co.kr/html/book/book.asp?seq=696811)
> - [코딩 인터뷰 완전 분석, 프로그래밍인사이트](https://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788966263080)
