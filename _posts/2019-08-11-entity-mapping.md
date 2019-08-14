---
layout: post
title: '[JPA] 엔티티 매핑 방법 (Entity Mapping)'
subtitle: '객체와 테이블, 필드와 컬럼 매핑하는 방법을 이해하자.'
date: 2019-08-11
author: heejeong Kwon
cover: '/images/inflearn-jpa/jpa-main2.png'
tags: inflearn jpa mapping annotation
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

[인프런 강의](https://www.inflearn.com/course/ORM-JPA-Basic#) 참고


<!-- ![그림 추가]() -->

**[들어가기 전]** JPA에서 중요한 것!
1. 영속성 컨텍스트, JPA 내부 동작 등의 **매커니즘 측면**
  - JPA가 내부적으로 어떤 매커니즘으로 동작하는지.
2. **설계 측면**
  - 객체와 RDB를 어떻게 매핑해서 사용하는지.

## 엔티티 매핑
- 기본적인 엔티티 매핑 소개 
  - 객체와 테이블 매핑
    - @Entity
    - @Table
  - 필드와 컬럼 매핑
    - @Column
  - 기본 키 매핑
    - @Id
  - 연관관계 매핑
    - Ex) Member와 Team
    - @ManyToOne
    - @JoinColumn

## 객체와 테이블 매핑
#### @Entity
- @Entity가 붙은 클래스는 JPA가 관리하는 클래스로, 해당 클래스를 엔티티라고 한다.
- JPA를 사용해서 테이블과 매핑할 클래스는 반드시 @Entity 를 붙여야 한다.
- **[주의]**
  - **기본 생성자 필수**
    - 파라미터가 없는 public 또는 protected 생성자가 필요하다.
    - JPA spec으로 규정되어 있다.
      - Why? JPA를 구현해서 쓰는 라이브러리들이 다양한 기술(Ex. Reflection)을 사용해서 객체를 프록싱할 때 필요하기 때문.
  - final 클래스, enum, interface, inner 클래스는 엔티티로 사용할 수 없다.
  - DB에 저장하고 싶은 필드에는 final을 사용할 수 없다.

- Entity 속성
<!-- //// 추가  -->
  - name
    - `@Entity(name = "Member")`
    - JPA에서 사용할 엔티티 이름을 지정한다.
    - 기본값: 클래스 이름을 그대로 사용 
    - 같은 클래스 이름이 없으면 가급적 기본값을 사용하도록 한다.

#### @Table
- @Table은 엔티티와 매핑할 테이블을 지정하는 것이다.

- @Table 속성
<!-- //// 추가  -->
  - name
    - `@Table(name = "MBR")`
    - 매핑할 테이블 이름을 지정한다.
    - 기본값: 엔티티 이름을 사용
  - catalog
    - 데이터베이스 catalog 매핑
  - schema
    - 데이터베이스 schema 매핑
  - uniqueConstraints(DDL)
    - DDL 생성 시에 유니크 제약 조건 생성 


---

## 데이터베이스 스키마 자동 생성
- DDL을 애플리케이션 실행 시점(loading)에 자동으로 생성한다.
- 테이블 중심 -> 객체 중심
- 데이터베이스 방언을 활용해서 **데이터베이스에 맞는 적절한 DDL**을 생성한다.
  - Ex) oracle: varchar2, MySQL: varchar 등
  <!-- - ////////////////////추가  -->
- 이렇게 생성된 DDL은 개발 로컬 장비에서만 사용한다.
  - 즉, 운영 서버에서는 사용하지 않거나, 적절히 다듬은 후 사용한다.
  - 로컬 PC에서 개발할 때 사용한다.

```xml
<property name="hibernate.hbm2ddl.auto" value="create"/>
```
- `hibernate.hbm2ddl.auto` 옵션의 속성값

| 옵션 | 설명 |
|-----|-----|
| create | 기존 테이블 삭제 후 다시 생성 (DROP + CREATE) |
| create-drop | create와 같으나 종료 시점에 테이블 DROP (테스트에서 사용하면 도움됨) |
| update | 변경 분만 반영 (운영 DB에서는 사용하면 안됨) `alter table ~` |
| validate | 엔티티와 테이블이 정상 매핑되었는지만 확인 |
| none | 아무 속성도 사용하지 않음 |

