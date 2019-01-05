---
layout: post
title: '[SpringMVC] IntelliJ에서 SpringMVC, Tomcat 설정'
subtitle: 'IntelliJ에서 Tomcat을 이용하여 SpringMVC Server를 띄울 수 있다.'
date: 2018-10-25
author: heejeong Kwon
cover: '/images/springMVC-project/springMVC-project-setting2-main.png'
tags: springMVC project example setting tomcat intellij
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - IntelliJ에서 Controller를 만들 수 있다.
> - IntelliJ에서 Tomcat을 이용하여 SpringMVC Server를 띄울 수 있다.

* 들어가기 전
  * OS: MacOS
  * IDE: IntelliJ (버전: 2018.1.5)
  * Spring, Spring MVC (버전: 4.3.18)
  * gradle, java 사전 설치 
  * tomcat 사전 설치 (버전: 8.5.34)

### IntelliJ에서 Gradle 프로젝트 생성 및 SpringMVC를 위한 설정
* [https://gmlwjd9405.github.io/2018/10/24/intellij-springmvc-gradle-setting.html](https://gmlwjd9405.github.io/2018/10/24/intellij-springmvc-gradle-setting.html)  참고

## 기본 설정
**1) web.xml 설정**
  * web - WEB-INF - web.xml 선택 후 아래와 같이 수정 
  * (아래 외의 다른 부분은 동일)

```xml   
<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern> <!-- "*.form"를  "/"로 변경-->
</servlet-mapping>
```
**2) Artifacts에서 Spring Library 추가**
* 1) File - Project Structure 선택 또는 단축키 (cmd + ;) 이용 
* 2) Project Setting의 Artifacts 선택 
  * Spring Library 사용을 위한 추가적인 설정 
    * (참고) Maven Library도 마찬가지로 dependency를 추가한 후 아래와 같은 추가적인 설정을 해야 Library를 사용할 수 있다.
  * ![](/images/springMVC-project/setting2/setting2-1.png)
  * 이때 제대로 뜨지 않는 경우는 밑에 Problems 선택
  * Fix - Add to Dependencies... 클릭
    * ![](/images/springMVC-project/setting2/setting2-2.png)
* 3) Available Elements 아래에 있는 Spring, Spring MVC Library 더블 클릭
  * ![](/images/springMVC-project/setting2/setting2-3.png)

**3) dispatcher-servlet.xml 설정**
```xml   
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- Annotation 활성화 -->
    <mvc:annotation-driven></mvc:annotation-driven>
    <!-- Component 패키지 지정 -->
    <context:component-scan base-package="com.heee.web.controller"></context:component-scan>

    <!-- 여기서 설정한 내용으로 view object의 이름이 결정된다. -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>

</beans>
```
* Annotation 활성화?
  * annotation(@)을 사용하겠다는 것을 명시
  * *@Component*: 일반적
  * *@Controller, @Repository, @Service*: 역할에 따른 구체적인 annotation 설정 가능
* Component 패키지 지정?
  * base-package로 설정한 패키지를 스캔하여 annotation이 달린 것을 bean으로 생성하여 Container에 담는다.
* view object의 이름?
  * view object name = **prefix + local view name + suffix**
  * 즉, 설정한 정보에 따라 `/WEB-INF/views/index.jsp` 의 형태로 구성된다.

**4) view 디렉터리 생성 및 jsp 파일 이동**
* 위의 dispatcher-servlet.xml에 설정한 위치에 맞게 jsp 파일을 옮긴다.
  * 1) web - WEB-INF 아래 view 디렉터리 생성
  * 2) index.jsp 이동 후 "Hello World" 출력을 위해 body 부분 수정
* ![](/images/springMVC-project/setting2/setting2-4.png)

## 간단한 Controller 생성
**1) 위의 dispatcher-servlet.xml에 설정한 위치에 맞게 패키지 및 클래스 생성**
* `src/main/java` 에 패키지 및 테스트 클래스 생성
* ![](/images/springMVC-project/setting2/setting2-5.png)

**2) Controller 클래스 내용**
* 빨간 경고창이 뜨는 위치에 커서를 댄 상태에서 **option + enter**를 누르면 import가 가능하다.
* ![](/images/springMVC-project/setting2/setting2-6.png)

```java
package com.heee.web.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HomeController {

    @RequestMapping(value = "/")
    public String home() {
        return "index";
    }
}
```
```java
package com.heee.web.controller;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;


@RestController
public class HomeController {

    @RequestMapping(value = "/")
    public String home() {
        return "Hello World";
    }
}
```
* <span style="background-color: #e1e1e1">@Controller 와 @RestController 의 차이</span>
  * @Controller
    * API와 view를 동시에 사용하는 경우에 사용
    * 대신 API 서비스로 사용하는 경우는 @ResponseBody를 사용하여 객체를 반환한다.
    * view(화면) return이 주목적 
  * @RestController
    * view가 필요없는 API만 지원하는 서비스에서 사용 (Spring 4.0.1부터 제공)
    * @RequestMapping 메서드가 기본적으로 @ResponseBody 의미를 가정한다.
    * data(json, xml 등) return이 주목적 
  *  즉, **@RestController = @Controller + @ResponseBody**
* 여기선 @Controller을 기준으로 작성한다.

## Tomcat 설정
**1) Tomcat 사전 설치** : [http://superordinarystory.tistory.com/3](http://superordinarystory.tistory.com/3) 참고 

**2) Tomcat Server 추가**
1. Run - Edit Configuration 선택 
2. '+' 클릭 후 Tomcat Server - Local 선택 
  * ![](/images/springMVC-project/setting2/setting2-7.png)
3. Name 설정
  * 만약 Application server에 내용이 없다면 직접 Configure.. 설정 
  * 위의 사전 설치 과정을 따라갔다면 `/usr/local/apache-tomcat-(ver)` 으로 설정
  * ![](/images/springMVC-project/setting2/setting2-8.png)
4. 맨 아래의 Warning의 Fix 클릭
  * ![](/images/springMVC-project/setting2/setting2-9.png)
5. 설정 완료 후 모습
  * ![](/images/springMVC-project/setting2/setting2-10.png)

## Tomcat Server를 실행하여 "Hello World" 띄우기 
* ![](/images/springMVC-project/setting2/setting2-11.png)
* ![](/images/springMVC-project/setting2/setting2-12.png)

# 관련된 Post
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다.
* IntelliJ에서 Gradle 프로젝트 생성 및 SpringMVC를 위한 설정하기에 대해 알고 싶으시면 [IntelliJ에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/10/24/intellij-springmvc-gradle-setting.html)를 참고하시기 바랍니다.



# References
> - [http://syung05.tistory.com/26](http://syung05.tistory.com/26)
> - [https://nesoy.github.io/articles/2017-02/SpringMVC](https://nesoy.github.io/articles/2017-02/SpringMVC)
> - [http://superordinarystory.tistory.com/3](http://superordinarystory.tistory.com/3)
> - [https://doublesprogramming.tistory.com/105](https://doublesprogramming.tistory.com/105)