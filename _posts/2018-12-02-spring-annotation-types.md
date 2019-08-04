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
<span style="background-color: #e1e1e1">계속해서 추가할 예정!</span>  

---

### @Component
* component-scan을 선언에 의해 특정 패키지 안의 **클래스들을 스캔하고, @Component Annotation이 있는 클래스에 대하여 bean 인스턴스를 생성**한다.

### @Controller, @Service, @Repository
* @Component ---구체화---> @Controller, @Service, @Repository
* bean으로 등록
* 해당 클래스가 Controller/Service/Repository로 사용됨을 Spring Framework에 알린다.
* 참고) [@WebServlet vs @Controller](https://gmlwjd9405.github.io/2018/12/22/webservlet-vs-controller.html)

### @RequestMapping
```java
@Controller
@RequestMapping("/home") // 1) Class Level
public class HomeController {
    /* an HTTP GET for /home */ 
    @RequestMapping(method = RequestMethod.GET) // 2) Handler Level
    public String getAllEmployees(Model model) {
        ...
    }
    /* an HTTP POST for /home/employees */ 
    @RequestMapping(value = "/employees", method = RequestMethod.POST) 
    public String addEmployee(Employee employee) {
        ...
    }
}
```
* @RequestMapping에 대한 모든 매핑 정보는 Spring에서 제공하는 HandlerMapping Class가 가지고 있다.
* 1) Class Level Mapping
    * 모든 메서드에 적용되는 경우
    * "/home"로 들어오는 모든 요청에 대한 처리를 해당 클래스에서 한다는 것을 의미한다.
* 2) Handler Level Mapping
    * 요청 url에 대해 해당 메서드에서 처리해야 되는 경우
    * "/home/employees" POST 요청에 대한 처리를 addEmployee()에서 한다는 것을 의미한다.
* **value**: 해당 url로 요청이 들어오면 이 메서드가 수행된다.
* **method**: 요청 method를 명시한다. 없으면 모든 http method 형식에 대해 수행된다.

### @RestController
* @Controller + @ResponseBody
* @ResponseBody를 모든 메소드에서 적용한다. 
    * 메소드의 반환 결과(문자열)를 JSON 형태로 반환한다.
* @Controller 와 @RestController 의 차이
    * @Controller
        * API와 view를 동시에 사용하는 경우에 사용
        * 대신 API 서비스로 사용하는 경우는 @ResponseBody를 사용하여 객체를 반환한다.
        * view(화면) return이 주목적
    * @RestController
        * view가 필요없는 API만 지원하는 서비스에서 사용 (Spring 4.0.1부터 제공)
        * @RequestMapping 메서드가 기본적으로 @ResponseBody 의미를 가정한다.
        * data(json, xml 등) return이 주목적
        * 즉, @RestController = @Controller + @ResponseBody

### @Required
* setter method에 사용한다.
* 영향을 받는 bean property 구성 시 XML 설정 파일에 반드시 property를 채워야 한다. (엄격한 체크)
    * 그렇지 않으면 BeanInitializationException 예외를 발생
* 예시 

```xml
<!-- Definition for student bean -->
<bean id = "student" class = "com.tutorialspoint.Student">
    <property name = "name" value = "Zara" />
    <property name = "age"  value = "11"/>
</bean>
```
* [참고](https://www.tutorialspoint.com/spring/spring_required_annotation.htm)

### @Autowired
* `org.springframework.beans.factory.annotation.Autowired`
* Type에 따라 알아서 Bean을 주입한다.
* 필드, 생성자, 입력 파라미터가 여러 개인 메소드(@Qualifier는 메소드의 파라미터)에 적용 가능
* Type을 먼저 확인한 후 못 찾으면 Name에 따라 주입한다.
    * Name으로 강제하는 방법: @Qualifier을 같이 명시 
* 예시 
    * ![](/images/spring-framework/autowired-example.png)
* TIP) Bean을 주입받는 방식 (3가지)
    * @Autowired
    * setter
    * 생성자 (@AllArgsConstructor 사용) -> **권장방식**

### @Qualifier
* 같은 타입의 빈이 두 개 이상이 존재하는 경우에 스프링이 어떤 빈을 주입해야 할지 알 수 없어서 스프링 컨테이너를 초기화하는 과정에서 예외를 발생시킨다.
* 이 경우 @Qualifier을 @Autowired와 함께 사용하여 정확히 어떤 bean을 사용할지 지정하여 특정 의존 객체를 주입할 수 있도록 한다.
* 예시
    * ![](/images/spring-framework/qualifier-example.png)
    1. xml 설정에서 bean의 한정자 값(qualifier value)을 설정한다. 
    2. @Autowired 어노테이션이 적용된 주입 대상에 @Qualifier 어노테이션을 설정한다.

