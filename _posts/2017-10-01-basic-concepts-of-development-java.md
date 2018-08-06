---
layout: post
title: '[기술 면접 질문] 기술 면접 예상 질문 대비하기 - Java편'
subtitle: '기술 면접 예상 질문 대비하기 - Java편'
date: 2017-10-01
author: heejeong Kwon
cover: '/images/basic-concepts-of-development/basic-concepts-of-development-main.png'
tags: 면접 기본개념 java
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## <mark>Java편</mark>  
<span style="color:#4d0000">java의 문법과 특징 및 기본적인 개념을 이해하고 기술 면접에 대비하자!</span>  

<br> <span style="background-color: #e1e1e1">계속해서 추가할 예정입니다!<span>

### java 프로그래밍 이란
* java 프로그래밍 이란
* java와 c언어의 차이
* java언어의 장단점

### OOP의 4가지 특징
1. 추상화
* 구체적인 사물들의 공통적인 특징을 파악해서 이를 하나의 개념(집합)으로 다루는 것
2. 캡슐화
* 정보 은닉(information hiding): 필요가 없는 정보는 외부에서 접근하지 못하도록 제한하는 것
3. 일반화 관계
* 여러 개체들이 가진 공통된 특성을 부각시켜 하나의 개념이나 법칙으로 성립시키는 과정
4. 다형성
* 서로 다른 클래스의 객체가 같은 메시지를 받았을 때 각자의 방식으로 동작하는 능력

> - 관련 POST  
> - [https://gmlwjd9405.github.io/2018/07/05/oop-features.html](https://gmlwjd9405.github.io/2018/07/05/oop-features.html)

### OOP의 5대 원칙 (SOLID)
* **S**: 단일 책임 원칙(SRP, Single Responsibility Principle)
  * 객체는 단 하나의 책임만 가져야 한다.
* **O**: 개방-폐쇄 원칙(OCP, Open Closed Principle)
  * 기존의 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계가 되어야 한다.
* **L**: 리스코프 치환 원칙(LSP, Liskov Substitution Principle)
  * 일반화 관계에 대한 이야기며, 자식 클래스는 최소한 자신의 부모 클래스에서 가능한 행위는 수행할 수 있어야 한다.
* **I**: 의존 역전 원칙(DIP, Dependency Inversion Principle)
  * 의존 관계를 맺을 때 변화하기 쉬운 것 또는 자주 변화하는 것보다는 변화하기 어려운 것, 거의 변화가 없는 것에 의존하라는 것이다.
* **D**: 인터페이스 분리 원칙(ISP, Interface Segregation Principle)
  * 인터페이스를 클라이언트에 특화되도록 분리시키라는 설계 원칙이다.

> - 관련 POST
> -  [https://gmlwjd9405.github.io/2018/07/05/oop-solid.html](https://gmlwjd9405.github.io/2018/07/05/oop-solid.html)


### java의 접근 제어자의 종류와 특징
* ![](/images/class-diagram/access-controller.png)

### 객체지향 프로그래밍과 절차지향 프로그래밍의 차이
*

### java의 static
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

> - 관련 POST
> - [https://gmlwjd9405.github.io/2018/08/04/java-static.html](https://gmlwjd9405.github.io/2018/08/04/java-static.html)

### java의 final 키워드
final 키워드
* 개념: 변수나 메서드 또는 클래스가 '변경 불가능'하도록 만든다.
* 원시(Primitive) 변수에 적용 시
  * 해당 변수의 값은 변경이 불가능하다.
* 참조(Reference) 변수에 적용 시
  * 참조 변수가 힙(heap) 내의 다른 객체를 가리키도록 변경할 수 없다.
* 메서드에 적용 시
  * 해당 메서드를 오버라이드할 수 없다.
* 클래스에 적용 시
  * 해당 클래스의 하위 클래스를 정의할 수 없다.

> - 관련 POST
> - [https://gmlwjd9405.github.io/2018/08/06/java-final.html](https://gmlwjd9405.github.io/2018/08/06/java-final.html)

### java의 제네릭(Generic)

### java의 가비지 컬렉션(Garbage Collection)

### 객체(Object)란 무엇인가

### 객체 직렬화(Serialization)와 역직렬화(Deserialization)란 무엇인가

### 클래스와 인스턴스의 차이(Class vs Instance)

### 오버로딩과 오버라이딩의 차이(Overloading vs Overriding)

### Call by Reference와 Call by Value의 차이

### 인터페이스와 추상 클래스의 차이(Interface vs Abstract Class)

### 프로세스와 스레드의 차이(Process vs Thread)

### 세션과 쿠키의 차이(Session vs Cookie)

### 동기화 객체의 종류
* 뮤텍스와 세마포어의 차이

### 동기화와 비동기화의 차이(Syncronous vs Asyncronous)



---

## 관련된 Post
* 알고리즘: [기술 면접 예상 질문 대비하기 - 알고리즘(Algorithm)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-algorithm.html) 을 참고하시기 바랍니다.
* 데이터베이스: [기술 면접 예상 질문 대비하기 - 데이터베이스(DB)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-db.html) 을 참고하시기 바랍니다.
* 디자인패턴: [기술 면접 예상 질문 대비하기 - 디자인패턴(Design Pattern)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-designpattern.html) 을 참고하시기 바랍니다.
* Java: [기술 면접 예상 질문 대비하기 - Java편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-java.html) 을 참고하시기 바랍니다.
* 운영체제: [기술 면접 예상 질문 대비하기 - 운영체제(OS)편](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-os.html) 을 참고하시기 바랍니다.


## References
<!-- > - [http://hahahoho5915.tistory.com/16](http://hahahoho5915.tistory.com/16)
> - [http://cheekee.co.kr/?p=273](http://cheekee.co.kr/?p=273)
> - [http://manducku.tistory.com/44](http://manducku.tistory.com/44)
> - [https://www.slideshare.net/HaYouri/hau-java](https://www.slideshare.net/HaYouri/hau-java) -->
