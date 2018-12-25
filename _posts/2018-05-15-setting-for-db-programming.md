---
layout: post
title: '[Spring JDBC] Spring JDBC를 이용한 데이터 접근 방법'
subtitle: 'Java Data Access Layer의 이해 및 Spring JDBC를 이용한 데이터 접근 방법'
date: 2018-05-15
author: heejeong Kwon
cover: '/images/setting-for-dbprogramming/setting-for-dbprogramming-main.png'
tags: spring mysql DB프로그래밍 jdbc
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Data Access Layer에 대해 이해할 수 있다.
    - DAO란 (DAO vs DTO)
    - DataSource란
    - JDBC란 (Plain JDBC vs Spring JDBC)
    - JDBC Driver란
> - Spring 프로젝트에서 DB(MySQL)의 데이터에 접근하기 위해 필요한 Library를 설치할 수 있다.


# Data Access Layer 이해하기
![](/images/setting-for-dbprogramming/data-access-layer.png)


### # DAO(Data Access Object) 란?
![](/images/setting-for-dbprogramming/dao-structure.png){: width="400" height="200"}

DAO(Data Access Object)는 실제로 DB에 접근하는 객체이다.
* Service와 DB를 연결하는 고리의 역할을 한다.
* DAO는 개발자가 직접 코딩해야되는 부분이다.
  * SQL를 사용하여 DB에 접근한 후 적절한 CRUD API를 제공한다.
* "Object"단위 ---(SQL을 이용한 CRUD)---> DB의 "Record"단위로 저장
  * Object와 Record 간의 miss match가 발생할 수 있다.
  * 이런 miss match는 해결해줘야 한다.

참고) <mark>DAO vs DTO</mark>
* [https://gmlwjd9405.github.io/2018/12/25/difference-dao-dto-entity.html](https://gmlwjd9405.github.io/2018/12/25/difference-dao-dto-entity.html)

### # DataSource 란?
DataSource는 JDBC 명세의 일부분이면서 일반화된 연결 팩토리이다. 즉 DB와 관계된 connection 정보를 담고 있으며, bean으로 등록하여 인자로 넘겨준다. 이 과정을 통해 Spring은 DataSource로 DB와의 연결을 획득한다.

* DataSource는 JDBC Driver vendor(Mysql, Oracle 등) 별로 여러가지가 존재한다.
* **DataSource가 하는 일**
  1. DB Server와의 기본적인 연결
  2. DB Connection Pooling 기능 (<mark>* </mark> 아래 참고)
  3. 트랜젝션 처리
* **DataSource의 구현 예시**
  1. <span style="color:#4d0000">**BasicDataSource**</span> (선택!)
  2. PoolingDataSource
  3. SingleConnectionDataSource
  4. DriverManagerDataSource
* **DataSource 설정 및 Bean 등록, 주입 방법** (<mark>* </mark> 코드 아래 참고)
* ![](/images/setting-for-dbprogramming/datasource-example.png)
  1. DB와의 연결을 위한 DB Server에 관한 정보(Property)를 설정한다.
    * 설정 정보: url, driver, username, password
  2. 해당 property file에 있는 값을 place holder을 통해 DataS래ucde의 속성으로 설정한 후 해당 BasicDataSource를 bean으로 등록한다.
    * Spring JDBC를 사용하려면 먼저, DB Connection을 가져오는 DataSource를 Spring IoC 컨테이너의 공유 가능한 *Bean으로 등록해야 한다.*
  3. 생성된 BasicDataSource Bean을 Spring JDBC에 주입한다.

<mark>* DB Connection Pooling이란?</mark>
  * 자바 프로그램에서 데이터베이스에 연결(Connection 객체를 얻는 작업)은 시간이 많이 걸린다.
  * 만약, **일정량의 Connection을 미리 생성시켜 저장소에 저장했다가** 프로그램에서 요청이 있으면 저장소에서 Connection 꺼내 제공한다면 시간을 절약할 수 있다. 이러한 프로그래밍 기법을 Connection Pooling이라 한다.
  * Connection Pooling을 이용하면 속도와 퍼포먼스가 좋아진다.

<mark>Placeholder의 장점</mark>
* 정보를 적어둔 곳에서 내용만 고치면 다른 모든 부분에서 변경된 내용이 적용된다.
* 예를 들어, Parameter 정보들을 적어둔 Property file에서 특정 정보를 수정하면  변경된 정보가 placeholder를 통해 `${jdbc.password}`에 주입된다.
* 예를 들어, pom.xml에서 springframework-version을 4.2.5.RELEASE로 설정했으면
`<groupId>org.springframework</groupId>`에 해당하는 밑에 요소들은 `${org.springframework-version}` 로 적어주면 알아서 버전에 맞는 것이 적용된다.

<mark>* DataSource 관련 코드</mark>
```code
jdbc.driver = com.mysql.jdbc.Driver
jdbc.url = jdbc:mysql//localhost:3306/databaseName
jdbc.username = root
jdbc.password = password
```
```xml
<context:property-placeholder location="com/spring/props/jdbc.properties"/>

<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
      <property name="driverClassName" value="${jdbc.driverClassName}" />
      <property name="url" value="${jdbc.url}" />
      <property name="username" value="${jdbc.username}" />
      <property name="password" value="${jdbc.password}" />
</bean>
```

### # JDBC(Java Database Connectivity) 란?
JDBC(Java Database Connectivity)는 DB에 접근할 수 있도록 ***Java에서*** 제공하는 API이다. (모든 Java의 Data Access 기술의 근간)
* JDBC는 데이터베이스에서 자료를 쿼리하거나 업데이트하는 방법을 제공한다.
* **Plain JDBC API의 문제점**
  1. 쿼리를 실행하기 전과 후에 많은 코드를 작성해야한다. EX) 연결 생성, 명령문, ResultSet 닫기, 연결 등
  2. 데이터베이스 로직에서 예외 처리 코드를 수행해야 한다.
  3. 트랜잭션을 처리해야 한다.
  4. 이러한 모든 코드를 반복하는 것으로, 시간이 낭비된다.


