---
layout: post
title: '[Spring] Spring Bean의 개념과 Bean Scope 종류'
subtitle: 'Spring Bean에 대해 이해한다.'
date: 2018-11-10
author: heejeong Kwon
cover: '/images/spring-framework/spring-framework-main.png'
tags: spring bean scope
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Spring Bean의 개념을 이해한다.
> - Spring Scope의 종류를 확인한다.
> - singleton, prototype scope의 개념을 이해한다.
> - 구체적인 예시를 확인한다. 

## Spring Bean이란
![](/images/spring-framework/spring-bean.png)
* Spring에서 **POJO**(plain, old java object)를 'Beans'라고 부른다.
* Beans는 애플리케이션의 핵심을 이루는 객체이며, Spring IoC(Inversion of Control) 컨테이너에 의해 인스턴스화, 관리, 생성된다.
* Beans는 우리가 컨테이너에 공급하는 **설정 메타 데이터**(XML 파일)에 의해 생성된다.
    * 컨테이너는 이 메타 데이터를 통해 Bean의 생성, Bean Life Cycle, Bean Dependency(종속성) 등을 알 수 있다.
* 애플리케이션의 객체가 지정되면, 해당 객체는 getBean() 메서드를 통해 가져올 수 있다.

### Spring Bean 정의
* 일반적으로 XML 파일에 정의한다.
* 주요 속성
    * class(필수): 정규화된 자바 클래스 이름
    * id: bean의 고유 식별자
    * scope: 객체의 범위 (sigleton, prototype)
    * constructor-arg: 생성 시 생성자에 전달할 인수
    * property: 생성 시 bean setter에 전달할 인수
    * init method와 destroy method

### XML based configuration file

```xml
<!-- A simple bean definition -->
<bean id="..." class="..."></bean>

<!-- A bean definition with scope-->
<bean id="..." class="..." scope="singleton"></bean>

<!-- A bean definition with property -->
<bean id="..." class="...">
	<property name="message" value="Hello World!"/>
</bean>

<!-- A bean definition with initialization method -->
<bean id="..." class="..." init-method="..."></bean>
```

## Spring Bean Scope
* 스프링은 기본적으로 모든 bean을 **singleton**으로 생성하여 관리한다.
    * 구체적으로는 애플리케이션 구동 시 JVM 안에서 스프링이 bean마다 하나의 객체를 생성하는 것을 의미한다.
    * 그래서 우리는 스프링을 통해서 bean을 제공받으면 언제나 주입받은 bean은 동일한 객체라는 가정하에서 개발을 한다.