### @Resource
* `javax.annotation.Resource`
* 표준 자바(JSR-250 표준) Annotation으로, Spring Framework 2.5.* 부터 지원 가능한 Annotation이다. 
* Annotation 사용으로 인해 특정 Framework에 종속적인 어플리케이션을 구성하지 않기 위해서는 @Resource를 사용할 것을 권장한다. 
    * @Resource를 사용하기 위해서는 class path 내에 jsr250-api.jar 파일을 추가해야 한다.
* 필드, 입력 파라미터가 한 개인 bean property setter method에 적용 가능
* [](https://blog.outsider.ne.kr/777)

---
## [Data Validation]

### @Vaild
* `import javax.validation.Valid;`

* @Size(max=10, min=2, message="errMsg")
* @Email(message="errMsg")
* @NotEmpty(message="errMsg")

---

## [Configuration]

### @Configuration

### @EnableWebSecurity

### @SpringBootApplication

### @EnableWebMvc

---

### @RestControllerAdvice

### @ExceptionHandler

### @ResponseStatus

---

## [Parameter를 받는 방법]
### @RequestParam
* HTTP GET 요청에 대해 매칭되는 **request parameter** 값이 자동으로 들어간다.
    * url 뒤에 붙는 parameter 값을 가져올 때 사용한다.
    * Ex) ` http://localhost:8080/home?index=1&page=2`
* 
```java
@GetMapping("/home")
public String show(@RequestParam("page") int pageNum {
}
```
    * 위의 경우 GET /home?index=1&page=2와 같이 uri가 전달될 때 page parameter를 받아온다.
    * @RequestParam 어노테이션의 괄호 안의 문자열이 전달 인자 이름(실제 값을 표시)이다.

### @PathVariable
* HTTP 요청에 대해 매칭되는 **request parameter** 값이 자동으로 들어간다.
    * uri에서 각 구분자에 들어오는 값을 처리해야 할 때 사용한다.
    * Ex) ` http://localhost:8080/index/1`
    * REST API에서 값을 호출할 때 주로 많이 사용한다.
* 
```java
@PostMapping("/index/{idx}")
@ResponseBody
public boolean deletePost(@PathVariable("idx") int postNum) {
	return postService.deletePost(postNum);
}
```
    * 위의 경우 POST /index/{idx}와 같이 uri가 전달될 때 해당하는 구분자 {idx}를 받아온다.


<mark>참고</mark> @RequestParam와 @PathVariable 동시 사용 예제 
```java
@GetMapping("/user/{userId}/invoices")
public List<Invoice> listUsersInvoices(@PathVariable("userId") int user,
	                                  @RequestParam(value = "date", required = false) Date dateOrNull) {
}
```
* 위의 경우 GET /user/{userId}invoices?date=190101 와 같이 uri가 전달될 때 
* 구분자 {userId}는 @PathVariable("userId")로, 
* 뒤에 이어붙은 parameter는 @RequestParam("date")로 받아온다.

### @RequestBody
* 반드시 HTTP POST 요청에 대해서만 처리한다.
    * HTTP POST 요청에 대해 **request body**에 있는 request message에서 값을 얻어와 매칭한다.
* RequestData를 바로 Model이나 클래스로 매핑한다.
    * 이를테면 JSON 이나 XML같은 데이터를 적절한 messageConverter로 읽을 때 사용하거나 POJO 형태의 데이터 전체로 받는 경우에 사용한다.

### @ModelAttribute
* @RequestParam과 비슷하다.
    * form 값

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

### @Table
* 엔티티 클래스에 매핑할 테이블 정보를 알려준다.
    - Ex) `@Table(name = "USER")`
* 이 어노테이션을 생략하면 클래스 이름을 테이블 이름 정보로 매핑한다.

### @Entity
* 실제 DB의 테이블과 매칭될 클래스임을 명시한다.
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
    * @Column을 생략하면 필드명을 사용해서 컬럼명과 매핑하게 된다.
    - Ex) `@Column(name = "username")`
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

### @AllArgsConstructor 
* 모든 필드 값을 파라미터로 받는 생성자를 추가한다.

### @RequiredArgsConstructor 
* final이나 @NonNull인 필드 값만 파라미터로 받는 생성자를 추가한다.
    * final: 값이 할당되면 더 이상 변경할 수 없다.

### @Getter
* 클래스 내 모든 필드의 Getter 메소드를 자동으로 생성한다.

### @Setter
* Controller에서 @RequestBody로 외부에서 데이터를 받는 경우엔 기본생성자 + set메소드를 통해서만 값이 할당된다.
* 그래서 이때만 setter를 허용한다.

### @ToString
* `@ToString(exclude = "password")`
    * 특정 필드를 toString() 결과에서 제외한다.
* 클래스명(필드1명=필드1값, 필드2명=필드2값, ...) 식으로 출력된다.

### @EqualsAndHashCode
* equals와 hashCode 메소드 오버라이딩

