---
layout: post
title: '[DB] DB 파티셔닝(Partitioning)이란'
subtitle: 'DB Partitioning의 개념과 장단점을 이해한다.'
date: 2018-09-24
author: heejeong Kwon
cover: '/images/network/3way-handshaking-main2.png'
tags: db partitioning
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Goal
> - DB 파티셔닝(Partitioning)이란
> - DB 파티셔닝(Partitioning)의 장단점
> - DB 파티셔닝(Partitioning)의 종류
> - DB 파티셔닝(Partitioning)의 분할 기준

## DB 파티셔닝(Partitioning)이란
### 배경
* 서비스의 크기가 점점 커지고 DB에 저장하는 데이터의 규모 또한 대용량화 되면서, 기존에 사용하는 DB 시스템의 **용량(storage)의 한계와 성능(performance)의 저하** 를 가져오게 되었다.
* 즉, VLDB(Very Large DBMS)와 같이 하나의 DBMS에 너무 큰 table이 들어가면서 용량과 성능 측면에서 많은 이슈가 발생하게 되었고, 이런 이슈를 해결하기 위한 방법으로 <span style="background-color: #e1e1e1">table을 '파티션(partition)'이라는 작은 단위로 나누어 관리하는 **'파티셔닝(Partitioning)'기법**</span> 이 나타나게 되었다.
* '파티셔닝(Partitioning)'기법을 통해 소프트웨어적으로 데이터베이스를 분산 처리하여 성능이 저하되는 것을 방지하고 관리를 보다 수월하게 할 수 있게 되었다.

### 개념
* 논리적인 데이터 엘리먼트들을 다수의 엔티티로 쪼개는 행위를 뜻하는 일반적인 용어
* 즉, **큰 table이나 index를 관리하기 쉬운 partition이라는 작은 단위로, 물리적으로 분할하는 것을 의미한다.**
  * 물리적인 데이터 분할이 있더라도, DB에 접근하는 application의 입장에서는 이를 인식하지 못한다.

### 목적
1. 성능(Performance)
  * 특정 DML과 Query의 성능을 향상시킴, 주로 대용량 Data WRITE 환경에서 효율적이다.
  * 특히, Full Scan에서 데이터 Access의 범위를 줄여 성능 향상을 가져온다.
  * 많은 INSERT가 있는 OLTP 시스템에서 INSERT 작업들을 분리된 partition들로 분산시켜 경합을 줄인다.
2. 가용성(Availability)
  * 물리적인 Partitioning으로 인해 전체 데이터의 훼손 가능성이 줄어들고 데이터 가용성이 향상된다.
  * 각 분할 영역(partition별로)을 독립적으로 백업하고 복구할 수 있다.
  * table의 partition 단위로 Disk I/O을 분산하여 경합을 줄이기 때문에 UPDATE 성능을 증가시킨다.
3. 관리용이성(Manageability)
  * 큰 table들을 제거하여 관리를 쉽게 해준다.

## DB 파티셔닝(Partitioning)의 장단점
### 장점
* 관리적 측면 : partition 단위 백업, 추가, 삭제, 변경
  * 전체 데이터를 손실할 가능성이 줄어들어 데이터 가용성이 향상된다.
  * partition별로 백업 및 복구가 가능하다.
  * partition 단위로 I/O 분산이 가능하여 UPDATE 성능을 증가시킨다.
* 성능적 측면 : partition 단위 조회 및 DML수행
  * 데이터 전체 검색 시 필요한 부분만 탐색해 성능이 증가한다.
    * 즉, Full Scan에서 데이터 Access의 범위를 줄여 성능 향상을 가져온다.

### 단점
* table간의 JOIN 대한 비용이 증가한다.
* 테이블과 인덱스를 별도로 파티션 할 수 없다.
  * 테이블과 인덱스를 같이 Partitioning 해야 한다.

## DB 파티셔닝(Partitioning)의 종류
![](/images/network/tcp-virtual-circuit.png){: width="450" height="330"}

