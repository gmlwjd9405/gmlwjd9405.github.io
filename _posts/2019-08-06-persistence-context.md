---
layout: post
title: '[JPA] 영속성 컨텍스트(Persistence Context)란'
subtitle: '영속성 컨텍스트의 개념과 이점'
date: 2019-08-06
author: heejeong Kwon
cover: '/images/inflearn-jpa/jpa-main2.png'
tags: inflearn jpa persistence-context
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[인프런 강의](https://www.inflearn.com/course/ORM-JPA-Basic#) 참고

## 영속성 관리
- JPA 에서 가장 중요한 2가지
    - 객체와 관계형 DB 매핑하기 (Object Relational Mapping) - *설계 관련*
    - 영속성 컨텍스트 - *JPA 내부 동작*

### EntityManagerFactory와 EntityManager
- *EntityManagerFactory*는 고객의 요청이 올 때마다 (thread가 하나 생성될 때마다) EntityManager를 생성한다.
- *EntityManager*는 내부적으로 DB connection pool을 사용해서 DB에 접근한다. 
- ![사진 추가]()

- **EntityManagerFactory**
    - JPA 는 EntityManagerFactory 를 만들어야 한다.
    - application loading 시점에 DB 당 딱 하나만 생성되어야 한다.
    `EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("hello");`
    - `entityManagerFactory.close();`
        - WAS 가 종료되는 시점에 EntityManagerFactory 를 닫는다.
        - 그래야 내부적으로 Connection pooling 에 대한 Resource 가 Release 된다.
- **EntityManager**
    - 실제 Transaction 단위를 수행할 때마다 생성한다.
    - 즉, 고객의 요청이 올 때마다 사용했다가 닫는다.
    - thread 간에 공유하면 안된다. (사용하고 버려야 한다.)
    - `entityManager.close();`
        - Transaction 수행 후에는 반드시 EntityManager 를 닫는다.
        - 그래야 내부적으로 DB Connection 을 반환한다.
- **EntityTransaction**
    - Data 를 "변경"하는 모든 작업은 반드시 Transaction 안에서 이루어져야 한다.
        - 단순한 조회의 경우는 상관없음.
    - `EntityTransaction tx = entityManager.getTransaction();`
    - tx.begin(); : Transaction 시작
    - tx.commit(); : Transaction 수행
    - tx.rollback(); : 작업에 문제가 생겼을 시

### 영속성 컨텍스트(Persistence Context)란 
- JPA를 이해하는데 가장 중요한 용어이다.
    - 논리적인 개념으로, 눈에 보이지 않는다.
    - <span style="background-color: #e1e1e1">"**Entity를 영구 저장하는 환경**"</span>이라는 뜻
- `EntityManager.persist(entity);` 의 동작 설명 
  - 실제로는 DB에 저장하는 것이 아니라 영속성 컨텍스트를 통해서 **Entity를 영속화한다**는 뜻이다.
  - 정확히 말하면 `persist()` 시점에는 **Entity를 영속성 컨텍스트에 저장하는 것**이다.
  <!-- - DB 저장은 이후이다. -->
- EntityManager를 통해서 영속성 컨텍스트에 접근한다.
  ![사진 추가]()
  - EntityManager가 생성되면 1:1로 영속성 컨텍스트가 생성된다. 
  <!-- - 스프링에서 EntityManager를 주입 받아서 쓰면, 같은 트랜잭션의 범위에 있는 EntityManager는 동일 영속성 컨텍스트에 접근한다. -->

### [엔티티의 생명주기 (Entity LifeCycle)](url 추가)
- **비영속(new/transient)**
  - 영속성 컨텍스트와 전혀 관계가 없는 상태
  - 객체를 생성'만' 한 상태 
  ```java
  // 객체를 생성한 상태 (비영속)
  Member member = new Member();
  member.setId("member1");
  member.setUsername("회원1");
  ```

- **영속(managed)**
  - 영속성 컨텍스트에 저장된 상태
  - Entity가 영속성 컨텍스트에 의해 관리되는 상태
  ```java
  // 객체를 생성한 상태 (비영속)
  Member member = new Member();
  member.setId("member1");
  member.setUsername("회원1");
  EntityManager entityManager = entityManagerFactory.createEntityManager();
  entityManager.getTransaction().begin();
  // 객체를 저장한 상태 (영속)
  entityManager.persist(member);
  ```
  - `EntityManager.persist(entity);`
    - 영속 상태가 된다고 바로 DB에 쿼리가 날라가지 않는다. (즉, DB 저장 X)
  - `transaction.commit();`
    - 트랜잭션의 commit 시점에 영속성 컨텍스트에 있는 정보들이 DB에 쿼리로 날라간다.

- **준영속(detached)**
  - 영속성 컨텍스트에 저장되었다가 분리된 상태
  - 영속성 컨텍스트에서 지운 상태 
  ```java
  // 회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태
  entityManager.detach(member);
  ```

- **삭제(removed)**
  - 실제 DB 삭제를 요청한 상태 
  ```java
  // 객체를 삭제한 상태
  entityManager.remove(member);
  ```

### 영속성 컨텍스트의 이점
- Applicaion 과 DB 사이의 중간 계층의 영속성 컨텍스트가 존재하는 이유
    - 버퍼링, 캐싱 등의 이점 

#### 1. 1차 캐시
![그림 추가]()

- 영속성 컨텍스트 내부에는 1차 캐시가 존재한다.
    - 1차 캐시를 영속성 컨텍스트라고 이해해도 된다.
- Map<Key, Value> 로 1차 캐시에 저장된다.
    - key : @Id로 선언한 필드 값 (DB pk)
    - value : 해당 Entity 자체

```java
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");
/* 영속 상태 (Persistence Context 에 의해 Entity 가 관리되는 상태) */
// DB 저장 X, 1차 캐시에 저장됨
entityManager.persist(member); 
// 1차 캐시에서 조회
Member findMember = entityManager.find(Member.class, "Member1"); 
```
- 1차 캐시에 Entity 가 있을 때의 이점
    - **조회**
    - `entityManager.find()` 를 하면 DB보다 먼저, 1차 캐시를 조회한다.
    - 1차 캐시에 해당 Entity가 존재하면 바로 반환한다. 
- **Q.** 1차 캐시에 조회하고자하는 Entity가 없다면? 
    - 1) DB에서 조회 한다.
    - 2) 해당 Entity를 DB에서 꺼내와 1차 캐시에 저장한다.
    - 3) Entity를 반환한다.
    - 이후에 다시 해당 Entity를 조회하면 1차 캐시에 있는 Entity를 반환한다.
