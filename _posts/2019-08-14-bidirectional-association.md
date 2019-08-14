---
layout: post
title: '[JPA] 양방향 연관관계란'
subtitle: '양방향 연관관계 및 연관관계의 주인에 대해 이해한다.'
date: 2019-08-14
author: heejeong Kwon
cover: '/images/inflearn-jpa/jpa-main2.png'
tags: inflearn jpa entity-mapping bidirectional-association 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[인프런 강의](https://www.inflearn.com/course/ORM-JPA-Basic#) 참고

# 양방향 연관관계와 연관관계의 주인
<!-- ## Goal
> - -->

## 양방향 연관관계 
- 연관관계를 찾는 방법
  - 객체는 참조, DB는 외래 키를 이용한 JOIN
  - 이 둘 간의 차이를 이해해야 한다.

![그림 추가]()
- 테이블 연관관계는 단방향 연관관계에서와 동일하다.
  - Why?
    - Member에서 Team을 알고 싶으면 TEAM_ID인 FK로 JOIN을 하면 된다.
    - Team에서 Member를 알고 싶으면 마찬가지로 나의 PK로 JOIN을 하면 된다.
    - 즉, 테이블의 연관관계는 사실상 방향이 없다. (FK로 서로를 알 수 있기 때문)
- 객체에서의 연관관계를 위해서는 Team에서도 `List members` 가 필요하다.
  - 즉, 객체 설계에서 양방향을 구현하고 싶으면 Member에서는 Team을 가지고 있고, Team에서는 List Members를 가지고 있도록 설계하면 된다. 
  - 이렇게 하면 Team에서도 getMemberList()로 특정 팀에 속한 멤버 리스트를 가져올 수 있다.

### 예제 
```java
@Entity
public class Member {
  @Id
  @GeneratedValue
  @Column(name = "MEMBER_ID")
  private Long id;
  
  @Column(name = "USERNAME")
  private String username;
  
  private int age;

  @ManyToOne // N:1
  @JoinColumn(name = "TEAM_ID")
  private Team team;
  ...
}
```
```java
@Entity
public class Team {
  @Id
  @GeneratedValue
  @Column(name = "TEAM_ID")
  private Long id;
  
  private String name;
  
  @OneToMany(mappedBy = "team") // 1:N
  private List<Member> members = new ArrayList<Member>(); // 초기화 (관례) - NullPointerException 방지 
  ...
}
```

- Member ---N:1--- Team
  - Member 입장에서는 다대일, Team 입장에서는 일대다의 관계를 갖는다.
  - **Member 엔티티**
    - 단방향과 동일하다.
    - `@ManyToOne`
  - **Team 엔티티**
    - Member 컬렉션을 추가한다.
    - `@OneToMany(mappedBy = "team")`
    - mappedBy로 team과 연관이 있는 것을 알려준다.
- 반대 방향으로도 객체 그래프를 탐색할 수 있다.
  ```java
  // Member 조회
  Member findMember = entityManager.find(Member.class, member.getId());
  // team에서 멤버들 조회
  List<Member> members = findMember.getTeam().getMembers();
  ```

## 연관관계 주인과 mappedBy
- mappedBy
  - JPA의 멘탈붕괴 난이도
  - 처음에는 이해하기 어렵지만, **객체와 테이블간에 연관관계를 맺는 차이**를 이해하면 쉬워진다.
- 객체는 방향을 가지고 있고, DB는 방향성이 없다. 
  - DB 테이블에서는 FK 하나로 양방향 관계(서로 JOIN)를 맺을 수 있다.

### 객체와 테이블간에 연관관계를 맺는 차이
#### 객체 연관관계 = 2개 
- 회원 -> 팀 연관관계 1개 (단방향)
- 팀 -> 회원 연관관계 1개 (단방향)
- **객체의 양방향 관계**
  - 객체의 양방향 관계는 사실 양방향 관계가 아니라 서로 다른 단방향 관계 2개이다.
  - 객체를 양방향으로 참조하려면 단방향 연관관계를 2개 만들어야 한다.
    - A -> B(a.getB())
    ```java
    class A { B b; }
    ```
    - B -> A(b.getA()) 
    ```java
    class B { A a; }
    ```

#### 테이블 연관관계 = 1개
- 회원 <-> 팀의 연관관계 1개 (양방향, 사실 방향이 없음)
- **테이블의 양방향 연관관계**
  - 테이블은 외래 키 하나로 두 테이블의 연관관계를 관리한다.
  - MEMBER.TEAM_ID 외래 키(FK) 하나로 양방향 연관관계를 가진다. (양쪽에서 서로 JOIN 가능)
  ```sql
  SELECT *
    FROM MEMBER M
    JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID
  ```
  ```sql
  SELECT *
    FROM TEAM T
    JOIN MEMBER M ON T.TEAM_ID = M.TEAM_ID
  ```

### 둘 중 하나로 외래 키를 관리해야 한다
![그림 추가]()

- 객체에서의 양방향 관계
  - Member도 Team을 가지고, Team도 Member를 가지면 이제 둘 중에 뭘로 연관관계를 매핑해야 할까?
  - **Q.** 
    - Member의 team 값을 업데이트 했을 때, Member의 TEAM_ID(FK)를 업데이트?
    - 아니면 Team의 members 값을 업데이트 했을 때, Member의 TEAM_ID(FK)를 업데이트?
  - **Q.** 
    - 팀에 멤버를 바꾸고 싶거나 새로운 팀으로 들어가고 싶을 때,
    - Member의 team 값을 업데이트? 아니면 Team의 members 값을 업데이트?
  - **A.**
    - DB에서는 객체가 참조를 어떻게 하든 외래 키값(Member의 TEAM_ID)만 업데이트하면 된다.
    - 객체에서는 
      - 단방향 연관관계 매핑: 참조와 외래 키만 업데이트 하면 된다.
      - 양방향 연관관계 매핑: 외래 키를 관리하는 명확한 하나의 관계가 필요하다.
    - 양방향 연관관계에서의 외래 키 관리 주인의 기준을 명확하게 하기 위해 **연관관계의 주인**이라는 개념이 나왔다.

### 연관관계의 주인(Owner)
#### 양방향 매핑 규칙
- 객체의 두 관계 중 하나를 연관관계의 주인으로 지정한다.
- 연관관계의 주인만이 외래 키를 관리한다. 
  - 진짜 매핑 
  - 주인만이 DB에 접근하여 값을 등록, 수정, 변경할 수 있다. 
  - 주인이 아닌 객체가 값을 변경하더라도 DB에는 영향이 없다.
- **주인이 아닌쪽은 읽기(SELECT)만 가능**하다.
  - 가짜 매핑 

#### 누구를 주인으로 해야 할까?
<!-- 색 추가 -->
- **외래 키가 있는 곳을 주인으로 정해라.**
  - DB에서는 N에 해당하는 테이블이 FK를 가지고 있다. (N쪽이 연관관계의 주인)
  - 즉, @JoinColumn이 있는 쪽이 주인이다.

![그림 추가]()

- N:1 의 관계에서 N에 해당하는 쪽(FK를 가짐)이 연관관계의 주인이 된다.
  - 여기서는 `Member.team`이 연관관계의 주인이다.
- 연관관계의 주인은 비즈니스 적으로 중요하지 않다. 
  - 단지 FK를 가진 N쪽이 주인이 된다.
  - Ex. [자동차 ---1:N--- 바퀴] 에서는 바퀴가 연관관계의 주인이다.
- 이렇게 해야 성능 이슈도 없고 설계도 깔끔한 구조가 된다. 
  - 깔끔한 구조
    - 엔티티 하나에서 관리가 된다. 
    - 즉, FK가 있는 엔티티에서 연관관계도 관리할 수 있다.
    - Ex. Member의 team을 바꿨더니 Member의 UPDATE Query가 날라간다.
- **Q.** `Team.members`를 주인으로 선택하고 Team의 List members를 변경했다. 그 결과는?
  - **A.** 다른 테이블인 Member의 UPDATE Query가 날라간다.

#### 주인인 객체와 주인이 아닌 객체의 구현 방법 
- Member ---N:1--- Team
  - Member 입장에서는 다대일, Team 입장에서는 일대다의 관계를 갖는다.
  - Member가 Owner가 된다.
- **주인인 객체**
  - mappedBy 속성을 사용하지 않는다.
  - `@ManyToOne`, `@JoinColumn` 필요 
  - mappedBy의 뜻? 
    - 어떤 대상에 의해서 매핑이 되었다.
- **주인이 아닌 객체**
  - mappedBy 속성으로 주인을 지정한다.
  - `@OneToMany` 필요 

<!-- 
    - 엔티티랑 테이블이 매핑이 되어있는 테이블에서 FK가 관리가 된다.
- 모 민족에서 프로젝트 중에, 모두 단방향으로 설계하고 개발했다. 양방향으로 뚫어서 조회가 필요한 경우에 그 때 양방향 매핑 하면 된다. 자바 코드만 수정이 일어나기 때문에 DB에 영향 주지 않는다.
  - 이미, 단방향 매핑만으로 ORM 매핑이 다 끝났다. 양방향은 단순히 조회를 편하게 하기 위해 부가 기능이 조금더 들어가는 거라고 보면 된다. 어차피 조회할 수 있는 기능만 있다.
 -->


---

# 관련된 Post
* JDBC, JPA/Hibernate, Mybatis의 차이에 대해 알고 싶으시면 [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)를 참고하시기 바랍니다.
* ORM의 개념에 대해 알고 싶으시면 [ORM이란](https://gmlwjd9405.github.io/2019/02/01/orm.html)을 참고하시기 바랍니다.


# Reference
> - [인프런 강의 참고](https://www.inflearn.com/course/ORM-JPA-Basic#)
