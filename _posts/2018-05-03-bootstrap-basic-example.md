---
layout: post
title: '[Bootstrap] Bootstrap 사용 예제'
subtitle: 'Bootstrap 예제: Bootstrap의 간단한 사용 예제를 통해 기본적인 개념을 이해한다.'
date: 2018-05-03
author: heejeong Kwon
cover: '/images/bootstrap-download-and-setting/bootstrap-download-and-setting-main.png'
tags: Bootstrap 예제
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Bootstrap의 간단한 사용 예제를 통해 기본적인 개념을 이해한다.
> - Bootstrap css container 개념을 이해한다.


* 들어가기 전
  * Front-end (html, css, javascript + Bootstrap)
  * Back-end (Spring + Hibernate)


# Bootstrap 사용 예제
* Bootstrap page: [https://getbootstrap.com/docs/3.3/getting-started/](https://getbootstrap.com/docs/3.3/getting-started/)
  * Bootstrap page에서는 다양한 Example들을 제공한다.
  * ![](/images/bootstrap-example/bootstrap-example.png)

* carousel
  * 여러 개의 화면을 슬라이딩 방식으로 나타내는 것
  * [https://getbootstrap.com/docs/3.3/examples/carousel/](https://getbootstrap.com/docs/3.3/examples/carousel/)
  * ![](/images/bootstrap-example/bootstrap-examples-carousel.png)

  1. 우클릭 -> 페이지 소스보기
  2. 모든 내용 (Ctrl + a) 복사
  3. 'eStore' 폴더(자신이 지정한 폴더 이름)에 index.html 생성
  4. index.html을 atom 연결 프로그램으로 열어 해당 내용 붙여 넣기

* carousel 코드 이해하기
  * // (내용 설명 부분)
  * ![](/images/bootstrap-example/bootstrap-example-tip.png)
  * ![](/images/bootstrap-example/bootstrap-example-tip4.png){: width="350" height="100"}
  * 위의 내용을 제외한 부분은 그대로 붙여 넣는다.

* 위의 carousel 예제와 같은 방법으로 다양한 Example을 사용할 수 있다.


# Bootstrap css container 개념
  * [https://getbootstrap.com/docs/3.3/css/](https://getbootstrap.com/docs/3.3/css/)

  * container / container-fluid 라는 클래스를 주어 영역을 지정한다.
  ![](/images/bootstrap-example/bootstrap-containers.png)
    * **container**
      * fiexed width container
      * device에 따라 크기 고정된다.
      * 줄이면 당연히 stacked 구조로 변경
    * **container-fluid**
      * full width container
      * 화면의 움직임에 따라 화면에 꽉 차도록 가변적으로 크기가 변한다.

  * web site content와 grid system의 경우 한 번 싸야 하는데 이때, container를 이용한다.
    * **grid system**
      * 그리드 시스템은 컨텐츠를 보관하는 일련의 행과 열을 통해 페이지 레이아웃을 작성하는 데 사용된다.
      * bootstrap에서는 12등분을 사용
      * ![](/images/bootstrap-example/grid-options.png)
      * Stacked-to-horizontal의 예
        * ![](/images/bootstrap-example/stacked-to-horizontal-example.png)
    * container 사용 예제
      * [https://www.w3schools.com/bootstrap/tryit.asp?filename=trybs_default&stacked=h](https://www.w3schools.com/bootstrap/tryit.asp?filename=trybs_default&stacked=h)
      * 위의 사이트를 통해 직접 수행해보고 결과를 확인한다.
      * ![](/images/bootstrap-example/bootstrap-containers-example.png)



# 관련된 Post
* Bootstrap의 개념과 설치방법 등을 알고 싶으시면 [Bootstrap 설치 및 환경설정](https://gmlwjd9405.github.io/2018/05/02/bootstrap-download-and-setting.html) 를 참고하시기 바랍니다.


# References
> - [https://getbootstrap.com/docs/3.3/getting-started/](https://getbootstrap.com/docs/3.3/getting-started/)
> - [https://getbootstrap.com/docs/3.3/examples/carousel/](https://getbootstrap.com/docs/3.3/examples/carousel/)
> - [https://getbootstrap.com/docs/3.3/css/](https://getbootstrap.com/docs/3.3/css/)
> - [https://www.w3schools.com/bootstrap/tryit.asp?filename=trybs_default&stacked=h](https://www.w3schools.com/bootstrap/tryit.asp?filename=trybs_default&stacked=h)