- 그러나, 사실 1차 캐시는 큰 성능 이점을 가지고 있지 않다. 
    - EntityManager는 Transaction 단위로 만들고 해당 DB Transaction이 끝날 때 (사용자의 요청에 대한 비지니스가 끝날 때) 같이 종료된다.
    - 즉, 1차 캐시도 모두 날라가기 때문에 굉장히 짧은 찰나의 순간에만 이득이 있다. (DB의 한 Transaction 안에서만 효과가 있다.)
    - 하지만, 비지니스 로직이 굉장히 복잡한 경우에는 효과가 있다. 
- **[참고]** 2차 캐시
    - 여러 명이 사용하는 캐시
    - Applicaion 전체에서 공유하는 캐시 
    <!-- * SELECT 쿼리가 안나간다. 한 트랜잭션 내에서. -->

### 2. 동일성 보장 (Identity) 
```java
Member a = entityManager.find(Member.class, "member1");
Member b = entityManager.find(Member.class, "member1");
System.out.println(a == b); // 동일성 비교 true
```
- 영속 Entity의 동일성(== 비교)을 보장한다.
    - 즉, "==" 비교가 true 임을 보장한다.
    - member1에 해당하는 Entity를 2번 조회하면 1차 캐시에 의해 같은 Reference 로 인식된다.
    - 하나의 Transaction 안에서 같은 Entity 비교 시 true
- 1차 캐시로 반복 가능한 읽기(REPEATABLE READ) 등급의 트랜잭션 격리 수준을 **DB가 아닌 아닌 애플리케이션 차원에서 제공**한다.


### 3. 엔티티 "등록" 시 트랜잭션을 지원하는 쓰기 지연 (Transactional Write-Behind)
```java
EntityManager entityManager = emf.createEntityManager();
EntityTransaction transaction = entityManager.getTransaction();
// EntityManager는 데이터 변경 시 트랜잭션을 시작해야 한다.
transaction.begin(); // Transaction 시작

entityManager.persist(memberA);
entityManager.persist(memberB);
// 이때까지 INSERT SQL을 DB에 보내지 않는다.

// 커밋하는 순간 DB에 INSERT SQL을 보낸다.
transaction.commit(); // Transaction 커밋 
```
- `entityManager.persist()`
    - JPA가 insert SQL을 계속 쌓고 있는 상태 