```java
User user1 = new User();
user1.setId(1L);
user1.setUsername("user");
user1.setPassword("pass");

User user2 = new User();
user1.setId(2L); // 부모 클래스의 필드가 다름
user2.setUsername("user");
user2.setPassword("pass");

user1.equals(user2);
// callSuper = true 이면 false, callSuper = false 이면 true
```
* `@EqualsAndHashCode(callSuper = true)`
    * callSuper 속성을 통해 equals와 hashCode 메소드 자동 생성 시 부모 클래스의 필드까지 감안할지 안 할지에 대해서 설정할 수 있다.
    * 즉, callSuper = true로 설정하면 부모 클래스 필드 값들도 동일한지 체크하며, callSuper = false로 설정(기본값)하면 자신 클래스의 필드 값들만 고려한다.

### @Builder 
* 어느 필드에 어떤 값을 채워야 할지 명확하게 정하여 생성 시점에 값을 채워준다.
* TIP) 생성자와 빌더의 차이
    * 생성 시점에 값을 채워주는 역할은 똑같다.
    * 하지만 빌더를 사용하면 어느 필드에 어떤 값을 채워야 할지 명확하게 인지할 수 있다.
    * 해당 클래스의 빌더 패턴 클래스를 생성
* 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함된다.

### @Data
* @Getter @Setter @EqualsAndHashCode @AllArgsConstructor 을 포함한 Lombok에서 제공하는 필드와 관련된 모든 코드를 생성한다.

---

## [Json]
### @JsonManagedReference
### @JsonBackReference
### @JsonProperty
### @JsonIgnore

---

## [Java Config를 위한 Annotation]
[](https://www.slipp.net/wiki/pages/viewpage.action?pageId=22282242)
### @Configuration
* `import org.springframework.context.annotation.Configuration;`

### @EnableTransactionManagement

### @PropertySource("classpath:application-properties.xml")

---

## [Jackson Property Inclusion Annotation]
[](https://cheese10yun.github.io/jackson-annotation-03/)

### @JsonIgnoreProperties
- 무시할 속성이나 속성 목록을 표시할 때 사용한다.

### @JsonIgnore
- 필드 레벨에서 무시할 속성을 표시할 때 사용한다.

### @JsonIgnoreType


### @JsonInclude
- 어노테이션 속성을 제외할 때 사용한다.
- `@JsonInclude(JsonInclude.Include.NON_NULL)`
  - NON_NULL 사용 시 name이 null인 경우에 제외된다.
  
### @JsonAutoDetect
- `@JsonAutoDetect(fieldVisibility = JsonAutoDetect.Visibility.ANY)`

---

## [Spring AOP를 위한 Annotation]
[](https://jojoldu.tistory.com/71?category=635883)
### @EnableAspectJAutoProxy
* `import org.springframework.context.annotation.EnableAspectJAutoProxy;`
* aop

### @Aspect
* `import org.aspectj.lang.annotation.Aspect;`

```java
@Aspect
public class Logger {
    @Pointcut("execution( void spring.aop.*.sound())")
    private void selectSound() {}  // signature

    @Before("selectSound()")
    public void aboutToSound() {
        System.out.println("before advice: about to sound() ");
    }

    @After("selectSound()")
    public void afterSound() {
        System.out.println("after advice: after sound() ");
    }
}
```

### @PointCut
* execution
* xml에서의 id: method 이름 // signature 

### @Before (이전)
* 어드바이스 타겟 메소드가 호출되기 전에 어드바이스 기능을 수행

### @After (이후)
* 타겟 메소드의 결과에 관계없이(즉 성공, 예외 관계없이) 타겟 메소드가 완료 되면 어드바이스 기능을 수행

### @Around (메소드 실행 전후)
* 어드바이스가 타겟 메소드를 감싸서 타겟 메소드 호출전과 후에 어드바이스 기능을 수행

### @AfterReturning (정상적 반환 이후)
* 타겟 메소드가 성공적으로 결과값을 반환 후에 어드바이스 기능을 수행

### @AfterThrowing (예외 발생 이후)
* 타겟 메소드가 수행 중 예외를 던지게 되면 어드바이스 기능을 수행

--- 

# 관련된 Post
* Spring Annotation 활성화 방법에 대해 알고 싶으시면 [Spring Annotation 활성화](https://gmlwjd9405.github.io/2018/12/18/spring-annotation-enable.html)을 참고하시기 바랍니다.


# References
> - [https://jojoldu.tistory.com/255?category=635883](https://jojoldu.tistory.com/255?category=635883)
> - [https://jeong-pro.tistory.com/151](https://jeong-pro.tistory.com/151)
> - [component-scan 참고](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc)
> - [mvc:annotation-driven 참고](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/mvc.html#mvc-config)
> - [lombok annotation 참고](http://www.daleseo.com/lombok-popular-annotations/)
> - [https://cheese10yun.github.io/jackson-annotation-03/](https://cheese10yun.github.io/jackson-annotation-03/)
