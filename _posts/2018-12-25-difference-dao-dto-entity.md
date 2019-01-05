---
layout: post
title: '[DAO] DAO, DTO, Entity Class의 차이'
subtitle: 'DAO, DTO, Entity Class의 차이를 이해하다.'
date: 2018-12-25
author: heejeong Kwon
cover: '/images/spring-framework/spring-dao-dto-main.png'
tags: spring dao dto entity domain
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - DAO(Data Access Object)란 무엇인지 이해한다.
> - DTO(Data Transfer Object)란 무엇인지 이해한다.
> - Entity Class란 무엇인지 이해한다.
> - package 구조에 따른 흐름, 해당 package의 역할 및 기능을 이해한다.

## DAO(Data Access Object) 란?
**repository package**
* 실제로 DB에 접근하는 객체이다.
  * Persistence Layer(DB에 data를 CRUD하는 계층)이다.
* Service와 DB를 연결하는 고리의 역할을 한다.
* SQL를 사용(개발자가 직접 코딩)하여 DB에 접근한 후 적절한 CRUD API를 제공한다.
  * JPA 대부분의 기본적인 CRUD method를 제공하고 있다.
  * `extends JpaRepository<User, Long>`
* 예시(JPA 사용 시)
```java
public interface QuestionRepository extends CrudRepository<Question, Long> {
}
```

## DTO(Data Transfer Object) 란?
**dto package**
* 계층간 데이터 교환을 위한 객체(Java Beans)이다.
  * DB에서 데이터를 얻어 Service나 Controller 등으터 보낼 때 사용하는 객체를 말한다.
  * 즉, DB의 데이터가 Presentation Logic Tier로 넘어오게 될 때는 DTO의 모습으로 바껴서 오고가는 것이다.
  * *로직을 갖고 있지 않는* 순수한 데이터 객체이며, getter/setter 메서드만을 갖는다.
  * 하지만 DB에서 꺼낸 값을 임의로 변경할 필요가 없기 때문에 DTO클래스에는 setter가 없다. (대신 생성자에서 값을 할당한다.)
* Request와 Response용 DTO는 View를 위한 클래스
  * 자주 변경이 필요한 클래스
  * Presentation Model
  * `toEntity()` 메서드를 통해서 DTO에서 필요한 부분을 이용하여 Entity로 만든다.
  * 또한 Controller Layer에서 Response DTO 형태로 Client에 전달한다.
* <mark>참고</mark> VO(Value Object) vs DTO
  * VO는 DTO와 동일한 개념이지만 read only 속성을 갖는다.
  * VO는 특정한 비즈니스 값을 담는 객체이고, DTO는 Layer간의 통신 용도로 오고가는 객체를 말한다.
* 예시
```java
@Getter
@NoArgsConstructor
@AllArgsConstructor
public class UserDto {
    @NotBlank
    @Pattern(regexp = "^([\\w-]+(?:\\.[\\w-]+)*)@((?:[\\w-]+\\.)*\\w[\\w-]{0,66})\\.([a-z]{2,6}(?:\\.[a-z]{2})?)$")
    private String email;

    @JsonIgnore
    @NotBlank
    @Size(min = 4, max = 15)
    private String password;

    @NotBlank
    @Size(min = 6, max = 10)
    private String name;

    public User toEntity() {
        return new User(email, password, name);
    }

    public User toEntityWithPasswordEncode(PasswordEncoder bCryptPasswordEncoder) {
        return new User(email, bCryptPasswordEncoder.encode(password), name);
    }
}
```

## Entity Class란
**domain package**
* 실제 DB의 테이블과 매칭될 클래스 
  * 즉, 테이블과 링크될 클래스임을 나타낸다.
  * Entity 클래스 또는 가장 Core한 클래스라고 부른다.
  * `@Entity`, `@Column`, `@Id` 등을 이용
* 최대한 외부에서 Entity 클래스의 getter method를 사용하지 않도록 해당 클래스 안에서 필요한 *로직 method을 구현한다.*
  * 단, *Domain Logic*만 가지고 있어야 하고 Presentation Logic을 가지고 있어서는 안된다.
  * 여기서 구현한 method는 주로 Service Layer에서 사용한다.
