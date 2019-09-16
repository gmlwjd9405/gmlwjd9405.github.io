---
layout: post
title: '[JPA] 값 타입(2) - 컬렉션 타입'
subtitle: '값 타입 중 컬렉션 타입에 대해 이해한다.'
date: 2019-09-13
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

> [기본값 타입과 임베디드 타입 참고 POST](https://gmlwjd9405.github.io/2019/09/12/value-type-of-basic-and-embedded.html)

## 3. 값 타입 컬렉션(collection value type)
<!-- ![그림 추가]() -->
- 값 타입을 컬렉션에 담아서 쓰는 것을 말한다.
    - 연관관계 매핑에서 엔티티를 컬렉션으로 사용하는 것이 아니라 값 타입을 컬렉션에 쓰는 것이다.
- 값 타입 컬렉션은 값 타입을 *하나 이상* 저장할 때 사용한다.

```java
public class Member {
    @Id
    private Long id;
    
    private Set<String> favoriteFoods;
    private List<Address> addressHistory;
}
```

- RDB에는 내부적으로 컬렉션을 담을 수 있는 구조가 없다. 그냥 값만 넣을 수 있는 구조이다.
    - 컬렉션은 1:N 의 개념이기 때문에 DB는 컬렉션을 하나의 테이블에 저장할 수 없다.
    - 이런 관계를 DB 테이블에 저장하려면 **별도의 테이블**(Join이 가능하도록)이 필요하다.
- **Q.** FAVORITE_FOOD, ADDRESS 에서 모든 속성으로 pk로 잡는 이유?
    - **A.** 해당 테이블은 값 타입인데, 하나를 식별자로 두는 개념을 도입하면 엔티티가 될 수도 있기 때문이다.
    - 즉, 테이블에 값들만 저장하고 이들을 묶어서 pk로 사용한다.  

### 구현 예시 
```java
@Entity
public class Member {
    ...
    @ElementCollection
    @CollectionTable(
        name = "FAVORITE_FOOD",
        joinColumns = @JoinColumn(name = "MEMBER_ID"))
    @Column(name = "FOOD_NAME") // 컬럼명 지정 (예외)
    private Set<String> favoriteFoods = new HashSet<>();

    @ElementCollection
    @CollectionTable(
        name = "ADDRESS",
        joinColumns = @JoinColumn(name = "MEMBER_ID"))
    private List<Address> addressHistory = new ArrayList<>();
    ...
}
```

- 기본 사용 어노테이션
    - `@ElementCollection`, `@CollectionTable` 어노테이션을 사용한다.
- 과정
    - Member 클래스에 값 타입 컬렉션을 추가한다.
    - addressHistory는 임베디드 타입이므로 컬럼명들을 그대로 사용하면 된다.
    - favoriteFoods는 String 하나(내가 만든 것이 아님)에 대한 Set이기 때문에 컬럼명을 지정해준다. 
    - memberId를 외래키로 지정한다.
        - 어떤 memberId에 소속되는지 알아야하기 때문에 연관 관계가 필요하다.

### 예제 
#### 컬렉션 값 타입 저장
```java
Member member = new Member();
member.setUsername("member1");
member.setHomeAddress(new Address("homeCity", "street", "10000"));

member.getFavoriteFoods().add("치킨");
member.getFavoriteFoods().add("족발");
member.getFavoriteFoods().add("피자");

member.getAddressHistory().add(new Address("old1", "street1", "10001"));
member.getAddressHistory().add(new Address("old2", "street2", "10002"));

em.persist(member);

tx.commit();
```
<!-- hibernate 쿼리 -->
- 먼저 Member가 저장되고 값 타입 컬렉션을 저장하는 별도의 테이블에 대한 INSERT Query가 6개 나갔다.
    - 값 타입 컬렉션(다른 테이블)에 대한 persist를 하지 않았는데 쿼리가 나갔다.
    - 즉, Member 객체의 라이프 사이클과 동일하게 적용되었다.
- **Why?**
    - 값 타입이기 때문이다.
    - Member에 소속된 값 타입들의 라이프 사이클은 Member에 의존한다. (별도의 생명주기가 없음)
    - 즉, 값 타입은 별도로 persist 또는 update를 할 필요가 없이 Member에서 값을 변경만 하면 자동으로 처리해준다.
    - 1:N 연관 관계에서 `Cascade=ALL`로 설정하고, `orphanRemoval=true` 로 설정한 것과 유사하다.
- c.f. 값 타입 컬렉션은 영속성 전이(Cascade) + 고아 객체 제거 기능을 필수로 가진다고 볼 수 있다.


#### 컬렉션 값 타입 조회
```java
// ... 위와 동일한 코드 

em.flush();
em.clear();

System.out.println("============ START ============");
Member findMember = em.find(Member.class, member.getId());
// em.persist(member);

tx.commit();
```

- `em.flush()`, `em.clear()`로 영속성 컨텍스트를 비운 후 Member를 조회한다.
- 결과
    - Member에 소속된 Embedded 타입의 Address 속성은 모두 같이 조회된다.
    - 그러나 컬렉션들은 조회 되지 않는다. 
- 즉, **컬렉션 값 타입들은 지연 로딩(Lazy Loading) 전략을 취한다.**
    ```java
    System.out.println("============ START ============");
    Member findMember = em.find(Member.class, member.getId());
    // 
    List<Address> addressHistory = findMember.getAddressHistory();
    for (Address address : addressHistory) {
        System.out.println("address = " + address.getCity());
    }
    // 
    Set<String> favoriteFoods = findMember.getFavoriteFoods();
    for (String favoriteFood : favoriteFoods) {
        System.out.println("favoriteFood = " + favoriteFood);
    }
    ```
    - **Why?** `@ElementCollection(fetch = LAZY)` 어노테이션의 fetch 기본값이 LAZY 이다.

    
#### 컬렉션 값 타입 수정
- 값 타입 수정에 대한 기본 개념 
    ```java
    System.out.println("============ START ============");
    Member findMember = em.find(Member.class, member.getId());

    // homeCity -> newCity 
    // findMember.getHomeAddress().setCity("newCity"); // 틀린 방법 

    Address address = findMember.getHomeAddress();
    findMember.setHomeAddress(new Address("newCity", address.getStreet(), address.getZipCode())); // 새로 생성 

    tx.commit();
    ```
    - 값 타입은 불변(Immutable)이어야 한다.
    - 따라서, 수정하고 싶은 임베디드 타입의 속성이 있는 경우 새로운 인스턴스를 생성하여 **통으로 갈아 끼워야 한다.**
        - e.g. 새로운 Address 인스턴스로 변경 

1. **컬렉션 값** 타입 수정 예시1 - `Set<String>` 수정 
    ```java
    System.out.println("============ START ============");
    Member findMember = em.find(Member.class, member.getId());
    // 치킨 -> 한식 
    findMember.getFavoriteFoods().remove("치킨");
    findMember.getFavoriteFoods().add("한식");
    ```
    - String은 불변 객체 이므로 삭제하고, 다시 리스트에 넣어준다.
    - String 자체가 값 타입이므로 업데이트를 할 수가 없다. 위와 마찬가지로 통으로 갈아 끼워야 한다.
    - 컬렉션의 값만 변경해도 JPA가 변경 사항을 알아 내서 실제 DB에 Query를 날린다. 
        - (영속성 전이가 되는 것처럼)
    - 컬렉션은 Member 소속의 단순한 값이기 때문에 Member에 모든 생명 주기를 맡긴다.

2. **컬렉션 값** 타입 수정 예시2 - `List<Address>` 수정 
    ```java
    System.out.println("============ START ============");
    Member findMember = em.find(Member.class, member.getId());
    // old1 -> newCity1
    findMember.getAddressHistory().remove(new Address("old1", "street1", "10001")); // equals 로 비교 
    findMember.getAddressHistory().add(new Address("newCity1", "street1", "10001"));
    ```
    - `equals()`, `hashCode()` 에 대한 제대로 된 재정의가 필요하다.
    - AddressHistory 테이블에서 Member 에 소속된 Address 를 모두 지운다.
        - 테이블에 있는 데이터를 갈아 끼운다.
        - old2, newCity1 에 대한 Address 를 새로 INSERT 한다. 
    - 아래 *제약사항 참고!*

### 값 타입 컬렉션의 제약사항
- 값 타입 컬렉션은 값 타입은 엔티티와 다르게 식별자 개념이 없다.
- 값은 변경하면 추적이 어렵다.
    ```sql
    Hibernate: 
        create table ADDRESS (
            MEMBER_ID bigint not null,
            city varchar(255),
            street varchar(255),
            zipcode varchar(255)
        )
    ```
    - ADDRESS 에는 id가 존재하지 않는다. 
    - 그렇기 때문에 값이 중간에 변경되었을 때 DB가 해당 row만을 찾아서 변경할 수 없다.
- 값 타입 컬렉션에 변경 사항이 발생하면, 주인 엔티티와 연관된 모든 데이터를 삭제하고 값 타입 컬렉션에 있는 현재 값을 모두 다시 저장한다. **(중요!)**
    - 대안
        - `@OrderColumn(name = "address_history_order")`를 사용하여 UPDATE Query가 날라갈 수 있도록 할 수 있다. 
        - 그러나, 의도한 대로 동작하지 않을 때가 많다.
    - 결론
        - **값 타입 컬렉션을 사용하지 말자.**
        - 대부분이 엔티티이다.
        - e.g. 주소
    - **Q.** 그럼 값 타입 컬렉션은 언제 사용?
        - **A.** 정말 단순한 경우! 추적할 필요도 없고 값이 바뀌어도 업데이트를 할 필요가 없을 때 
        - e.g. select box 에서 [치킨, 피자, ...] 중 여러 메뉴를 선택할 때
- 값 타입 컬렉션을 매핑하는 테이블은 모든 컬럼을 묶어서 기본 키를 구성해야 한다.
    - null 입력 X
    - 중복 저장 X 

### 값 타입 컬렉션 대안
- 실무에서는 상황에 따라 값 타입 컬렉션 대신에 **일대다 관계를 고려**하는 것이 낫다.
    - 일대다 관계를 위한 엔티티를 만들고, 여기에서 값 타입을 사용하자.
- 영속성 전이(Cascade) + 고아 객체 제거를 사용해서 값 타입 컬렉션처럼 사용하자.
    - `@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)`
    - 실무에서 쿼리 최적화에도 유리하다.

#### 대안에 대한 예시 코드 
```java
@Entity
public class Member {
    ...
    @ElementCollection
    @CollectionTable(
        name = "FAVORITE_FOOD",
        joinColumns = @JoinColumn(name = "MEMBER_ID"))
    @Column(name = "FOOD_NAME")
    private Set<String> favoriteFoods = new HashSet<>();

    // 변경 
    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
    @JoinColumn(name = "MEMBER_ID")
    private List<AddressEntity> addressHistory = new ArrayList<>();
    ...
}
```
```java
@Entity
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@Table(name = "ADDRESS")
public class AddressEntity {

    @Id @GeneratedValue
    private Long id;

    private Address address; // 값 타입 
}
```
```java
Member member = new Member();
member.setUsername("member1");
member.setHomeAddress(new Address("homeCity", "street", "10000"));

member.getAddressHistory().add(new AddressEntity("old1", "street1", "10001"));
member.getAddressHistory().add(new AddressEntity("old2", "street2", "10002"));

em.persist(member);

em.flush();
em.clear();
```

- 과정 
    - Member 클래스에서 List<Address> 를 List<AddressHistory> 엔티티로 대체한다.
        - @OneToMany와 @JoinColumn 으로 1:N 단방향 매핑을 한다. 
        - 영속성 전이(Cascade) + 고아 객체 제거
    - AddressEntity 클래스에서 내부적으로 Address 값 타입을 포함한다.
- 개념 
    - ADDRESS 테이블에 ID 라는 개념이 생겼다. 또한 MEMBER_ID를 FK로 가진다.
        - 즉, 식별자가 있다는 것은 ADDRESS는 엔티티라는 것이고, 값을 가져와서 마음대로 수정할 수 있다.
    - **값 타입을 엔티티로 승급화**한다.
        - 실제 실무에서 많이 사용 
    - c.f. 1:N 단방향에서 ADDRESS 테이블에 UPDATE Query가 나가는 것은 어쩔 수 없다.
        - Why? 다른 테이블 (Member)에 외래키가 있기 때문에 
        - UPDATE Query를 없애려면 1:N, N:1 양방향으로 변경 


## 정리
- 엔티티 타입의 특징
    - 식별자가 있다.
    - 생명 주기를 관리 한다.
    - 공유할 수 있다.
- 값 타입의 특징
    - 식별자가 없다.
    - 생명 주기를 엔티티에 의존한다. (내가 제어하지 못함)
    - 공유하지 않는 것이 안전하다. (복사해서 사용)
    - 불변 객체로 만드는 것이 안전하다. (어쩔 수 없이 공유되더라도 불변으로 만든다.)
- 값 타입은 정말 값 타입이라 판단될 때만 사용하자.
- 엔티티와 값 타입을 혼동해서 엔티티를 값 타입으로 만들면 안된다.
- 식별자가 필요하고, 지속해서 값을 추적하고 변경해야 한다면 그것은 값 타입이 아닌 엔티티이다.

---

# 관련된 Post
* JDBC, JPA/Hibernate, Mybatis의 차이에 대해 알고 싶으시면 [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)를 참고하시기 바랍니다.
* ORM의 개념에 대해 알고 싶으시면 [ORM이란](https://gmlwjd9405.github.io/2019/02/01/orm.html)을 참고하시기 바랍니다.


# Reference
> - [인프런 강의 참고](https://www.inflearn.com/course/ORM-JPA-Basic#)
