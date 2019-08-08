---
layout: post
title: '[JPA] 엔티티의 생명주기 (Entity LifeCycle)'
subtitle: '영속과 준영속의 개념 이해하기'
date: 2019-08-08
author: heejeong Kwon
cover: '/images/inflearn-jpa/jpa-main2.png'
tags: inflearn jpa persistence-context 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[인프런 강의](https://www.inflearn.com/course/ORM-JPA-Basic#) 참고


## 엔티티의 생명주기 (Entity LifeCycle)
![그림 추가]()
- **비영속(new/transient)**
  - 영속성 컨텍스트와 전혀 관계가 없는 상태
  - 객체를 생성'만' 한 상태 
  - ![그림 추가]()
  ```java
  // 객체를 생성한 상태 (비영속)
  Member member = new Member();
  member.setId("member1");
  member.setUsername("회원1");
  ```
- **영속(managed)** (아래 추가 설명)
  - 영속성 컨텍스트에 저장된 상태
  - Entity가 영속성 컨텍스트에 의해 관리되는 상태
  - `EntityManager.persist(entity);`
    - 영속 상태가 된다고 바로 DB에 쿼리가 날라가지 않는다. (즉, DB 저장 X)
  - `transaction.commit();`
    - 트랜잭션의 commit 시점에 영속성 컨텍스트에 있는 정보들이 DB에 쿼리로 날라간다.
  - ![그림 추가]()
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
- **준영속(detached)** (아래 추가 설명)
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

## 영속 상태와 준영속 상태  
- **영속 상태**란
    - 1) `entityManager.persist();` 로 영속성 컨텍스트에 저장된 상태
    - 2) `entityManager.find();` 로 조회할 때 영속성 컨텍스트 1차 캐시에 없어서 DB에서 조회한 후 해당 Entity를 1차 캐시에 올라간 상태 (엔티티 매니저가 관리하는 상태)
- **준영속 상태**
    - 영속 상태의 엔티티가 영속성 컨텍스트에서 분리(detached)된 상태 
    - 준영속 상태에서는 영속성 컨텍스트가 제공하는 기능을 사용하지 못한다.
        - Dirty Checking, Update Query 
- 기본 예시
```java
Member member = entityManager.find(Member.class, 150L); // 영속 상태
member.setName("AAAAA");
transaction.commit();
```
- `entityManager.find()`: 1차 캐시에 없으므로 DB에서 조회한 Entity를 1차 캐시에 넣는다. (영속 상태)
- 변경 감지(Dirty Checking)
    - 이름 변경에 대한 Entity 데이터 변화를 감지한다.
    - 1차 캐시의 Entity와 Snapshot이 다른 것을 감지하고 UPDATE Query를 날린다.

### 준영속 상태로 만드는 방법 (영속 -> 준영속)
1. entityManager.detach(entity): 특정 엔티티만 준영속 상태로 전환
2. entityManager.clear(): 영속성 컨텍스트를 완전히 초기화
3. entityManager.close(): 영속성 컨텍스트를 종료

#### detach
```java
// 영속 상태 (Persistence Context 에 의해 Entity 가 관리되는 상태)
Member findMember = entityManager.find(Member.class, 150L);
findMember.setName("AAAAA");

entityManager.detach(findMember); // 영속성 컨텍스트에서 떼넨다. (더 이상 JPA 의 관리 대상이 아님.)

tx.commit(); // DB에 insert query 가 날라가는 시점 (아무 일도 발생하지 않음.)
```
- `entityManager.detach();`: 해당 Entity를 영속성 컨텍스트에서 분리한다.
    - **JPA가 관리하지 않는 객체**가 된다.
- Transaction commit에서 아무 일도 발생하지 않는다.
- 즉, Entity가 변경이 되었는데 실제로 UPDATE Query가 나가지 않는다.
- 직접 쓸 일이 거의 없다.

#### clear
```java
// 영속 상태 (Persistence Context 에 의해 Entity 가 관리되는 상태)
Member findMember = entityManager.find(Member.class, 150L);
findMember.setName("AAAAA");

entityManager.clear(); // 영속성 컨텍스트를 완전히 초기화

Member findMember2 = entityManager.find(Member.class, 150L); // 간은 Entity 를 다시 조회

tx.commit(); // DB에 insert query 가 날라가는 시점 (아무일도 발생하지 않음.)
```
- `entityManager.clear();`: EntityManager 안에 있는 영속성 컨텍스트를 모두 지운다.
    - 영속성 컨텍스트를 완전히 초기화
- clear 후 같은 Entity를 다시 조회할 때는 SELECT Query가 다시 나간다.
    - 총 2번의 SELECT 쿼리가 발생한다.
    - clear는 1차 캐시에 상관없이 쿼리를 확인하고 싶을 때 즉, testcase 작성 시에 도움이 된다.

#### close
- 영속성 컨텍스트를 종료시킨다.
- JPA 의 관리 대상이 아니게 된다.


# 관련된 Post
* JDBC, JPA/Hibernate, Mybatis의 차이에 대해 알고 싶으시면 [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)를 참고하시기 바랍니다.
* ORM의 개념에 대해 알고 싶으시면 [ORM이란](https://gmlwjd9405.github.io/2019/02/01/orm.html)을 참고하시기 바랍니다.


# Reference
> - [인프런 강의 참고](https://www.inflearn.com/course/ORM-JPA-Basic#)
