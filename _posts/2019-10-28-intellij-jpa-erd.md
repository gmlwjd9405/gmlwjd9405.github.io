---
layout: post
title: '[IntelliJ] intellij에서 JPA Entitiy 기반의 ERD 그리기'
subtitle: 'intellij에서 JPA Entity 기반의 ERD를 확인할 수 있다.'
date: 2019-10-28
author: heejeong Kwon
cover: '/images/erd/intellij-erd-main.png'
tags: intellij jpa erd 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

> - 들어가기 전
>   - Hibernate를 사용한 프로젝트를 기준으로 설명한다.


## Persistence Tab 설정 
- File > Project Structure > Project Settings > Facets > [+] 버튼 클릭 
    - ![](/images/erd/intellij-erd-1.png)

- JPA 선택 > Choose Module 창 > ["ERD를 확인할 프로젝트 명".main] 선택 
    - ![](/images/erd/intellij-erd-2.png)

- Project Settings > Modules > 위에서 생성한 JPA 선택 
    - ![](/images/erd/intellij-erd-3.png)

- Default JPA Provider 를 Hibernate 로 선택 
    - ![](/images/erd/intellij-erd-4.png)


## JPA Entity 기반의 ERD 확인하기 
- intellij 왼쪽의 Persistence Tab 생성 확인
    - ![](/images/erd/intellij-erd-5.png)

#### 기본적인 ER Diagram 확인 
- Persistence 클릭 > 확인하고자 하는 Entity 구조 우클릭 > ER Diagram 클릭 
    - ![](/images/erd/intellij-erd-6.png)
    - ![](/images/erd/intellij-erd-7.png)

#### 조금 더 상세한 ERD 확인 
- ERD 창 임의의 위치에서 우클릭 > Show Edge Labels 클릭 
    - ![](/images/erd/intellij-erd-8.png)
    - ![](/images/erd/intellij-erd-9.png)