1. 수평(horizontal) 파티션
  * **샤딩(Sharding)** 과 동일한 개념
  * 하나의 테이블의 각 행을 다른 테이블에 분산시키는 것이다.
  * 스키마 복제 후 샤드키를 기준으로 데이터를 나누는 것을 말한다.
  * 스키마(schema)가 같은 데이터를 두 개 이상의 테이블에 나누어 저장하는 것을 말한다.
  * 퍼포먼스, 가용성을 위해 KEY 기반으로 여러 곳에 분산저장
  * 일반적으로 분산 저장기술에서 파티셔닝은 수평분할을 의미한다.
  * 가령 같은 주민 데이터를 처리하기 위해 스키마가 같은 '서현동주민 테이블'과 '정자동주민 테이블'을 사용하는 것을 말한다. 인덱스의 크기를 줄이고, 작업 동시성을 늘리기 위한 것이다. 보통 수평 분할을 한다고 했을 때는 하나의 데이터베이스 안에서 이루어지는 경우를 지칭한다.
  * 에를 들어 예를 들면, 나는 고객의 데이터베이스를 CustomerId를 샤드키로 사용하여 샤딩하기로 하였다. 0 ~ 10000 번 고객의 정보는 하나의 샤드에 저장하고 10001 ~ 20000 번 고객의 정보는 다른 샤드에 저장하기로 하였다. DBA는 데이터 엑세스 패턴과 저장 공간 이슈(로드의 적절한 분산 , 데이터의 균등한 저장)를 고려하여 적절한 샤드키를 결정하게 된다.
* 장단점
<!-- Benefits
데이터의 개수를 기준으로 나누어 Partitioning한다.
데이터의 개수가 작아지고 따라서 index의 개수도 작아지게 된다. 자연스럽게 성능은 향상된다.
Disadvantages
서버간의 연결과정이 많아진다.
데이터를 찾는 과정이 기존보다 복잡하기 때문에 latency가 증가하게 된다.
하나의 서버가 고장나게 되면 데이터의 무결성이 깨질 수 있다. -->

2. 수직(vertical) 파티션
  * 스키마를 나누고 데이터가 따라 옮겨가는것을 말한다.
  * 하나의 엔티티를 2개 이상으로 분리하는 작업
  * 모든 컬럼들 중 특정 컬럼들을 쪼개서 따로 저장하는 형태를 의미한다.
  * 관계형 DB에서 3정규화와 같은 개념으로 접근하면 이해가 용이하다.
  * 예를 들면, 한 고객은 하나의 청구 주소를 가지고 있을 수 있다. 그러나 나는 데이터의 유연성을 위해 다른 데이터베이스로 정보를 이동하거나 보안의 이슈등을 이유로 CustomerId를 참조하도록 하고 청구 주소 정보를 다른 테이블로 분리할 수 있다.


* 장단점
      <!-- 테이블의 컬럼을 기준으로 나누어 Partitioning한다.
      정규화하는 과정도 이와 비슷하다고 볼 수 있지만 Vertical Partitioning은 이미 정규화된 Data를 분리하는 과정이다.
      자주 사용하는 컬럼등을 분리시켜 성능을 향상시킬 수 있다.

  샤딩이든 파티셔닝이든 필요한 데이터만 빠르게 조회할 수 있기 때문에 쿼리 자체가 가볍다.

  샤딩(수평 분할)의 경우에는 오래되서 조회가 안되는 데이터를 클라우드에 올리거나 별도의 디스크에 저장해서 운영 상의 스토리지 이득을 볼 수 있다.

  파티셔닝(수직 분할)의 경우에는 한 테이블을 SELECT 하면 결국 모든 컬럼을 메모리에 올리게 되므로 필요없는 컬럼까지 올라가서 한번에 읽을 수 있는 ROW가 줄어든다. 이는 I/O 측면에서 봤을 때 필요한 컬럼만 올리면 훨씬 많은 수의 ROW를 메모리에 올릴 수 있으니 성능상의 이점이 있다.

  또한 수직 분할은 같은 타입의 데이터가 저장되기 때문에 저장 시 데이터 압축률을 높일 수 있다. -->

## DB 파티셔닝(Partitioning)의 분할 기준
1. 범위 분할 (range partitioning)
2. 목록 분할 (list partitioning)
3. 해시 분할 (hash partitioning)
4. 합성 분할 (composite partitioning)

<!-- # 관련된 Post
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다. -->


# References
> - [http://theeye.pe.kr/archives/1917](http://theeye.pe.kr/archives/1917)
> - [https://zetastring.tistory.com/338](https://zetastring.tistory.com/3380)
> - [http://www.gurubee.net/lecture/1906](http://www.gurubee.net/lecture/1906)
> - [http://wiki.gurubee.net/pages/viewpage.action?pageId=3899999](http://wiki.gurubee.net/pages/viewpage.action?pageId=3899999)
> - [http://wiki.gurubee.net/pages/viewpage.action?pageId=26742648](http://wiki.gurubee.net/pages/viewpage.action?pageId=26742648)
> - [위키-데이터베이스분할](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4_%EB%B6%84%ED%95%A0)
