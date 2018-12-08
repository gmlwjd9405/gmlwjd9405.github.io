---
layout: post
title: '[Spring] Spring Annotation의 종류와 그 역할'
subtitle: 'Spring Annatation의 종류와 그 역할에 대해 이해한다.'
date: 2018-12-02
author: heejeong Kwon
cover: '/images/spring-framework/spring-annotation-main.png'
tags: spring annotation
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---
<span style="color:#4d0000">계속해서 추가할 예정!</span>  

### @Component

### @Controller

### @RestController
* @Controller + @ResponseBody
* @ResponseBody를 모든 메소드에서 적용한다. 
    * 메소드의 반환 결과(문자열)를 JSON 형태로 반환한다.

### @Autowired
* Bean을 주입한다.
* TIP) Bean을 주입받는 방식 (3가지)
    * @Autowired
    * setter
    * 생성자 (@AllArgsConstructor 사용) -> **권장방식**

### @RequestBody
* RequestData를 바로 Model이나 클래스로 매핑한다.

### @RequestParam

### @PathVariable 

--- 

## [공용 생성일/수정일을 위한 Annotation]
### @EnableJpaAuditing 
* JPA Auditing을 활성화한다.

### @MappedSuperclass
* JPA Entity 클래스들이 BaseTimeEntity을 상속할 경우 필드들(createdDate, modifiedDate)도 컬럼으로 인식하도록 한다.

### @EntityListeners(AuditingEntityListener.class)
* BaseTimeEntity 클래스에 Auditing 기능을 포함한다.

### @CreatedDate
* Entity가 생성되어 저장될 때 시간이 자동으로 저장된다.

### @LastModifiedDate
* 조회한 Entity의 값을 변경할 때 시간이 자동으로 저장된다.

### @Transactional
* 메소드 내에서 Exception이 발생하면 해당 메소드에서 이루어진 모든 DB 작업을 초기화한다.
    * 즉, save 메소드를 통해서 10개를 등록해야 하는데 5번째에서 Exception이 발생하면 앞에 저장된 4개 까지 모두 롤백한다.
    * (정확히 얘기하면, 이미 넣은걸 롤백시키는건 아니며, 모든 처리가 정상적으로 됐을때만 DB에 커밋하며 그렇지 않은 경우엔 커밋하지 않는 것이다.) 
* 비지니스 로직과 트랜잭션 관리는 모두 Service에서 관리한다. 
    * 따라서 일반적으로 DB 데이터를 등록/수정/삭제 하는 Service 메소드는 @Transactional를 필수적으로 가져간다.

--- 

## [JPA에서 제공하는 Annotation]
* JPA를 사용하면 DB 데이터에 작업할 경우 실제 쿼리를 날리지 않고 Entity 클래스의 수정을 통해 작업한다.

### @Entity
* 실제 DB의 테이블과 매칭될 클래스임을 명시한다ㅏ.
* 즉, 테이블과 링크될 클래스임을 나타냅니다. 
    * Entity Class
    * 가장 Core한 클래스
* 클래스 이름을 언더스코어 네이밍(_)으로 테이블 이름을 매칭한다.
    * Ex) SalesManage스.java -> sales_manager table
* TIP) Controller에서 쓸 DTO 클래스란
    * Request와 Response용 DTO는 View를 위한 클래스로, 자주 변경이 필요한 클래스이다.
    * Entity 클래스와 DTO 클래스를 분리하는 이유
        * View Layer와 DB Layer를 철저하게 역할 분리를 하는게 좋다.
        * 테이블과 매핑되는 Entity 클래스가 변경되면 여러 클래스에 영향을 끼치게 되는 반면 View와 통신하는 DTO 클래스(Request/ Response 클래스)는 자주 변경되므로 분리해야 한다.

### @Id
* 해당 테이블의 PK 필드를 나타낸다.

### @GeneratedValue
* PK의 생성 규칙을 나타낸다.
    * TIP) 가능한 Entity의 PK는 Long 타입의 Auto_increment를 추천한다.
* 기본값은 AUTO로, MySQL의 auto_increment와 같이 자동 증가하는 정수형 값이 된다.
    * 스프링 부트 2.0에선 옵션을 추가하셔야만 auto_increment가 된다.
    * [참고](https://jojoldu.tistory.com/295)

### @Column
* 테이블의 컬럼을 나타내면, 굳이 선언하지 않더라도 해당 클래스의 필드는 모두 컬럼이 된다.
* 이 Annotation을 사용하는 이유는, 기본값 외에 추가로 변경이 필요한 옵션이 있을 경우 사용한다.
* Ex) 문자열의 경우 VARCHAR(255)가 기본값인데, 사이즈를 500으로 늘리고 싶거나(ex: title), 타입을 TEXT로 변경하고 싶거나(ex: content) 등의 경우에 사용된다.

--- 

## [Lombok Library Annotation]

### @NoArgsConstructor
* 기본생성자를 자동으로 추가한다.
* access = AccessLevel.PROTECTED 
    * 기본생성자의 접근 권한을 protected로 제한
    * 생성자로 protected Posts() {}와 같은 효과
    * Entity 클래스를 프로젝트 코드상에서 기본생성자로 생성하는 것은 막되, JPA에서 Entity 클래스를 생성하는것은 허용하기 위해 추가한다.

### @Getter
* 클래스 내 모든 필드의 Getter 메소드를 자동으로 생성한다.

### @Setter
* Controller에서 @RequestBody로 외부에서 데이터를 받는 경우엔 기본생성자 + set메소드를 통해서만 값이 할당된다.
* 그래서 이때만 setter를 허용한다.

### @Builder 
* 어느 필드에 어떤 값을 채워야 할지 명확하게 정하여 생성 시점에 값을 채워준다.
* TIP) 생성자와 빌더의 차이
    * 생성 시점에 값을 채워주는 역할은 똑같다.
    * 하지만 빌더를 사용하면 어느 필드에 어떤 값을 채워야 할지 명확하게 인지할 수 있다.
    * 해당 클래스의 빌더 패턴 클래스를 생성
* 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함된다.

--- 

# 관련된 Post
* Spring Annotation의 개념에 대해 알고 싶으시면 [Spring Annotation의 개념](https://gmlwjd9405.github.io/2018/12/01/spring-accnotation-concept.html)을 참고하시기 바랍니다.


# References
> - [https://jojoldu.tistory.com/255?category=635883](https://jojoldu.tistory.com/255?category=635883)
> - [https://jeong-pro.tistory.com/151](https://jeong-pro.tistory.com/151)