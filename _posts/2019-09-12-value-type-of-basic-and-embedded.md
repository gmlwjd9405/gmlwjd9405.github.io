---
layout: post
title: '[JPA] 값 타입(1) - 기본값 타입, 임베디드 타입'
subtitle: '값 타입 중 기본값 타입과 임베디드 타입에 대해 이해한다.'
date: 2019-09-12
author: heejeong Kwon
cover: '/images/inflearn-jpa/jpa-main2.png'
tags: inflearn jpa value-type 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[인프런 강의](https://www.inflearn.com/course/ORM-JPA-Basic#) 참고

## Goal
> 1. 기본값 타입
> 2. **임베디드 타입 (복합 값 타입)**
> - 값 타입과 불변 객체
> - 값 타입의 비교
> 3. **값 타입 컬렉션**

# JPA의 최상위 데이터 타입 분류 (2가지)
## 1. 엔티티 타입
- @Entity로 정의하는 객체
- 데이터가 변해도 식별자로 지속해서 추적이 가능하다.
- e.g. 회원 엔티티의 키나 나이 값을 변경해도 식별자로 인식이 가능하다.

## 2. 값 타입
- int, Integer, String 처럼 단순히 값으로 사용하는 자바 기본 타입이나 객체
- 식별자가 없고 값만 있으므로 변경시 추적이 불가능하다.
- e.g. 숫자 100을 200으로 변경하면 완전히 다른 값으로 대체된다.

# 구체적인 데이터 타입 분류 (3가지)
1. 기본값 타입
   - 자바 기본 타입 (int, double)
   - 래퍼 클래스 (Integer, Long)
   - String
2. 임베디드 타입 (embedded type, 복합 값 타입)
    - JPA에서 정의해서 사용해야 한다.
    - e.g. 좌표의 경우, Position Class
3. 컬렉션 값 타입 (collection value type)
    - 마찬가지로 JPA에서 정의해서 사용해야 한다.
    - 컬렉션에 기본값 또는 임베디드 타입을 넣은 형태이다.

---

## 1. 기본값 타입
### 종류
- 자바 기본 타입 (int, double)
- 래퍼 클래스 (Integer, Long)
- String

### 특징
- 생명 주기를 엔티티에 의존한다.
  - e.g. 회원을 삭제하면 이름, 나이 필드도 함께 삭제된다.
- 값 타입은 공유하면 안된다.
  - **Why?** Side Effect가 발생할 수 있기 때문에 
  - e.g. 회원 이름 변경 시 다른 회원의 이름도 함께 변경되면 안된다. 

### 참고
- 자바의 기본 타입은 절대 공유되지 않는다.
    ```java
    int a = 20;
    int b = a; // 20이라는 값을 복사
    b = 10; 
    ```
    - int, double 같은 기본 타입(primitive type)은 절대 공유되지 않는다.
    - 기본 타입은 공유되지 않고 항상 값을 복사한다.
    - 즉, Side Effect가 발생하지 않는다.
- Integer 같은 래퍼 클래스나 String과 같은 특수한 클래스는 공유 가능한 객체이지만 변경은 불가능하다.
    ```java
    Integer a = new Integer(10);
    Integer b = a; // a의 참조를 복사 
    a.setValue(20); // Side Effect!! 
    ```
    - 10이 복사되는 것이 아니라 a의 레퍼런스가 넘어가서 같은 인스턴스를 공유한다.
    - `a.setValue(20)` 에서 b값도 20으로 변경되는 Side Effect가 발생할 수 있다.
    - 그래서! Java에서는 변경 자체를 불가능(Immutable)하게 만들어서 Side Effect를 막았다.

---

## 2. 임베디드 타입 (embedded type, 복합 값 타입)
- 새로운 값 타입을 직접 정의할 수 있다.
- JPA는 **임베디드 타입**(embedded type, 내장 타입)이라고 한다.
- 주로 기본 값 타입을 모아서 만들기 때문에 **복합 값 타입**이라고도 한다.

### 예시 
- 회원 엔티티는 이름, 근무 시작일, 근무 종료일, 주소 도시, 주소 번지, 주소 우편번호를 가진다.
    <!-- - ![그림 추가]() -->
    - startDate, endDate / city, street, zipcode 는 공통 필드로 사용 가능 
- 회원 엔티티는 이름, 근무기간, 집주소 가진다.
    <!-- - ![그림 추가]() -->
    - 실 생활에서는 위와 같이 추상화해서 쉽게 표현한다.
    - 즉, 위와 같이 묶을 수 있는 것이 임베디드 타입이다.
- 임베디드 타입으로 구현한 형태
    <!-- - ![그림 추가]() -->
    - 클래스를 새로 뽑는다.

### 특징 및 장점 
- 재사용이 가능하다.
- 클래스 내에서의 응집도가 높다.
- `Period.isWork()` 처럼 해당 값 타입만 사용하는 의미 있는 메소드를 만들 수 있다.
    - *객체지향적인 설계가 가능하다.*
- 임베디드 타입도 int, String과 같은 값 타입이다. **(중요!)**
    - 엔티티가 아니다. 엔티티의 값일 뿐이다.
    - 추적이 되지 않기 때문에 변경하면 끝난다.
    - 임베디드 타입을 포함한 모든 값 타입은 값 타입을 소유한 *엔티티에 생명주기를 의존한다.*
- 임베디드 타입의 값이 null 이라면 매핑한 컬럼 값은 모두 null로 저장된다.

### 기본 사용법
- *기본 생성자*는 필수이다.
- `@Embeddable`
  - 값 타입을 정의 하는 곳에 표시한다.
- `@Embedded`
  - 값 타입을 사용하는 곳에 표시한다.
- c.f. `@Embeddable`, `@Embedded` 중 하나만 명시하면 되지만, 둘 다 적는 것을 권장한다.

### 임베디드 타입과 테이블 매핑
<!-- - ![그림 추가]() -->
- 값 타입 / 임베디드 타입에 상관없이 **DB** 회원 테이블의 형태는 동일하다. 
- 하지만 **객체**의 경우는 데이터 뿐만 아니라 그에 맞는 기능(메서드)를 가지고 있기 때문에 공통으로 묶은 임베디드 타입으로 구성하면 이점이 있다. 
```java
@Entity
public class Member {
    ...
    @Embedded
    private Address homeAddress;
    @Embedded
    private Address workAddress;
    ...
}
```
```java
@Embeddable
@NoArgsConstructor
public class Address {
    private String city;
    private String street;
    private String zipcode;
    ...
}
@Embeddable
@NoArgsConstructor
public class Period {
    private LocalDateTime startDate;
    private LocalDateTime endDate;
    ...
}
```
- 이점?
    - 임베디드 타입을 사용하기 전과 후에 **매핑하는 테이블은 같다.** (중요!)
        - 임베디드 타입은 엔티티의 값일 뿐이다.
    - 객체와 테이블을 아주 세밀하게(find-grained) 매핑하는 것이 가능하다.
    - 잘 설계한 ORM 애플리케이션은 매핑한 테이블의 수보다 클래스의 수가 더 많다.
    - 용어/코드를 공통으로 관리할 수 있게 된다.

### 임베디드 타입과 연관관계
<!-- - ![그림 추가]() -->
- 임베디드 타입은 임베디드 타입을 가질 수 있다.
    - e.g. Address <<Value>> 임베디드 타입은 Zipcode <<Value>> 라는 임베디드 타입을 가진다. 
- 임베디드 타입은 엔티티 타입을 가질 수 있다. 
    - e.g. PhoneNumber <<Value>> 임베디드 타입이 PhoneEntity <<Entity>> 를 가질 수 있다. 
    - FK만 가지면 되기 때문이다.
- **Q.** 한 Entity 안에서 같은 값 타입을 2개 이상 가지면 어떻게 될까? 
    ```java
    @Entity
    public class Member {
    ...
    @Embedded
    private Address homeAddress; // 주소 
    @Embedded
    private Address workAddress; // 주소 
    ...
    }
    ```
    - 컬럼 명이 중복된다.
        - `MappingException: Repeated column` Error

- **A.** `@AttributeOverrides`, `@AttributeOverride`를 통해 속성을 재정의한다.
  ```java
  @Entity
  public class Member {
    ...
    @Embedded
    private Address homeAddress;
    @Embedded
    @AttributeOverrides({ // 새로운 컬럼에 저장 (컬럼명 속성 재정의)
      @AttributeOverride(name="city", column=@Column(name = "WORK_CITY"),
      @AttributeOverride(name="street", column=@Column(name = "WORK_STREET"),
      @AttributeOverride(name="zipcode", column=@Column(name = "WORK_ZIPCODE")})
    private Address workAddress;
    ...
  }
  ```

---

## 값 타입과 불변 객체
- 값 타입은 복잡한 객체 세상을 조금이라도 단순화하려고 만든 개념이다. 
- 따라서 값 타입은 단순하고 안전하게 다룰 수 있어야 한다.

### 값 타입 공유 참조의 부작용 
- 임베디드 타입 같은 값 타입을 여러 엔티티에서 공유하면 굉장히 위험하다.
    - **Side Effect(부작용)**이 발생한다.
- 예시 
    - ![그림 추가]()
    - Member Class가 같은 Address 객체를 가지고 있고, 두 Member 객체 중 첫 번째 Member의 Address(city) 속성만 변경하려고 하는 경우 
    ```java
    Address address = new Address("city", "street", "10000");
    // 
    Member member = new Member();
    member.setUsername("member1");
    member.setHomeAddress(address);
    em.persist(member);
    // 
    Member member2 = new Member();
    member2.setUsername("member2");
    member2.setHomeAddress(address);
    em.persist(member2);
    // 첫 번째 member의 Address(city) 속성만 변경하고 싶다.
    member.getHomeAddress().setCity("new city");
    tx.commit(); // UPDATE Query 2번 
    ```
- 결과 
    - UPDATE Query가 두 번 날라간다. 
    - 즉, DB에서의 member1과 member2의 city 값이 모두 변경된다.
    - Side Effect와 같은 버그는 잡기가 굉장히 어렵다.
- 해결책
    - 엔티티 간에 공유하고 싶은 값 타입은 **엔티티로 만들어서 공유**해야 한다.

### 값 타입 복사
- 위의 예시처럼 값 타입의 실제 인스턴스인 값을 공유하는 것은 굉장히 위험하다.
- 대신 값(인스턴스)를 복사해서 사용한다.
    - ![그림 추가]()
    ```java
    Address address = new Address("city", "street", "10000");
    // 
    Member member = new Member();
    member.setUsername("member1");
    member.setHomeAddress(address);
    em.persist(member);
    // Address 객체 복사
    Address copyAddress = new Address(address.getCity(), address.getStreet(), address.getZipcode());
    // 
    Member member2 = new Member();
    member2.setUsername("member2");
    member2.setHomeAddress(copyAddress); // 복사한 것을 넣는다.
    em.persist(member2);
    // 첫 번째 member의 Address(city) 속성만 변경된다.!
    member.getHomeAddress().setCity("new city");
    ```

### 객체 타입의 한계
- 위의 예시처럼 항상 값을 복사해서 사용하면 공유 참조로 인해 발생하는 부작용을 피할 수 있다.
- 그러나, 임베디드 타입처럼 직접 정의한 값 타입은 자바의 기본 타입(primitive)이 아니라 객체 타입이다.
    - 자바의 **기본 타입**에 값을 대입하면 자바는 기본적으로 값을 복사한다.
    ```java
    int a = 10;
    int b = a; // 기본 타입은 값을 복사
    b = 4; 
    ```
    - **객체 타입**은 참조 값을 직접 대입하는 것을 막을 수 있는 방법이 없다. 
        ```java
        Address a = new Address("old");
        Address b = a; // 객체 타입은 참조를 전달 (같은 인스턴스를 가리킴)
        b.setCity("new"); // a, b 모두 city가 변경된다.
        ```
        - 객체 타입은 타입이 같으면 그냥 대입(=)해서 넣을 수 있다. 
        - e.g. 누군가 실수로 복사하지 않은 address를 그대로 넣었다면 컴파일 단계에서 알 수 있는 방법이 없다. (`member2.setHomeAddress(address)`)
- 따라서, **객체의 공유 참조는 피할 수 없다.**
- 해결책
    - 불변 객체 (Immutable Object) 

### 불변 객체 (Immutable Object)
- **값 타입은 불변 객체(immutable object)로 설계**해야 한다.
    - 객체 타입을 수정할 수 없게 만들면 부작용(Side Effect)을 원천 차단할 수 있다.
    - 불변이라는 작은 제약으로 부작용이라는 큰 재앙을 막을 수 있다 
- **불변 객체**란
    - 생성 시점 이후 절대 값을 변경할 수 없는 객체
    - 생성자로만 값을 설정하고 Setter를 만들지 않으면 된다.
    - c.f. Integer, String은 자바가 제공하는 대표적인 불변 객체이다.
- **Q.** 원래 값 객체의 필드를 실제로 바꾸고 싶으면? 
- **A.** Address 를 통으로 변경해야 한다.
    ```java
    Address address = new Address("city", "street", "10000");
    
    Member member = new Member();
    member.setUsername("member1");
    member.setHomeAddress(address);
    em.persist(member);
    
    Address newAddress = new Address("NewCity", address.getStreet(), address.getZipcode());
    member.setHomeAddress(newAddress); // 통으로 변경 
    
    tx.commit();
    ```

---

## 값 타입의 비교
- 깂 타입은 인스턴스가 달라도 그 안에 값이 같으면 같은 것으로 봐야 한다.
```java
int a = 10;
int b = 10;
System.out.println("a == b: " + (a == b)); // true
```
- 객체 타입은 값이 같아도 주소가 다르기 때문에 == 비교 시 false를 반환한다.
```java
Address address1 = new Address("서울시");
Address address2 = new Address("서울시");
System.out.println("address1 == address2: " + (address1 == address2)); // false
```

- **동일성(identity)** 비교
    - 인스턴스의 참조 값을 비교한다. 
    - == 사용
    - primitive 타입 
- **동등성(equivalence)** 비교
    - 인스턴스의 값을 비교한다.
    - equals() 사용
    - 임베디드 타입 
    - `System.out.println("address1 == address2: " + (address1.equals(address2)));` // 재정의 시 true 
- 값 타입은 a.equals(b)를 사용해서 동등성 비교를 해야 한다.
- 값 타입의 `equals()` 메소드를 적절하게 재정의해야 한다.
    - 자동으로 만들어주는 equals()를 이용한다.
    - (주로 모든 필드를 사용하여 재정의 한다.)
    - equals()에 맞게 `hashCode()`도 재정의 해줘야 한다.


> [컬렉션 타입 참고 POST](https://gmlwjd9405.github.io/2019/09/13/value-type-of-collection-copy.html)


# 관련된 Post
* JDBC, JPA/Hibernate, Mybatis의 차이에 대해 알고 싶으시면 [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)를 참고하시기 바랍니다.
* ORM의 개념에 대해 알고 싶으시면 [ORM이란](https://gmlwjd9405.github.io/2019/02/01/orm.html)을 참고하시기 바랍니다.


# Reference
> - [인프런 강의 참고](https://www.inflearn.com/course/ORM-JPA-Basic#)
