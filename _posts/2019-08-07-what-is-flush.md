---
layout: post
title: '[JPA] 플러시(Flush)란'
subtitle: '플러시(Flush)의 개념과 동작 과정'
date: 2019-08-07
author: heejeong Kwon
cover: '/images/inflearn-jpa/jpa-main2.png'
tags: inflearn jpa persistence-context flush
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[인프런 강의](https://www.inflearn.com/course/ORM-JPA-Basic#) 참고

## 플러시 (Flush)
- 영속성 컨텍스트의 변경 내용을 DB 에 반영하는 것을 말한다.
- Transaction commit 이 일어날 때 `flush`가 동작하는데, 이때 쓰기 지연 저장소에 쌓아 놨던 INSERT, UPDATE, DELETE SQL들이 DB에 날라간다.
    - **주의!** 영속성 컨텍스트를 비우는 것이 아니다.
- 쉽게 얘기해서 영속성 컨텍스트의 변경 사항들과 DB의 상태를 맞추는 작업이다.
    - 플러시는 영속성 컨텍스트의 변경 내용을 DB에 동기화한다.


### 플러시의 동작 과정 
1. 변경을 감지한다. (Dirty Checking)
2. 수정된 Entity를 쓰기 지연 SQL 저장소에 등록한다.
3. 쓰기 지연 SQL 저장소의 Query를 **DB에 전송**한다. (등록, 수정, 삭제 Query)

- flish가 발생한다고 해서 commit이 이루어지는 것이 아니고 flush 다음에 실제 commit이 일어난다.
- 플러시가 동작할 수 있는 이유는 데이터베이스 트랜잭션(작업 단위)이라는 개념이 있기 때문이다.
    - 트랜잭션이 시작되고 해당 트랜잭션이 commit 되는 시점 직전에만 동기화 (변경 내용을 날림) 해주면 되기 때문에, 그 사이에서 플러시 매커니즘의 동작이 가능한 것이다.
- **[참고]** JPA는 기본적으로 데이터를 맞추거나 동시성에 관련된 것들은 데이터베이스 트랜잭션에 위임한다. 


### 영속성 컨텍스트를 플러시하는 방법
#### 1. em.flush() 을 통한 직접 호출
```java
// 영속 상태 (Persistence Context 에 의해 Entity 가 관리되는 상태)
Member member = new Member(200L, "A");
entityManager.persist(member);

entityManager.flush(); // 강제 호출 (쿼리가 DB 에 반영됨)

System.out.println("DB INSERT Query 가 즉시 나감. -- flush() 호출 후 --  Transaction commit 됨.");
tx.commit(); // DB에 insert query 가 날라가는 시점 (Transaction commit)
```

- **Q.** 플러시가 일어나면 1차 캐시가 모두 지워질까?
    - NO! 그대로 남아있다.
    - 쓰기 지연 SQL 저장소에 있는 Query들만 DB에 전송까지만 되는 과정일뿐이다.


#### 2. 트랜잭션 커밋 시 플러시 자동 호출

#### 3. JPQL 쿼리 실행 시 플러시 자동 호출
```java
em.persist(memberA);
em.persist(memberB);
em.persist(memberC);

// 중간에 JPQL 실행
query = entityManager.createQuery("select m from Member m", Member.class);
List<Member> members = query.getResultList();
```
- **Q.** memberA, B, C 를 영속성 컨텍스트에 저장한 상태에서 바로 조회하면 조회가 될까?
    - 조회가 되지 않는다.
    - DB 에 Query로도 날라가야 반영이 될텐데 INSERT Query 자체가 날라가지 않은 상태이다. 이런 상태에서 JPQL로 DB에서 가져오는 SELECT Query 요청을 한 것이므로 당연히 조회되지 않는다.
    - JPQL은 SQL로 번역이 돼서 실행되는 것이다. 
- 이 때문에 JPA의 기본 모드는 JPQL 쿼리 실행 시 `flush()`를 자동으로 날린다.
    - 즉, JPQL 쿼리 실행 시 플러시 자동 호출로 인해 위의 코드는 조회가 가능하다.

### 플러시 모드 옵션
- `em.setFlushMode(FlushModeType.COMMIT);`
- **FlushModeType.AUTO**
- 기본 설정 값
- Transaction 을 commit 하거나 Query를 실행할 때 `flush()`를 먼저 수행한다.
- **FlushModeType.COMMIT**
- Transaction commit 할때만 `flush()`를 먼저 수행한다.
- Query를 실행할 때는 `flush()`를 먼저 수행하지 않는다.
- 이점: persist 한 것과 전혀 다른 테이블을 조회하는 경우



# 관련된 Post
* JDBC, JPA/Hibernate, Mybatis의 차이에 대해 알고 싶으시면 [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)를 참고하시기 바랍니다.
* ORM의 개념에 대해 알고 싶으시면 [ORM이란](https://gmlwjd9405.github.io/2019/02/01/orm.html)을 참고하시기 바랍니다.


# Reference
> - [인프런 강의 참고](https://www.inflearn.com/course/ORM-JPA-Basic#)
