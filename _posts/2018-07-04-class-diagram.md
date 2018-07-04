---
layout: post
title: '[UML] 클래스 다이어그램 작성법'
subtitle: '클래스 다이어그램 작성법'
date: 2018-07-04
author: heejeong Kwon
cover: '/images/java-programming/java-programming-main2.png'
tags: Java
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 클래스 다이어그램을 이해할 수 있다.
> - 클래스 다이어그램을 작성할 수 있다.


<br><br>
[들어가기 전]
* UML(Unified Modeling Language)이란
  * 시스템을 모델로 표현해주는 대표적인 모델링 언어
* UML 다이어그램의 종류
  1. 구조 다이어그램(Structure Diagram)
    * **클래스 다이어그램,** 객체 다이어그램, 복합체 구조 다이어그램, 배치 다이어그램, 컴포넌트 다이어그램, 패키지 다이어그램
  2. 행위 다이어그램(Behavior Diagram)
    * 활동 다이어그램, 상태 머신 다이어그램, 유즈 케이스 다이어그램, 상호작용 다이어그램


## 클래스 다이어그램이란
시간에 따라 변하지 않는 시스템의 정적인 면을 보여주는 대표적인 UML 구조 다이어그램
* 목적: 시스템을 구성하는 클래스들 사이의 관계를 표현한다.


## 1. 클래스
* 클래스(Class)란
1. 동일한 속성과 행위를 수행하는 객체의 집합
2. 객체를 생성하는 설계도
  * 즉, 클래스는 공통의 속성과 책임을 갖는 객체들의 집합이자 실제 객체를 생성하는 설계도이다.

<!-- ~~~Java
public class Cat {
  private String name;

  public void meow() {
    System.out.println(name + "~~ 웁니다.");
  }

  public Cat(String name) {
    this.name = name;
  }
}
~~~   -->

* UML 클래스의 표현
  * 가장 윗부분: 클래스 이름
  * 중간 부분: 속성(클래스의 특징)
  * 마지막 부분: 연산(클래스가 수행하는 책임)
  <!-- * ![](/images/class-diagram/access-controller.png) -->
  * 경우에 따라 속성 부분과 연산 부분은 생략할 수 있다.
  * 속성과 연산의 가시화를 정의
    * UML에서는 접근제어자를 사용해 나타낸다.
    * ![](/images/class-diagram/access-controller.png)
  * 분석 단계와 설계 단계에서의 클래스 다이어그램
    <!-- * ![](/images/class-diagram/access-controller.png) -->


## 2. 관계
* UML에서 제공하는 클래스들 사이의 관계
  <!-- * ![](/images/class-diagram/access-controller.png) -->

### 연관 관계
* 한 클래스가 다른 클래스와 **연관 관계** 를 가지면 각 클래스의 객체는 해당 연관 관계에서 어떤 **역할** 을 수행하게 된다.
  * 두 클래스 사이의 연관 관계가 명확한 경우에는 연관 관계 이름을 사용하지 않아도 된다.
  * 역할 이름은 실제 프로그램을 구현할 때 연관된 클래스의 객체들이 서로를 참조할 수 있는 속성의 이름으로 활용할 수 있다.
* 실선
  * 양방향 연관 관계: 두 클래스의 객체들이 서로의 존재를 인식한다.
  * 일반적으로 **다대다** 연관 관계는 양방향 연관 관계로 표현되는 것이 적절하다.
* 화살표
  * 단방향 연관 관계: 한 쪽은 알지만 다른 쪽은 상대방의 존재를 모른다.
  *
* 다중성 표시
  <!-- * ![](/images/class-diagram/access-controller.png) -->

# 관련된 Post

# References
> - []()
