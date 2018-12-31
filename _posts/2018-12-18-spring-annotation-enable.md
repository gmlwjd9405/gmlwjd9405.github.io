---
layout: post
title: '[Spring] Spring Annotation 활성화'
subtitle: 'annotation-driven, component-scan, annotation-config 차이'
date: 2018-12-18
author: heejeong Kwon
cover: '/images/spring-framework/spring-annotation-main.png'
tags: spring annotation
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - Annotation 기본 개념을 이해한다.
> - Annotation를 활성화할 수 있다.
> - annotation-driven, component-scan, annotation-config 차이를 이해한다.


## Annotation 기본 개념 
* xml 설정이 너무 길어짐에 따라 그 대안으로 나타났다.
* 클래스/메서드/필드에 Annotation을 달아 그 자체로 설정이 가능하도록 한다.
    * 단, xml이 우선순위가 더 높다.
* 기본적으로 활성화되지 않기 때문에 xml에 명시적인 활성화 작업이 필요하다.
    * IDE에서 지원한다. (체크하면 자동으로 추가)

## Annotation 활성화 
### Java config
```java
@Configuration
@EnableWebMvc
@ComponentScan("package-name")
public class WebConfig { ... }
```
    
### xml config
```xml
<!-- Annotation 활성화 -->
<mvc:annotation-driven></mvc:annotation-driven> 
<!-- Component 패키지 지정 -->
<context:component-scan base-package="package-name"></context:component-scan>
```

---

## annotation-driven, component-scan, annotation-config 차이
### mvc:annotation-driven
* 스프링 MVC 컴포넌트들을 그것의 디폴트 설정을 가지고 활성화하기 위해 사용된다. 
* 이 태그는 **Spring MVC가 @Controller에 요청을 보내기 위해 필요한 HandlerMapping과 HandlerAdapter를 bean으로 등록**한다.
    * 이렇게 등록된 bean에 의해 요청 url과 컨트롤러를 매칭할 수 있다.
    * 또한 컨트롤러(@Controller)에서는 @RequestMapping, @ExceptionHandler 등과 같은 주석을 통해 해당 기능을 사용할 수 있도록 한다.
    * 근본적으로 @Controller 없이는 이 태그는 아무것도 하지 않는다고 할 수 있다.
    * bean을 생성하기 위해 xml 파일에 context:component-scan을 명시하면 이 태그를 포함하지 않아도 MVC 애플리케이션은 작동한다.
* RequestMappingHandlerMapping?
    * 요청 url과 매칭되는 컨트롤러(@Controller)를 검색하는 역할. 즉, 요청 url을 보고 어떤 컨트롤러가 처리할지 결정한다.
* RequestMappingHandlerAdapter?
    * 컨트롤러의 실행 결과(요청을 처리한 결과)를 리턴하는 역할. Annotation 기반의 Controller 처리를 위해 반드시 필요하다.

### context:component-scan
* 특정 패키지 안의 **클래스들을 스캔하고, Annotation을 확인 후 bean 인스턴스를 생성**한다.
    * @Component @Controller @Service @Repository 등의 Annotation이 존재해야 bean을 생성할 수 있다.
* 이것의 장점 중 하나는 @Autowired와 @Qualifier Annotation을 인식할 수 있다.
* component-scan을 선언했다면 context:annotation-config를 선언할 필요가 없다.

### context:annotation-config
* ApplicationContext 안에 **이미 등록된 bean들의 Annotation을 활성화** 하기 위해 사용된다.
    * bean들이 XML로 등록됐는지 혹은 패키지 스캐닝을 통해 등록됐는지는 중요하지 않다.
* 이미 스프링 컨텍스트에 의해 생성되어 저장된 bean들에 대해서 @Autowired와 @Qualifier  Annotation을 인식할 수 있다.
* component-scan 또한 같은 일을 할 수 있는데, context:annotation-config는 bean을 등록하는 작업을 하지 않는다.
    * 즉, bean을 등록하기 위해 패키지를 안의 클래스를 스캔할 수 없다.
* 이 태그를 설정하면 @Required @Autowired @Resource @PostConstruct @PreDestroy @Configuration 기능을 각각 설정하지 않아도 된다.


<br><mark>참고</mark> tx:annotation-driven
* 등록된 빈 중에서 @Transactional이 붙은 클래스/인터페이스/메소드를 찾아 트랜잭션 어드바이스를 적용한다.

<br><mark>참고</mark> mvc:resources mapping
* 정적인 data files들의 위치 mapping 해준다.
    * Controller가 처리할 필요 없이 해당 위치의 디렉터리에서 바로 접근할 수 있다.
    * HTTP GET 요청에서의 정적인 data에 바로 매핑이 가능하다.
* `<mvc:resources mapping="/static/**" location="/static/" />` 
    * WebContent/webapp/static/(ex. image file, css, js, fonts)
    * web/static/(ex. image file, css, js, fonts)
* `<mvc:resources mapping="/resources/**" location="/resources/" />`
    * WebContent/webapp/resources/(ex. image file, css, js, fonts)
    * web/resources/(ex. image file, css, js, fonts)
    * jsp에서 사용하기 위해서 
        * `<link rel="stylesheet" type="text/css" href="/resources/css/main.css">` 추가 

https://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html
# 관련된 Post
* Spring Annotation 종류에 대해 알고 싶으시면 [Spring Annotation의 종류](https://gmlwjd9405.github.io/2018/12/02/spring-annotation-types.html)을 참고하시기 바랍니다.


# References
> - [component-scan 참고](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc)
> - [mvc:annotation-driven 참고](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/mvc.html#mvc-config)
> - [https://stackoverflow.com/questions/7414794/difference-between-contextannotation-config-vs-contextcomponent-scan](https://stackoverflow.com/questions/7414794/difference-between-contextannotation-config-vs-contextcomponent-scan)
> - [https://stackoverflow.com/questions/20551217/spring-support-for-controller-given-by-contextcomponent-scan-vs-mvcannot](https://stackoverflow.com/questions/20551217/spring-support-for-controller-given-by-contextcomponent-scan-vs-mvcannot)