### # JDBC Template 이란?
JDBC Template은 Spring JDBC 접근 방법 중 하나로, 내부적으로 Plain JDBC API를 사용하지만 위와 같은 문제점들을 제거한 형태의 ***Spring에서*** 제공하는 class이다.
* **Spring JDBC가 하는 일**
  1. Connection 열기와 닫기
  2. Statement 준비와 닫기
  3. Statement 실행
  4. ResultSet Loop처리
  5. Exception 처리와 반환
  6. Transaction 처리
* **Spring JDBC에서 개발자가 할 일**
  * 핵심적으로 해야될 작업만 해주면 나머지는 Framwork가 알아서 처리해준다.
  * ![](/images/setting-for-dbprogramming/spring-jdbc-example.png)
  1. datasource 설정
  2. sql문 작성
  3. 결과 처리
* Spring framework는 아래와 같은 **Spring JDBC 접근 방법**을 제공한다.
  1. <span style="color:#4d0000">**JdbcTemplate**</span> (선택!)
    * 전형적인 Spring JDBC 접근 방법이고 가장 인기가 좋다.
    * 쿼리를 직접 작성하는 방법을 제공하므로 많은 작업과 시간을 절약 할 수 있다.
  2. NamedParameterJdbcTemplate
  3. SimpleJdbcTemplate
  4. SimpleJdbcInsert 및 SimpleJdbcCall
* **JdbcTemplate이 제공하는 기능**
  1. 실행 : Insert나 Update같이 DB의 데이터에 변경이 일어나는 쿼리를 수행하는 작업
  2. 조회 : Select를 이용해 데이터를 조회하는 작업
  3. 배치 : 여러 개의 쿼리를 한 번에 수행해야 하는 작업