* <mark>참고</mark> Entity 클래스와 DTO 클래스를 분리하는 이유
  * View Layer와 DB Layer의 역할을 철저하게 분리하기 위해서
  * 테이블과 매핑되는 Entity 클래스가 변경되면 여러 클래스에 영향을 끼치게 되는 반면 View와 통신하는 DTO 클래스(Request / Response 클래스)는 자주 변경되므로 분리해야 한다.
  * Domain Model을 아무리 잘 설계했다고 해도 각 View 내에서 Domain Model의 getter만을 이용해서 원하는 정보를 표시하기가 어려운 경우가 종종 있다. 이런 경우 Domain Model 내에 Presentation을 위한 필드나 로직을 추가하게 되는데, 이러한 방식이 모델링의 순수성을 깨고 Domain Model 객체를 망가뜨리게 된다.
  * 또한 Domain Model을 복잡하게 조합한 형태의 Presentation 요구사항들이 있기 때문에 Domain Model을 직접 사용하는 것은 어렵다.
  * 즉 DTO는 Domain Model을 복사한 형태로, 다양한 Presentation Logic을 추가한 정도로 사용하며 Domain Model 객체는 Persistent만을 위해서 사용한다.
  
 
* 예시
```java
@Entity
@Getter
@AllArgsConstructor
@NoArgsConstructor
@EqualsAndHashCode
@ToString
public class User implements Serializable {
    private static final long serialVersionUID = 7342736640368461848L;

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @JsonProperty
    private Long id;

    @Column(nullable = false)
    @JsonProperty
    private String email;

    @Column(nullable = false)
    @JsonIgnore
    private String password;

    // @Override 
    // public boolean equals(Object o) { ... }
    // @Override
    // public int hashCode() { ... }
    // @Override
    // public String toString() { ... }
```

---

## 전체 구조 (package 기준)
![](/images/spring-framework/spring-package-flow.png)

### controller(web)
* 기능
  * 해당 요청 url에 따라 적절한 view와 mapping 처리
  * `@Autowired Service`를 통해 service의 method를 이용
  * 적절한 ResponseEntity(DTO)를 body에 담아 Client에 반환
* 예시 1) 
```java
@Controller
@RequestMapping("/")
public class HomeController {
    @GetMapping
    public String home(HttpSession session) {
        if (!SessionUtil.getUser(session).isPresent()) {
            return "login";
        }
        return "index";
    }
}
```
  * @Controller
    * API와 view를 동시에 사용하는 경우에 사용
    * 대신 API 서비스로 사용하는 경우는 @ResponseBody를 사용하여 객체를 반환한다.
    * view(화면) return이 주목적
* 예시 2) 
```java
@RestController
@RequestMapping("/api/users")
public class ApiUserController {
    @Autowired
    private UserService userService;

    @PostMapping("/login")
    public ResponseEntity login(@RequestBody @Valid LoginDto loginDto, HttpSession session) {
        SessionUtil.setUser(session, userService.login(loginDto));
        return new ResponseEntity(HttpStatus.OK);
    }
}
```
  * @RestController
      * view가 필요없는 API만 지원하는 서비스에서 사용 (Spring 4.0.1부터 제공)
      * @RequestMapping 메서드가 기본적으로 @ResponseBody 의미를 가정한다.
      * data(json, xml 등) return이 주목적: **return ResponseEntity**
      * 즉, @RestController = @Controller + @ResponseBody

### service
* 기능
  * `@Autowired Repository`를 통해 repository의 method를 이용
  * 적절한 Business Logic을 처리한다.
  * DAO로 DB에 접근하고 DTO로 데이터를 전달받은 다음, 비지니스 로직을 처리해 적절한 데이터를 반환한다.
* 예시
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    @Resource(name = "bCryptPasswordEncoder")
    private PasswordEncoder bCryptPasswordEncoder;
    @Autowired
    private MessageSourceAccessor msa;

    public User save(UserDto userDto) {
        if (isExistUser(userDto.getEmail())) {
            throw new UserDuplicatedException(msa.getMessage("email.duplicate.message"));
        }
        return userRepository.save(userDto.toEntityWithPasswordEncode(bCryptPasswordEncoder);
    }
}
```

### repository(dao)
* 기능
  * 실제로 DB에 접근하는 객체이다.
  * Service와 DB를 연결하는 고리의 역할을 한다.
  * SQL를 사용(개발자가 직접 코딩)하여 DB에 접근한 후 적절한 CRUD API를 제공한다.
    * JPA 대부분의 기본적인 CRUD method를 제공하고 있다.
    * `extends JpaRepository<User, Long>`
* 예시 (JPA의 경우)
```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```
```java
public interface QuestionRepository extends CrudRepository<Question, Long> {
}
```

### dto(DTO Class)와 domain(Entity Class)
* 구체적인 내용과 예시는 위의 설명 참고


# References
> - [http://toby.epril.com/?p=99](http://toby.epril.com/?p=99)
> - [http://lazymankook.tistory.com/30](http://lazymankook.tistory.com/30)