* request, session, global session의 Scope는 일반 Spring 어플리케이션이 아닌, Spring MVC Web Application에서만 사용된다.
* [참고](https://docs.spring.io/spring/docs/4.2.5.RELEASE/spring-framework-reference/html/beans.html#beans-factory-scopes)
    * ![](/images/spring-framework/spring-bean-scope.png)

### 1. Singleton
* 'singleton' bean은 Spring 컨테이너에서 **한 번** 생성된다. 
    * 컨테이너가 사라질 때 bean도 제거된다.
* 생성된 하나의 인스턴스는 single beans cache에 저장되고, 해당 bean에 대한 요청과 참조가 있으면 캐시된 객체를 반환한다.
    * 즉, 하나만 생성되기 때문에 동일한 것을 참조한다.
* 기본적으로 모든 bean은 scope이 명시적으로 지정되지 않으면 singleton이다.

![](/images/spring-framework/spring-bean-singleton.png) 

* xml 설정
    * `<bean id="..." class="..." scope="singleton"></bean>`
* annotation 설정
    * 대상 클래스에 `@Scope("singletone")`


### 2. Prototype
* 'prototype' bean은 모든 요청에서 **새로운** 객체를 생성하는 것을 의미한다.
    * 즉, prototype bean은 의존성 관계의 bean에 주입 될 때 새로운 객체가 생성되어 주입된다.
    * 정상적인 방식으로 gc에 의해 bean이 제거된다.

![](/images/spring-framework/spring-bean-prototype.png) 

* xml 설정
    * `<bean id="..." class="..." scope="prototype"></bean>`
* annotation 설정
    * 대상 클래스에 `@Scope("prototype")`

<mark>참고</mark>
* 싱글톤으로 적합한 객체 
1. 상태가 없는 공유 객체: 상태를 가지고 있지 않은 객체는 동기화 비용이 없다. 따라서 매번 이 객체를 참조하는 곳에서 새로운 객체를 생성할 이유가 없다.
2. 읽기용으로만 상태를 가진 공유 객체: 1번과 유사하게 상태를 가지고 있으나 읽기 전용이므로 여전히 동기화 비용이 들지 않는다. 매 요청마다 새로운 객체 생성할 필요가 없다. 
3. 공유가 필요한 상태를 지닌 공유 객체: 객체 간의 반드시 공유해야 할 상태를 지닌 객체가 하나 있다면,  이 경우에는 해당 상태의 쓰기를 가능한 동기화 할 경우 싱글톤도 적합하다.
4. 쓰기가 가능한 상태를 지니면서도 사용빈도가 매우 높은 객체: 애플리케이션 안에서 정말로 사용빈도가 높다면, 쓰기 접근에 대한 동기화 비용을 감안하고서라도 싱글톤을 고려할만하다. 이 방법은 1. 장시간에 걸쳐 매우 많은 객체가 생성될 때, 2. 해당 객체가 매우 작은 양의 쓰기상태를 가지고 있을 때, 3. 객체 생성비용이 매우 클 때에 유용한 선택이 될 수 있다.
* 비싱글톤으로 적합한 객체
1. 쓰기가 가능한 상태를 지닌 객체: 쓰기가 가능한 상태가 많아서 동기화 비용이 객체 생성 비용보다 크다면 싱글톤으로 적합하지 않다.
2. 상태가 노출되지 않은 객체: 일부 제한적인 경우, 내부 상태를 외부에 노출하지 않는 빈을 참조하여 다른 의존객체와는 독립적으로 작업을 수행하는 의존 객체가 있다면 싱글톤보다 비싱글톤 객체를 사용하는 것이 더 나을 수 있다.

### Example
* PetOwner Class

```java
package com.spring;

public class PetOwner {
    String userName;
    public AnimalType animal;

    public PerOwner(AnimalType animal) { this.animal = animal; }
    
    public String getUserName() {
        System.out.println("Person name is " + , userName);
        return userName;
    }
    public void setUserName(String userName) { this.userName = userName; }
    
    public void play() { animal.sound(); }
}
```

* MainApp Class

```java
package com.spring;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainApp {
    public static void main(String[] args) {
        /* main함수에서 Contaier를 생성 */ 
        // 설정 파일은 인자로 넣고, 해당 설정 파일에 맞게 bean들을 만든다.
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("com/spring/beans/bean.xml");

        // getBean()을 통해 bean의 주소값을 가져온다.  
        PetOwner person1 = (PerOwner) context.getBean("petOwner");
        person1.setUserName("Alice");
        person1.getUserName();

        PetOwner person2 = (PerOwner) context.getBean("petOwner");
        person2.getUserName();

        context.close();
    } 
}
```

* bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.2.xsd">

    <bean id="dog" class="com.spring.Dog">
        <property name="myName" value="poodle"></property>
    </bean>

    <bean id="cat" class="com.spring.Cat">
        <property name="myName" value="bella"></property>
    </bean>

    <bean id="petOwner" class="com.spring.PetOwner" scope="singleton">
        <constructor-arg name="animal" ref="dog"></constructor-arg>
    </bean>
</beans>
```
* 결과 
    * `scope="singleton"` 인 경우
        * Person name is Alice
        * Person name is Alice
    * `scope="prototype"` 인 경우
        * Person name is Alice
        * Person name is null

# 관련된 Post
* Spring Annotation 활성화 방법에 대해 알고 싶으시면 [Spring Annotation 활성화](https://gmlwjd9405.github.io/2018/12/18/spring-annotation-enable.html)을 참고하시기 바랍니다.


# References
> - [https://www.slipp.net/wiki/pages/viewpage.action?pageId=25528177](https://www.slipp.net/wiki/pages/viewpage.action?pageId=25528177)
> - [https://docs.spring.io/spring/docs/4.2.5.RELEASE/spring-framework-reference/html/beans.html#beans-factory-scopes](https://docs.spring.io/spring/docs/4.2.5.RELEASE/spring-framework-reference/html/beans.html#beans-factory-scopes)
> - [http://javaslave.tistory.com/45](http://javaslave.tistory.com/45)