* [**JdbcTemplate 사용법 참고**](https://gmlwjd9405.github.io/2018/12/19/jdbctemplate-usage.html)

### # JDBC Driver 란?
JDBC Driver는 자바 프로그램의 요청을 DBMS가 이해할 수 있는 프로토콜로 변환해주는 클라이언트 사이드 어댑터이다.
* DB마다 Driver가 존재하므로, 자신이 사용하는 DB에 맞는 JDBC Driver를 사용한다.
* DataSource를 JDBC Template에 주입(Dependency Injection)시키고 JDBC Template은
JDBC Driver를 이용하여 DB에 접근한다.

---

# Spring 프로젝트에서 DB 프로그래밍을 위해 필요한 Library
1. **jdbc class: spring-jdbc**
  * org.springframework.jdbc.core.JdbcTemplate
2. **data source: commons-dbcp**
  * org.apache.commons.dbcp.BasicDataSource
3. **jdbc driver: mysql-connector-java**
  * com.mysql.jdbc.Driver



# Spring 프로젝트에서의 DB 프로그래밍을 위해 필요한 Library 설정하기
* pom.xml의 dependencies tab 클릭 후 각 Library를 입력하여 add한다.
* 만약 pom.xml의 dependencies의 add가 안된다면

1. 아래를 적용한 후 다시 시도한다.
  * window > show view > others > maven > maven repository
  * Global Repositories의 하위 전체 선택 후 > 우클릭 > Rebuild Index 클릭
  * ![](/images/setting-for-dbprogramming/maven-add.png)
2. google에 maven repository를 검색하여 소스에 직접 넣을 수 있다.
<br> ( Maven 사이트: [https://mvnrepository.com/](https://mvnrepository.com/) )
<br> maven이 제대로 되지 않는 경우, 아래의 dependency를 직접 pom.xml에 넣는다.

~~~javascript
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${org.springframework-version}</version>
</dependency>
<dependency>
    <groupId>commons-dbcp</groupId>
    <artifactId>commons-dbcp</artifactId>
    <version>1.4</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.38</version>
</dependency>
~~~
* 각 library는 필요한 하위 부분을 함께 가져온다.
  * ![](/images/setting-for-dbprogramming/sub-libraries.png)
  * **jdbc class: spring-jdbc**
    * spring-beans
    * spring-core
    * spring-tx
  * **data source: commons-dbcp**
    * commons-pool
  * **jdbc driver: mysql-connector-java**



# 관련된 Post
* JdbcTemplate 사용법에 대해 알고 싶으시면 [JdbcTemplate 사용법](https://gmlwjd9405.github.io/2018/12/19/jdbctemplate-usage.html)을 참고하시기 바랍니다.
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다.
* MySQL 설치방법 등을 알고 싶으시면 [MySQL 설치](https://gmlwjd9405.github.io/2018/05/08/mysql-download.html) 를 참고하시기 바랍니다.
* MySQL Workbench 사용법을 알고 싶으시면 [MySQL Workbench 사용법](https://gmlwjd9405.github.io/2018/05/11/mysql-workbench-guide.html) 을 참고하시기 바랍니다.


# References
> - [https://ko.wikipedia.org/wiki/JDBC](https://ko.wikipedia.org/wiki/JDBC)
> - [https://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/jdbc.html](https://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/jdbc.html)
> - [https://www.javatpoint.com/spring-JdbcTemplate-tutorial](https://www.javatpoint.com/spring-JdbcTemplate-tutorial)
> - [https://blog.outsider.ne.kr/882](https://blog.outsider.ne.kr/882)
> - [http://preamtree.tistory.com/88](http://preamtree.tistory.com/88)
> - [http://smallgiant.tistory.com/13](http://smallgiant.tistory.com/13)
> - [http://www.java-school.net/jdbc/Connection-Pool](http://www.java-school.net/jdbc/Connection-Pool)
