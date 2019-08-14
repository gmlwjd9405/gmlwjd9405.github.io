---
layout: post
title: '[JPA] 단방향 연관관계란'
subtitle: '단방향 연관관계를 이해하고 기본적인 객체지향 모델링 방법을 익힌다.'
date: 2019-08-13
author: heejeong Kwon
cover: '/images/inflearn-jpa/jpa-main2.png'
tags: inflearn jpa entity-mapping one-way-association 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[인프런 강의](https://www.inflearn.com/course/ORM-JPA-Basic#) 참고

# 단방향 연관관계

## Goal
> - 객체와 테이블 연관관계의 차이를 이해한다. 
> - 객체의 참조와 테이블의 외래 키를 어떻게 매핑하는지 안다. 
> - 용어 이해
>   - 방향 (Direction)
>     - 단방향, 양방향
>   - 다중성 (Multiplicity)
>     - 다대일 (N:1), 일대다 (1:N), 일대일 (1:1), 다대다 (N:N) 
>   - 연관관계의 주인 (Owner)
>     - 객체 양방향 연관관계는 관리하는 주인이 필요하다.


## 연관관계가 필요한 이유
- '객체지향 설계의 목표는 자율적인 객체들의 협력 공동체를 만드는 것이다' - 조영호(객체지향의 사실과 오해)
- 객체를 테이블에 맞추어 모델링 -> 객체 지향 모델링 (객체 연관관계 사용)

### 예제 시나리오
- 회원과 팀이 있다.
- 회원은 하나의 팀에만 소속될 수 있다.
- 회원과 팀은 다대일 관계다.

### 1. 객체를 테이블에 맞추어 모델링 (연관 관계가 없는 객체)
![그림 추가]()

- N:1의 연관관계를 가지는 테이블
  - Member ---N:1--- Team
- 연관관계가 없는 객체
  - 참조 대신 외래 키(teamId)를 그대로 사용한다.

```java
@Entity
public class Member {
  @Id
  @GeneratedValue
  private Long id;
  
  @Column(name = "USERNAME")
  private String username;
  
  private int age;
  
  @Column(name = "TEAM_ID")
  private Long teamId;
  ...
}
```
```java
@Entity
public class Team {
  @Id
  @GeneratedValue
  private Long id;
  
  private String name;
  ...
}
```

#### Q. 객체를 테이블에 맞추어 모델링 했을 때의 문제점?
- **저장**의 경우,
  ```java
  // 팀 저장
  Team team = new Team();
  team.setName("TeamA");
  entityManager.persist(team);	// PK값이 세팅된 상태 (영속 상태)
  // 회원 저장
  Member member = new Memeber();
  member.setName("member1");
  member.setTeamId(team.getId());	// 외래키 식별자를 직접 다룸  
  entityManager.persist(member);
  ```
  - **A.** 외래 키의 식별자를 직접 다뤄야 하는 문제가 있다.
    - Team 객체를 영속화한 후 Member의 teamId값(FK)에 직접 값을 세팅해줘야 한다.
    - `member.setTeamId(team.getId();`은 위에서 team이 영속 상태 이므로 getId()해서 가져올 수 있지만, 객체 지향적인 방법이 아니다.
    - `member.setTeam(team);`과 같이 사용하고 싶다는 생각이 들 것이다.

- **조회**의 경우,
  ```java
  // 조회
  Member findMember = entityManager.find(Member.class, member.getId());
  // 연관 관계가 없음 (못가져옴)
  Team findTeam = entityManger.find(Team.class, team.getId());
  ```
  ```java
  // 먼저 식별자를 가져와야 함 (가져옴)
  Long findTeamId = findMember = findMember.getTeamId();
  Team findTeam = entityManger.find(Team.class, team.getId());
  ```
  - **A.** 연관 관계가 없기 때문에 식별자를 통해 다시 팀을 조회해야 한다.
  - 즉, 조회를 두 번 해야 한다.
  - 객체 지향적인 방법은 아니다.

- **결론**
  - 객체를 테이블에 맞추어 데이터 중심으로 모델링하면 *협력 관계를 만들 수 없다.*
    - 테이블: 외래 키로 조인을 사용해서 연관된 테이블을 찾는다.
    - 객체: 참조를 사용해서 연관된 객체를 찾는다.
    - 테이블과 객체 사이에는 이런 큰 간격이 존재한다.


## 단방향 연관관계 매핑 이론
### 2. 객체 지향 모델링 (객체 연관관계 사용)
![그림 추가]()

- N:1의 연관관계를 가지는 테이블
  - Member ---N:1--- Team
- 객체 연관관계 사용 
  - 객체의 참조와 테이블의 외래 키를 매핑한다.

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
  
//  @Column(name = "TEAM_ID")
//  private Long teamId;
  
  @ManyToOne
  @JoinColumn(name = "TEAM_ID") // 매핑 
  private Team team;
  ...
}
```

1. 외래 키 대신에 Team 객체를 넣고 TEAM_ID를 매핑한다. 
2. 조인할 컬럼을 명시한다.
  - @JoinColumn으로 조인 컬럼을 명시한다. 
  - 안적어도 default로 들어가지만, 적어 주는 것이 좋다.
3. 연관관계를 맺는다.
  - Member ---N:1--- Team
  - Member 입장에서는 다대일, Team 입장에서는 일대다의 관계를 갖는다.
  - @ManyToOne으로 연관관계를 설정한다.
  - Team 이라는 필드가 DB의 "TEAM_ID"라는 외래 키와 매핑된다.

#### ORM 매핑 
![그림 추가]()
- 객체의 참조와 테이블의 외래 키를 매핑하여 단방향 연관관계를 설정한다.

- **저장**의 경우 
 ```java
  // 팀 저장
  Team team = new Team();
  team.setName("TeamA");
  entityManager.persist(team);
  // 회원 저장
  Member member = new Memeber();
  member.setName("member1");
  member.setTeam(team);    // 단방향 연관관계 설정, 참조 저장
  entityManager.persist(member);

  Member findMember = entityManager.find(Member.class,  member.getId());
  ```
  - Team 객체를 그대로 넣는다.
- **조회**의 경우
  ```java
  // 조회
  Member findMember = em.find(Member.class, member.getId());
  // 참조를 사용해서 연관관계 조회
  Team findTeam = findMember.getTeam();
  ```
  ```sql
  select
    member.MEMBER_ID
    member.TEAM_ID
    member.USERNAME
    team.TEAM_ID
    team.name
  from
    Member member
  left outer join
    Team team on member.TEAM_ID=team.TEAM_ID
  where
    member.MEMBER_ID=?
  ```
  - Member의 getTeam()을 통해 연관된 Team을 바로 조회할 수 있다.
    - 참조로 연관관계를 조회할 수 있다. 
    <!-- - 객체 그래프 탐색!! -->
  - 위의 SELECT Query문을 보면 
    - JPA가 조인을 통해 Member와 Team을 한 번에 가지고 오는 것을 확인할 수 있다.
    - 하지만, 처음부터 조인해서 가져오지 않고 일단 멤버만 가져오고 싶다라고 하면

#### FetchType 옵션 
- **@ManyToOne(fetch = FetchType.LAZY)**
  - 지연 로딩 전략 
  - 실제 연관관계를 가진 팀이 Touch 될 때, 그 때 Team을 조회하는 쿼리를 날려서 가져온다.
- **@ManyToOne(fetch = FetchType.EAGER)**
  - 기본값
  - 연관관계를 가진 객체를 조인해서 먼저 모두 가져온다.
  <!-- - 현업에서는 전부 다 LAZY로 기본 설정 해놓고, 꼭 필요한 경우에만 쿼리를 실제 날리는 시점에 원하는 결과를 최적화해서 가져오는 방법을 사용하는 것을 권장한다. -->


**[참고]** 영속성 1차 캐시에서 가져오지 않고 DB Query를 날려서 직접 확인하고 싶으면,
```java
entityManager(member);

