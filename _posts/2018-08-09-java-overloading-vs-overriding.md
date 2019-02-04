---
layout: post
title: '[Java] 오버로딩과 오버라이딩의 차이(Overloading VS Overriding)'
subtitle: 'java 오버로딩과 오버라이딩의 차이에 대해 이해한다.'
date: 2018-08-09
author: heejeong Kwon
cover: '/images/oop-solid/java-main.png'
tags: java overloading overriding
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 오버로딩(Overloading)에 대해 이해할 수 있다.
> - 오버라이딩(Overriding)에 대해 이해할 수 있다.

## 오버로딩(Overloading)
### 오버로딩이란
두 메서드가 같은 이름을 갖고 있으나 **인자의 수나 자료형이 다른** 경우를 말한다.

### 오버로딩 예시
~~~java
public double computeArea(Circle c) { ... }
public double computeArea(Circle c1, Circle c2) { ... }
public double computeArea(Square c) { ... }
~~~


<br>
## 오버라이딩(Overriding)
### 오버라이딩이란
상위 클래스의 메서드와 이름과 용례(signature)가 같은 함수를 하위 클래스에 재정의하는 것을 말한다.
* 즉, 상속 관계에 있는 클래스 간에 같은 이름의 메서드를 정의하는 것을 말한다.


### 오버라이딩 예시
~~~java
public abstract class Shape {
  public void printMe() { System.out.println("Shape"); }
  public abstract double computeArea();
}
public class Circle extends Shape {
  private double rad = 5;
  @Override // 개발자의 실수를 방지하기 위해 @Override(annotation) 쓰는 것을 권장
  public void printMe() { System.out.println("Circle"); }
  public double computeArea() { return rad * rad * 3.15; }
}
public class Ambiguous extends Shape {
  private double area = 10;
  public double computeArea() { return area; }
}
~~~
~~~java
public class Main {
  public static void main(String[] args) {
    Shape[] shapes = new Shape[2];
    Circle circle = new Circle();
    Ambiguous ambiguous = new Ambiguous();

    shapes[0] = circle;
    shapes[1] = ambiguous;

    for(Shape s : shapes) {
      s.printMe();
      System.out.println(s.computeArea());
    }
  }
}
~~~
~~~java
출력 결과
Circle
78.75
Shape
10
~~~
* Circle에서 printMe() 메서드를 재정의한다.
* 오버라이딩을 할 때 개발자의 실수를 방지하기 위해 메서드 위에 @Override 를 관례적으로 적는다.



<!-- ### 상속과 관련한 오버라이딩의 추가 개념 -->


<!-- # 관련된 Post
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다. -->

<!-- 상속 Post 작성 후 업데이트! -->


# References
> - [코딩 인터뷰 완전 분석, 프로그래밍인사이트](https://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788966263080)
> - [http://itpangpang.xyz/105](http://itpangpang.xyz/105)
