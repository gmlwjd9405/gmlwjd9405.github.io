---
layout: post
title: '[JPA] JPA를 사용하는 이유'
subtitle: 'SQL 중심적인 개발의 문제점'
date: 2019-08-03
author: heejeong Kwon
cover: '/images/inflearn-jpa/jpa-main2.png'
tags: inflearn jpa 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[인프런 강의](https://www.inflearn.com/course/ORM-JPA-Basic#) 참고

## JPA와 모던 자바 데이터 저장 기술
- 애플리케이션 객체 지향 언어 (Java, Scala 등) + 관계형 DB (Oracle, MySQL 등)
- 즉, 객체를 관계형 DB에 관리하는 것이 중요

## SQL 중심적인 개발의 문제점 
### 지루한 코드의 무한 반복 
- CRUD의 반복 
- 자바 객체를 SQL로, SQL을 자바 객체로 변환하는 과정의 반복 

### 객체 지향과 RDB 간의 패러다임 불일치
- 객체 지향: ‘객체 지향 프로그래밍은 추상화, 캡슐화, 정보은닉, 상속, 다형성 등 시스템의 복잡성을 제어할 수 있는 다양한 장치들을 제공한다.’
- RDB: 데이터를 잘 정규화해서 보관하는 것이 목표 

### Object와 RDB의 차이
1. 상속
- RDB는 상속 관계가 없다.
- 유사하게 물리 모델로, Table 슈퍼타입 서브타입 관계가 존재한다. 
  - 하지만!! 조회할 때 복잡해지기 때문에 DB에 저장할 객체에는 상속 관계를 쓰지 않는다.
2. 연관 관계
- 객체는 Reference를 사용한다. 
  - 한 방향으로만 관계가 존재 
  - Ex. `member.getTeam();`
- RDB는 FK를 사용하여 Join 쿼리를 통해 관계를 찾는다.
  - 양 방향으로 모두 조회 가능. 즉 단방향이 존재하지 않는다.
  - Ex. `JOIN ON M.TEAM_ID = T.TEAM_ID`
3. 데이터 타입
4. 데이터 식별 방법

### 모델링 과정에서의 문제
#### 객체를 테이블에 맞추어 모델링
```java
class Member { 
    String id;      //MEMBER_ID 컬럼 사용
    Long teamId;    //TEAM_ID FK 컬럼 사용 //**
    String username;//USERNAME 컬럼 사용
}
class Team { 
    Long id;     //TEAM_ID PK 사용
    String name; //NAME 컬럼 사용
}
```
- 객체를 테이블에 맞추어 모델링에서의 문제: 객체지향스럽지 않다.
#### 객체다운 모델링
```java
class Member { 
    String id;      //MEMBER_ID 컬럼 사용 
    Team team;      //참조로 연관관계를 맺는다. //** 
    String username;//USERNAME 컬럼 사용 
                     
    Team getTeam() { 
        return team; 
    } 
}
class Team { 
    Long id;     //TEAM_ID PK 사용 
    String name; //NAME 컬럼 사용 
}
```
- `member.getTeam().getId()` 를 TEAM_ID에 넣는다.
- `INSERT INTO MEMBER(MEMBER_ID, TEAM_ID, USERNAME) VALUES ...`
- 객체다운 모델링에서의 문제: 데이터 조회의 복잡성 
  ```sql
  SELECT M.*, T.* 
  FROM MEMBER M 
  JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID  
  ```
  ```java
  public Member find(String memberId) { 
      //SQL 실행 ... 
      Member member = new Member(); 
      //데이터베이스에서 조회한 회원 관련 정보를 모두 입력 
      Team team = new Team(); 
      //데이터베이스에서 조회한 팀 관련 정보를 모두 입력
      //회원과 팀 관계 설정 
      member.setTeam(team); //** 
      return member; 
  }
  ```

#### 객체다운 모델링 개선
- 객체는 자유롭게 객체 그래프(연관 관계가 있는 객체 사이)를 탐색할 수 있어야 한다.
  - 하지만, RDB와 연결된 데이터 탐색 시 처음 실행하는 SQL에 따라 탐색 범위가 결정된다.
  ```sql
  SELECT M.*, T.*
  FROM MEMBER M
  JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID  
  ```
  ```java
  member.getTeam(); //OK
  member.getOrder(); //null
  ```
  - 이 때문에 **Entity 신뢰 문제**가 발생한다.
  ```java
  class MemberService { 
        ...
        public void process() { 
            Member member = memberDAO.find(memberId); 
            member.getTeam(); //??? 
            member.getOrder().getDelivery(); // ??? 
        } 
  }
  ```
- 그렇다고 모든 객체를 미리 로딩할 수는 없다.
  - 대안: 상황에 따라 동일한 회원 조회 메서드를 여러벌 생성
  - 문제: 다양한 상황을 고려한 너무 많은 메서드 생성
  ```java
  memberDAO.getMember();  // Member만 조회
  memberDAO.getMemberWithTeam();  // Member와 Team 조회
  memberDAO.getMemberWithOrderWithDelivery();  // Member,Order,Delivery 
  ```

- 계층형 아키텍처
  - 진정한 의미의 계층 분할이 어렵다.
  <!-- - 물리적 .... -->

- 비교하기
```java
String memberId = "100"; 
Member member1 = memberDAO.getMember(memberId); 
Member member2 = memberDAO.getMember(memberId);
member1 == member2; // 다르다!!!
class MemberDAO { 
    public Member getMember(String memberId) { 
        String sql = "SELECT * FROM MEMBER WHERE MEMBER_ID = ?"; 
        ... 
        //JDBC API, SQL 실행 
        return new Member(...); 
    }
}
```
```java
String memberId = "100";
Member member1 = list.get(memberId);
Member member2 = list.get(memberId);
member1 == member2; // 같다!!! (참조값이 같다.)
```

객체답게 모델링 할수록 매핑 작업만 늘어난다. 
객체를 자바 컬렉션에 저장 하듯이 DB에 저장할 수는 없을까?


# 관련된 Post
* JDBC, JPA/Hibernate, Mybatis의 차이에 대해 알고 싶으시면 [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)를 참고하시기 바랍니다.
* ORM의 개념에 대해 알고 싶으시면 [ORM이란](https://gmlwjd9405.github.io/2019/02/01/orm.html)을 참고하시기 바랍니다.
- JPA의 개념에 대해 알고 싶으시면 [JPA란](https://gmlwjd9405.github.io/2019/08/04/what-is-jpa.html)을 참고하시기 바랍니다.


# Reference
> - [인프런 강의 참고](https://www.inflearn.com/course/ORM-JPA-Basic#)