// 추가
entityManager.flush(); // 현재 영속성 컨텍스트에 있는 것을 DB에 던져서 sync를 맞춘다.
entityManager.clear(); // 영속성 컨텍스트를 초기화시킨다.

Member findMember = entityManager.find(Member.class, member.getId()); // find 시 날라가는 쿼리를 확인한다.
```
- `flush()`와 `clear()`가 없으면 아래의 `find()`에서는 SELECT Query는 날라가지 않는다.
- 영속성 컨텍스트의 1차 캐시에 쿼당 객체를 저장하기 때문에 쿼리를 날리지 않고 1차 캐시에서 객체 정보를 가져오기 때문이다.


### 연관관계 수정
```java
// 새로운 팀B
Team teamB = new Team();
teamB.setName("TeamB");
entityManager.persist(teamB);

// 회원1에 새로운 팀B 설정
member.setTeam(teamB);
```
- setTeam()을 통해 기존의 팀을 새로운 팀으로 변경하면, 연관관계가 바뀐다.
- UPDATE Query를 통해서 FK 가 업데이트된다.


---

# 관련된 Post
* JDBC, JPA/Hibernate, Mybatis의 차이에 대해 알고 싶으시면 [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)를 참고하시기 바랍니다.
* ORM의 개념에 대해 알고 싶으시면 [ORM이란](https://gmlwjd9405.github.io/2019/02/01/orm.html)을 참고하시기 바랍니다.


# Reference
> - [인프런 강의 참고](https://www.inflearn.com/course/ORM-JPA-Basic#)
