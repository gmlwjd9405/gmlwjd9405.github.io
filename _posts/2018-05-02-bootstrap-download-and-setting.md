---
layout: post
title: '[Bootstrap] Bootstrap 설치 및 환경설정'
subtitle: 'Bootstrap 개요: Bootstrap 설치 및 기본 개념과 템플릿 예제에 대해 이해한다.'
date: 2018-05-02
author: heejeong Kwon
cover: '/images/bootstrap-download-and-setting/bootstrap-download-and-setting-main.png'
tags: Bootstrap 설치 다운로드 PlugIn
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Bootstrap 개념을 이해한다.
> - Bootstrap 설치 방법을 익힌다.
> - Bootstrap 기본 HTML 템플릿을 이해한다.


* 들어가기 전
  * Front-end (html, css, javascript + Bootstrap)
  * Back-end (Spring + Hibernate)


# Bootstrap 개념
* 정의
  * Bootstrap is the most popular HTML, CSS, and JS framework for developing **responsive, mobile first** projects on the web.
  * ***responsive***
    * 반응형 웹, 요즘 다양한 device(mobile에서도 smart phone, tablet 등이 있다.)에 따라 적절하게 반응한다.
    * 웹 페이지를 만드는 방법 3가지
    ![](/images/bootstrap-download-and-setting/bootstrap-webpage.png)
    1. desktop pc 용으로 웹 페이지를 만든다. (mobile에서 보면 화면이 굉장히 작게 보인다.)
    2. desktop PC용과 mobile용 각각 별도의 웹 페이지를 만든다.
    3. 웹 페이지는 하나로 만들고 그 페이지가 적절하게 반응한다. (반응형 웹, 화면에 맞게 조정(adaptation)이 된다.)
    * 반응형 웹
      * 반응형 웹의 예) ex. Stacked to Horizontal
        * Stacked: 쌓인다. 수직 방향으로 나타난다.
        * Horizontal : 화면이 넓어지면 수평 방향으로 나타난다.
  * ***mobile first*** VS desktop first
    * desktop PC 또는 mobile 중 어디에 맞춰서 웹 페이지를 개발할지 정해야 한다.
    * 모바일 우선(Mobile First) VS 데스크톱 우선(Desktop First)
      * 참고 URL: [https://www.cmsfactory.net/node/10362](https://www.cmsfactory.net/node/10362)
- 현재 가장 유명한 html, css, javascript 프레임워크이다. (특히 css과 가장 관련)
- 트위터 개발자들이 만든 프레임워크이다.
- 간단하게 말하자면 view를 꾸미는 것이다.


# Bootstrap 설치
* Bootstrap page: [https://getbootstrap.com/docs/3.3/getting-started/](https://getbootstrap.com/docs/3.3/getting-started/)
    * ![](/images/bootstrap-download-and-setting/bootstrap-download.png)
    * 설치 방법 2가지
      * 방법 1: local에 다운로드하는 방법
      * 방법 2: CDN을 이용하여 자동으로 다운받도록 하는 방법
* 방법 1: local에 다운로드하는 방법
  1. bootstrap-3.3.7-dist 압축 풀기(css, fonts, js라는 내부폴더 생성된다.)
  2. 바탕화면에 'eStore'(자신이 설정한 폴더 이름)라는 이름으로 폴더 생성
  3. bootstrap-3.3.7-dist의 내부폴더 복사해서 eStore에 붙여넣기
  4. 내부폴더 css의
    * .map 파일은 삭제해도 무관
    * 확장자 css에서 .min은 축약의 형태로 큰 차이는 없다. (해당 프로젝트에서는 .min사용)
  5. 내부폴더 js의 bootstrap.min을 제외하고 나머지는 삭제한 (가볍게 하기 위해)


# Bootstrap 기본 HTML 템플릿을 이용한 기본적인 환경설정
* 환경 설정
  * Bootstrap page에서 기본적인 template을 제공한다.
  * 이것을 이용하여 .xml을 만들면 된다.
    * ![](/images/bootstrap-download-and-setting/bootstrap-basic-template.png)
* Bootstrap 기본 HTML 템플릿 이해하기
  * ![](/images/bootstrap-download-and-setting/bootstrap-basic-template-code.png)
  * 주석을 제외한 기본 HTML 템플릿 코드
    ~~~javascript
    <!DOCTYPE html><html lang="en">
      <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        //가장 중요한 부분 viewport 삽입
        <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->
        <title>Bootstrap 101 Template</title>

        <!-- Bootstrap -->
        <link href="css/bootstrap.min.css" rel="stylesheet">
        //앞으로 사용할 bootstrap에 관계된 css 파일을 연결(다운로드 받은 것)
        <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
        <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
      </head>

      <body>
        <h1>Hello, world!</h1>

        <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
        //jquery(브라우저 호환성이 있는 HTML 속 자바스크립트 라이브러리)에 관계된 부분 반드시 삽입
        <!-- Include all compiled plugins (below), or include individual files as needed -->
        <script src="js/bootstrap.min.js"></script>
        //bootstrap에 관계된 js 부분(다운로드 받은 것)
      </body></html>
      ~~~


# 관련된 Post
* Bootstrap의 간단한 예제를 알고 싶으시면 [Bootstrap 사용 예제](https://gmlwjd9405.github.io/2018/05/03/bootstrap-basic-example.html) 를 참고하시기 바랍니다.


# References
> - [https://getbootstrap.com/docs/3.3/getting-started/](https://getbootstrap.com/docs/3.3/getting-started/)
> - [https://www.cmsfactory.net/node/10362](https://www.cmsfactory.net/node/10362)