- **[주의]**
  - **운영 장비에는 절대 create, create-drop, update를 사용하면 안된다.**
  - 개발 초기 단계(로컬 개발 서버)
    - create 또는 update
  - 테스트 서버(여러 명이 같이 사용하는 개발 서버)
    - update 또는 validate
    - 웬만하면 테스트 서버에서도 validate나 none을 쓰는 것이 좋다.
    - Why? 데이터가 많은 상태에서 `alter`를 했을 때 자칫 시스템이 중단될 수도 있기 때문이다.
    - 직접 스크립트를 짜서 적용해보고 문제가 없으면 DBA에게 검수를 받은 후 운영 서버에 적용하는 것을 권장한다.
  - 스테이징과 운영 서버
    - validate 또는 none

- **[참고]** 데이터베이스 방언이란?
  <!-- -  ////////////////// -->

### DDL 생성 기능
- 제약 조건 추가 가능 
  - `@Column(nullable = false, length = 10)`
    - 회원 이름은 필수
    - 10글자 초과 X
  - `@Column(unique = true)`
    - DB에 영향을 주는 것
    - 해당 필드는 unique 함을 의미 
  - `@Table(uniqueConstraints = {@UniqueConstraint(name = "NAME_AGE_UNIQUE", columnNames = {"NAME", "AGE"})})`
    - Unique 제약 조건 추가
- DDL 생성 기능은 DDL을 자동 생성할 때만 사용되고 JPA의 실행 로직에는 영향을 주지 않는다.
  - 즉, DB에만 영향을 주는 것이지 애플리케이션에 영향을 주는 것이 아니다.
  - JPA의 실행 매커니즘에 영향을 주는 것이 아니라 `alter table`과 같은 스크립트가 실행되는 것을 말한다.
  - Cf) `insert`, `update`는 Runtime에 영향을 준다.

---

## 필드와 컬럼 매핑
### 요구 사항 추가
1. 회원은 일반 회원과 관리자로 구분해야 한다.
2. 회원 가입일과 수정일이 있어야 한다.
3. 회원을 설명할 수 있는 필드가 있어야 한다. 이 필드는 길이 제한이 없다.

```java
public enum RoleType {
  USER, ADMIN
}
```
```java
@Entity
@Table(name = "MBR")
public class Member {
  @Id
  private Long id;

  @Column(name = "name")
  private String username;

  private Integer age;

  @Enumerated(EnumType.String)
  private RoleType roleType;

  @Temporal(TemporalType.TIMESTAMP)
  private Date createDate;

  @Temporal(TemporalType.TIMESTAMP)
  private Date lastModifiedDate;

  @Lob
  private String description;

  @Transient
  private int temp;

  public Member() {

  }
}
```
```sql
create table Member (
  id bigint not null,
  age integer,
  createdDate timestamp,
  description clob,
  lastModifiedDate timestamp,
  roleType varchar(255),
  name varchar(255),
  primary key (id)
)
```

#### @Id
- pk 매핑

#### @Column
- 컬럼 매핑 
- @Column 속성 
<!-- - **[표 추가]** /////////// -->
  - name
    - `@Colunm(name = "name")`
    - 객체명과 DB 컬럼명을 다르게 하고 싶은 경우, DB 컬럼명으로 설정할 이름을 name 속성으로 적는다.
  - insertable
  - updatable
    - 컬럼을 수정했을 때 DB에 추가를 할 것인지 여부 
    - update 문이 나갈 때 해당 컬럼을 반영할 것인지 여부 
    - `@Column(updatable = false)`: 변경이 되어도 DB에 반영되지 않는다.
  - nullable
    - `@Column(nullable = false)`: NOT NULL 제약조건이 된다.
  - unique 
    - 잘 사용하지 않는다.
      - Why? `constraint UK_ewkrjwel239flskdfj01 unique (name)` 과 같이 이름을 랜덤으로 만든다.
    - 이름에 대한 설정을 직접하려면 아래의 방법을 사용해야 한다.
      - `@Table(uniqueConstraints = {@UniqueConstraint(name = "NAME_AGE_UNIQUE", columnNames = {"NAME", "AGE"})})`
  - columnDefinition
    - `@Column(columnDefinition = "varchar(100) default 'EMPTY'")`: 직접 컬럼 정보를 작성할 수 있다.
  - length
    - 문자 길이 제약 조건 
    - String 타입에만 사용한다.
  - precision
    - 숫자가 엄청 큰 BigDecimal 의 경우 (Ex.`private BigDecimal age;`)


