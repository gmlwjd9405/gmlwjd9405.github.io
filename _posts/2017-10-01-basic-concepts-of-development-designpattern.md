---
layout: post
title: '[기술 면접 질문] 기술 면접 예상 질문 대비하기 - 디자인패턴(Design Pattern)편'
subtitle: '기술 면접 예상 질문 대비하기 - 디자인패턴(Design Pattern)편'
date: 2017-10-01
author: heejeong Kwon
cover: '/images/basic-concepts-of-development/basic-concepts-of-development-main.png'
tags: 면접 기본개념 디자인패턴 design-pattern
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## <mark>디자인패턴(Design Pattern)편</mark>  
<span style="color:#4d0000">디자인패턴(design pattern)과 관련된 기본적인 개념을 이해하고 기술 면접에 대비하자!</span>  

<br> <span style="background-color: #e1e1e1">계속해서 추가할 예정입니다!<span>

### 클래스 다이어그램 작성법
> - 관련 POST
> - [https://gmlwjd9405.github.io/2018/07/04/class-diagram.html](https://gmlwjd9405.github.io/2018/07/04/class-diagram.html)

### 디자인 패턴의 이해
* 디자인 패턴이란
  * 소프트웨어를 설계할 때 특정 맥락에서 자주 발생하는 고질적인 문제들이 또 발생했을 때 재사용할 할 수있는 훌륭한 해결책
  * "바퀴를 다시 발명하지 마라(Don't reinvent the wheel)"
  * 이미 만들어져서 잘 되는 것을 처음부터 다시 만들 필요가 없다는 의미이다.
* GoF 디자인 패턴의 분류
  * GoF(Gang of Fout)라 불리는 사람들이 23가지의 디자인 패턴을 정리하고 각각의 디자인 패턴을 생성(Creational), 구조(Structural), 행위(Behavioral) 3가지로 분류했다.
  * 에리히 감마(Erich Gamma), 리차드 헬름(Richard Helm), 랄프 존슨(Ralph Johnson), 존 블리시디스(John Vissides)
* GoF 디자인 패턴의 종류
![](/images/design-pattern/types-of-designpattern.png)
> - 관련 POST
> - [https://gmlwjd9405.github.io/2018/07/06/design-pattern.html](https://gmlwjd9405.github.io/2018/07/06/design-pattern.html)

### 스트래티지 패턴
* **행위를 클래스로 캡슐화해** 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴
  * 같은 문제를 해결하는 여러 알고리즘이 클래스별로 캡슐화되어 있고 이들이 필요할 때 교체할 수 있도록 함으로써 동일한 문제를 다른 알고리즘으로 해결할 수 있게 하는 디자인 패턴
  * '행위(Behavioral) 패턴'의 하나
    * 아래 참고
* 즉, **전략을 쉽게 바꿀 수 있도록** 해주는 디자인 패턴이다.
  * 전략이란
    * 어떤 목적을 달성하기 위해 일을 수행하는 방식, 비즈니스 규칙, 문제를 해결하는 알고리즘 등
* 특히 게임 프로그래밍에서 게임 캐릭터가 자신이 처한 상황에 따라 공격이나 행동하는 방식을 바꾸고 싶을 때 스트래티지 패턴은 매우 유용하다.
* ![](/images/design-pattern-strategy/strategy-pattern.png)
* 역할이 수행하는 작업
  * Strategy
    * 인터페이스나 추상 클래스로 외부에서 동일한 방식으로 알고리즘을 호출하는 방법을 명시
  * ConcreteStrategy
    * 스트래티지 패턴에서 명시한 알고리즘을 실제로 구현한 클래스
  * Context
    * 스트래티지 패턴을 이용하는 역할을 수행한다.
    * 필요에 따라 동적으로 구체적인 전략을 바꿀 수 있도록 setter 메서드('집약 관계')를 제공한다.
> - 관련 POST
> - [https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)


### 싱글턴 패턴
* 전역 변수를 사용하지 않고 **객체를 하나만 생성** 하도록 하며, 생성된 객체를 **어디에서든지 참조할 수 있도록** 하는 패턴
  * '생성(Creational) 패턴'의 하나 (*아래 참고*)
* ![](/images/design-pattern-singleton/singleton-example.png){: width="150" height="100"}
* 역할이 수행하는 작업
  * Singleton
    * 하나의 인스턴스만을 생성하는 책임이 있으며 getInstance 메서드를 통해 모든 클라이언트에게 동일한 인스턴스를 반환하는 작업을 수행한다.
> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)

### 스테이트 패턴

### 커맨드 패턴

### 옵서버 패턴

### 데커레이터 패턴

### 템플릿 메서드 패턴

### 팩토리 메서드 패턴

### 추상 팩토리 패턴

### 컴퍼지트 패턴


---

## 관련된 Post
* 알고리즘: [기술 면접 예상 질문 대비하기 - 알고리즘(Algorithm)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-algorithm.html) 을 참고하시기 바랍니다.
* 데이터베이스: [기술 면접 예상 질문 대비하기 - 데이터베이스(DB)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-db.html) 을 참고하시기 바랍니다.
* 디자인패턴: [기술 면접 예상 질문 대비하기 - 디자인패턴(Design Pattern)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-designpattern.html) 을 참고하시기 바랍니다.
* Java: [기술 면접 예상 질문 대비하기 - Java편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-java.html) 을 참고하시기 바랍니다.
* 운영체제: [기술 면접 예상 질문 대비하기 - 운영체제(OS)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-os.html) 을 참고하시기 바랍니다.


## References
<!-- > - [http://hahahoho5915.tistory.com/16](http://hahahoho5915.tistory.com/16) -->
