---
layout: post
title: '[JPA] JPA란'
subtitle: 'JPA 소개 및 JPA의 기본 동작 과정'
date: 2019-08-04
author: heejeong Kwon
cover: '/images/inflearn-jpa/jpa-main2.png'
tags: inflearn jpa 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[인프런 강의](https://www.inflearn.com/course/ORM-JPA-Basic#) 참고

## ORM 이란
- Object-relational mapping (객체 관계 매핑)
    - 객체는 객체대로 설계하고, 관계형 데이터베이스는 관계형 데이터베이스대로 설계
    - ORM 프레임워크가 중간에서 매핑
- 대중적인 언어에는 대부분 ORM 기술이 존재
- ORM은 객체와 RDB **두 기둥 위에 있는 기술**

## JPA 란
### Java Persistence API
- EJB
    - 과거의 자바 표준 
    - Entity Bean
    - 속도 느림. 
- Hibernate
    - ORM 프레임워크 (오픈 소스)
- JPA
    - 현재 자바 진영의 ORM 기술 표준
    - JPA는 **인터페이스의 모음**이다. 즉, 실제로 동작하는 것이 아니다.
    - JPA 2.1 표준 명세를 구현한 3가지 **구현체**: Hibernate, EclipseLink, DataNucleus
    - ![](/images/inflearn-jpa/basic-structure.png)
    - 버전
        - JPA 1.0(JSR 220) 2006년 : 초기 버전. 복합 키와 연관관계 기능이 부족
        - JPA 2.0(JSR 317) 2009년 : 대부분의 ORM 기능을 포함, JPA Criteria 추가
        - JPA 2.1(JSR 338) 2013년 : 스토어드 프로시저 접근, 컨버터(Converter), 엔티
티 그래프 기능이 추가

### JPA를 사용해야되는 이유?
#### SQL 중심적인 개발에서 객체 중심으로 개발
- [SQL 중심적인 개발의 문제점 참고](https://gmlwjd9405.github.io/2019/08/03/reason-why-use-jpa.html)

#### 생산성
- 간단한 CRUD
    - 저장: `jpa.persist(member)`
    - 조회: `Member member = jpa.find(memberId)`
    - 수정: `member.setName(“변경할 이름”)`
    - 삭제: `jpa.remove(member)`

#### 유지보수
- 기존: 필드 변경시 모든 SQL 수정
- JPA: 필드만 추가하면 됨, SQL은 JPA가 처리

#### 패러다임의 불일치 해결
1. JPA와 상속
- 저장
  - 개발자가 할 일: `jpa.persist(album);`
  - 나머진 JPA가 처리: `INSERT INTO ITEM ...`, `INSERT INTO ALBUM ...`
- 조회
  - 개발자가 할 일: `Album album = jpa.find(Album.class, albumId);`
  - 나머진 JPA가 처리: `SELECT I.*, A.* FROM ITEM I JOIN ALBUM A ON I.ITEM_ID = A.ITEM_ID`
2. JPA와 연관관계
- 연관관계 저장
  - `member.setTeam(team);`
  - `jpa.persist(member);`
3. JPA와 객체 그래프 탐색
- 객체 그래프 탐색
  - `Member member = jpa.find(Member.class, memberId);`
  - `Team team = member.getTeam();`
- 신뢰할 수 있는 엔티티, 계층
```java
class MemberService { 
        ...
        public void process() { 
            Member member = memberDAO.find(memberId); 
            member.getTeam(); //자유로운 객체 그래프 탐색
            member.getOrder().getDelivery(); 
        } 
}
```
4. JPA와 비교하기
- 동일한 트랜잭션에서 조회한 엔티티는 같음을 보장
```java
String memberId = "100"; 
Member member1 = jpa.find(Member.class, memberId); 
Member member2 = jpa.find(Member.class, memberId);
member1 == member2; //같다.
```

#### 성능
- 중간 계층이 있는 경우 아래의 방법으로 성능을 개선할 수 있는 기능이 존재한다.
    - 모아서 쓰는 *버퍼링 기능*
    - 읽을 때 쓰는 *캐싱 기능*

1. 1차 캐시와 동일성(identity) 보장 - *캐싱 기능*
- 1) 같은 트랜잭션 안에서는 같은 엔티티를 반환 - **약간의** 조회 성능 향상 (크게 도움 X)
```java
String memberId = "100"; 
Member m1 = jpa.find(Member.class, memberId); // SQL 
Member m2 = jpa.find(Member.class, memberId); // 캐시 (SQL 1번만 실행, m1을 가져옴)
println(m1 == m2) //true
```
- 2) DB Isolation Level이 Read Commit이어도 애플리케이션에서 Repeatable Read 보장
2. 트랜잭션을 지원하는 쓰기 지연(transactional write-behind) - *버퍼링 기능*
- INSERT
```java
/** 1. 트랜잭션을 커밋할 때까지 INSERT SQL을 모음 */
transaction.begin();  // [트랜잭션] 시작
em.persist(memberA); 
em.persist(memberB); 
em.persist(memberC); 
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.
//커밋하는 순간 데이터베이스에 INSERT SQL을 모아서 보낸다. 
/** 2. JDBC BATCH SQL 기능을 사용해서 한번에 SQL 전송 */
transaction.commit(); // [트랜잭션] 커밋
```
- UPDATE
```java
/** 1. UPDATE, DELETE로 인한 로우(ROW)락 시간 최소화 */
transaction.begin();  // [트랜잭션] 시작
changeMember(memberA);  
deleteMember(memberB);  
비즈니스_로직_수행();     //비즈니스 로직 수행 동안 DB 로우 락이 걸리지 않는다.    
//커밋하는 순간 데이터베이스에 UPDATE, DELETE SQL을 보낸다.
/** 2. 트랜잭션 커밋 시 UPDATE, DELETE SQL 실행하고, 바로 커밋 */
transaction.commit(); // [트랜잭션] 커밋
```
3. 지연 로딩(Lazy Loading)
- 지연 로딩: 객체가 실제 사용될 때 로딩
    <!-- - ![x](/images/inflearn-jpa/basic-structure.png) -->
- 즉시 로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조회
    <!-- - ![x](/images/inflearn-jpa/basic-structure.png) -->

#### 데이터 접근 추상화와 벤더 독립성
#### 표준

---

## JPA의 동작 과정
- JPA는 애플리케이션과 JDBC 사이에서 동작 
    - ![](/images/inflearn-jpa/basic-structure.png)
    - 개발자가 JPA를 사용하면, JPA 내부에서 JDBC API를 사용하여 DB와 통신한다.
    - JPA 구동 방식
        - ![](/images/inflearn-jpa/basic-structure.png)
        - Persistence
        - 설정 정보 조회 
- 저장 과정
    - ![](/images/inflearn-jpa/basic-structure.png)
- 조회 과정
    - ![](/images/inflearn-jpa/basic-structure.png)



# 관련된 Post
* JDBC, JPA/Hibernate, Mybatis의 차이에 대해 알고 싶으시면 [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)를 참고하시기 바랍니다.
* ORM의 개념에 대해 알고 싶으시면 [ORM이란](https://gmlwjd9405.github.io/2019/02/01/orm.html)을 참고하시기 바랍니다.


# Reference
> - [인프런 강의 참고](https://www.inflearn.com/course/ORM-JPA-Basic#)