- `transaction.commit()`
    - 커밋하는 시점에 insert SQL을 동시에 DB에 보낸다.
    - 동시에 쿼리들을 쫙 보낸다.(쿼리를 보내는 방식은 동시 or 하나씩 옵션에 따라)

![그림 추가]()
- `entityManager.persist(memberA)`
    - 1) memberA가 **1차 캐시**에 저장한다.
    - 2) 1)과 동시에 JPA가 Entity를 분석하여 insert Query를 만든다.
    - 3) insert Query를 **쓰기 지연 SQL 저장소** 라는 곳에 쌓는다.
    - 4) DB에 바로 넣지 않고 기다린다.
    - 5) memberB도 동일.
- `transaction.commit()`
    - 6) **쓰기 지연 SQL 저장소**에 쌓여있는 Query들을 DB로 날린다. (`flush()`)
        - `flush()` 는 1차캐시를 지우지는 않는다. 쿼리들을 DB에 날려서 DB와 싱크를 맞추는 역할을 한다.
    - 7) `flush()` 후에 실제 DB Transaction이 커밋된다. (`commit()`)

- ***버퍼링 기능*** 이라고 할 수 있다.
    - DB에 쿼리를 날리는 것에 대한 옵션 설정을 통한 최적화 가능 
        - jpa의 이런 기능을 잘 활용하면 성능 개선을 쉽게 할 수 있다.
    - jdbc 일괄 처리 옵션 (jdbc batch option)
        - persistence.xml에 아래와 같은 옵션을 줄 수 있다.
        - commit 직전까지 insert query를 해당 사이즈 만큼 모아서 한번에 처리한다. 
    - `<property name="hibernate.jdbc.batch_size" value=10/>`

### 4. 엔티티 "수정" 시 변경 감지 (Dirty Checking) 
```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
transaction.begin(); // Transaction 시작

// 영속 엔티티 조회
Member memberA = em.find(Member.class, "memberA");

// 영속 엔티티 데이터 수정
memberA.setUsername("hi");
memberA.setAge(10);

// em.update(member) 또는 em.persist(member) 이런 코드가 있어야 하지 않을까?

transaction.commit(); // Transaction 커밋
```
- Entity 데이터 수정 시 `update()`나 `persist()`로 영속성 컨텍스트에 해당 데이터를 업데이트 해달라고 알려줘야 하지 않을까?
    - NO!
- Entity 데이터만 수정하고 commit 하면 알아서 DB에 반영된다.
- 즉, 데이터를 set하면 해당 데이터의 변경을 감지하여 자동으로 UPDATE Query가 나가는 것이다.

#### 변경 감지 (Dirty Checking)
![그림 추가]()
- 1차 캐시 
    - @Id, Entity, Snapshot (값을 읽어온 최초의 상태)
    - Snapshot: 영속성 컨텍스트에 최초로 값이 들어왔을 때의 상태값을 저장한다.
- 변경 감지 매커니즘 
    - `transaction.commit()` 을 하면 
    - 1) `flush()`가 일어날 때 **엔티티와 스냅샷을 일일이 비교**한다.
    - 2) 변경사항이 있으면 UPDATE Query를 만든다.
    - 3) 해당 UPDATE Query를 쓰기 지연 SQL 저장소에 넣는다.
    - 4) UPDATE Query를 DB에 반영한 후 `commit()`한다.
- **[참고]** @DynamicUpdate
    - Dirty Checking으로 생성되는 update 쿼리는 기본적으로 모든 필드를 업데이트한다.
    - @DynamicUpdate를 통해 엔티티 수정 시 변경된 필드만 반영되도록 할 수 있다.
    - https://jojoldu.tistory.com/415

### 5. 엔티티 삭제
```java
Member memberA = em.find(Member.class, "memberA");

em.remove(memberA); // 엔티티 삭제
```
- 위의 Entity 수정에서의 매커니즘과 동일
- Transaction의 commit 시점에 DELETE Query가 나간다.


# 관련된 Post
* JDBC, JPA/Hibernate, Mybatis의 차이에 대해 알고 싶으시면 [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)를 참고하시기 바랍니다.
* ORM의 개념에 대해 알고 싶으시면 [ORM이란](https://gmlwjd9405.github.io/2019/02/01/orm.html)을 참고하시기 바랍니다.


# Reference
> - [인프런 강의 참고](https://www.inflearn.com/course/ORM-JPA-Basic#)
