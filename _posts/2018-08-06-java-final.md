---
layout: post
title: '[Java] java final 키워드'
subtitle: 'Java의 final 키워드의 사용법과 활용을 이해한다.'
date: 2018-08-06
author: heejeong Kwon
cover: '/images/java-programming/java-programming-main2.png'
tags: Java final
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - non-static 멤버와 static 멤버의 차이를 이해할 수 있다.
> - static 멤버의 사용법을 ㅇ이해할 수 있다.
> - static의 활용과 static 메서드의 제약 조건을 이해할 수 있다.

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
* Ex) public static final

~~~java
class ShareClass {
  public static final double PI = 3.141592653589793;
}
/* ShareClass 내에서의 사용 */
double area = PI * radius * radius;
/* 다른 클래스에서의 사용 */
double area = ShareClass.PI * radius * radius;
~~~


<!-- # 관련된 Post
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다. -->


# References
> - [명품 Java Programming](https://www.booksr.co.kr/html/book/book.asp?seq=696811)