#### @Enumerated
- Enum Type 매핑
  - Enum 객체 사용 시 해당 annotation을 사용 
  - DB에는 Enum Type이 없다.
- EnumType.ORDINAL
  - enum 순서를 데이터베이스에 저장 (기본값)
- EnumType.String
  - enum 이름을 데이터베이스에 저장 
- **[주의]** 
  - EnumType.ORDINAL 은 사용하지 않는다.
  - Why? 요구 사항에 따라 새로운 Type이 추가될 수 있기 때문에 순서가 변경될 수 있다.
    - 즉, GUEST를 enum class 맨 앞에 추가 했을 때 GUEST와 USER 모두가 '0'으로 저장된다.
  - 즉, 필수처럼 EnumType.String 을 사용한다.

```java
@Target({ElementType.METHOD, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Enumerated {
    EnumType value() default EnumType.ORDINAL;
}
```
```java
public enum EnumType {
    ORDINAL, STRING;
    private EnumType() { }
}
```

#### @Temporal
- 날짜 Type (`java.util.Date`, `java.util.Calendar`) 매핑 
  - 날짜 타입 사용 시 해당 annotation을 사용 
- java의 Date에는 날짜와 시간이 모두 존재한다. 그러나 DB의 Date에는 아래의 세 가지로 구분해서 사용한다
- TemporalType enum class에는 세 가지가 존재한다.
  - DATE: 날짜, DB의 date 타입과 매핑 
    - Ex. 2019-08-13
  - TIME: 시간, DB의 time 타입과 매핑
    - Ex. 14:20:38
  - TIMESTAMP: 날짜와 시간, DB의 timestamp 타입과 매핑
    - Ex. 2019-08-13 14:20:38
- **지금은 필요 없음**
  - java 8의 `LocalDate`(date), `LocalDateTime`(timestamp) 을 사용할 때는 생략이 가능하다.
  - hibernate에서 지원 
  - Ex. `private LocalDate createDate` 또는 `private LocalDateTime createDateTime`

#### @Lob
- DB에서 varchar를 넘어서는 큰 내용을 넣고 싶은 경우에 해당 annotation을 사용 
- @Lob에는 지정할 수 있는 속성이 없다.
- 매핑하는 필드의 타입에 따라 DB의 BLOB, CLOB과 매핑된다.
  - 필드의 타입이 문자열: CLOB
    - String, char[], java.sql.CLOB
  - 그 외 나머지: BLOB
    - byte[], java.sql.BLOB


#### @Transient
- 특정 필드를 컬럼에 매핑하지 않음.
<!-- 내용 확인 -->
- DB에 관계없이 메모리에서만 사용하고자 하는 객체에 해당 annotation을 사용
  - 즉, 주로 메모리상에서만 임시로 어떤 값을 보관하고 싶을 때 사용한다.
- 해당 annotation이 붙은 필드는 DB에 저장, 조회가 되지 않는다.


---

# 관련된 Post
* JDBC, JPA/Hibernate, Mybatis의 차이에 대해 알고 싶으시면 [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)를 참고하시기 바랍니다.
* ORM의 개념에 대해 알고 싶으시면 [ORM이란](https://gmlwjd9405.github.io/2019/02/01/orm.html)을 참고하시기 바랍니다.


# Reference
> - [인프런 강의 참고](https://www.inflearn.com/course/ORM-JPA-Basic#)
