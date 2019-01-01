---
layout: post
title: '[IntelliJ] Spring .jsp 파일 update가 적용되지 않는 경우'
subtitle: 'IntelliJ에서 resource 파일 변경이 반영되지 않는 경우'
date: 2019-01-01
author: heejeong Kwon
cover: '/images/error/error-main.png'
tags: error intellij spring not-working jsp update
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## 오류 내용
`Spring jsp 파일을 수정했는데 자동으로 브라우저에 반영되지 않는 경우`<br>
`IntelliJ에서 resource 파일 변경이 반영되지 않는 경우`

## 해결 방법
1. Run > Edit Configurations... 설정
* ![](/images/error/not-working-jsp-update.png)
* Server tab > On frame deactivation : **Update classes and resources** 
  * 수정한 클래스나 JSP 파일이 자동으로 반영된다.

2. Run > Run 'Tomcat Server' 


<!-- # 관련된 Post
* []() -->


# References
> - [http://smallgiant.tistory.com/30](http://smallgiant.tistory.com/30)
> - [http://lovesigma.github.io/realtime-resource--and-cors%C2%A0/](http://lovesigma.github.io/realtime-resource--and-cors%C2%A0/)