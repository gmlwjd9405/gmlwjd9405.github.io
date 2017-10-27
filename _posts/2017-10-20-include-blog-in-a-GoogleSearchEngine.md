---
layout: post
title: '[Jekyll] 검색엔진에 블로그 등록하기 _Google편'
subtitle: 'Jekyll 블로그를 Google 검색엔진에 노출하기'
date: 2017-10-20
author: heejeong Kwon
cover: '/images/GoogleSearchEngine/GoogleSearchEngine-main.png'
tags: google 검색엔진 jekyll
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 기본 환경: Jekyll을 이용한 .github.io 블로그 사용자
> - Jekyll을 이용한 .github.io 블로그를 'Google 검색엔진'에 노출하기


## 구글 웹마스터 도구(Search Console)에 속성 추가(블로그 등록)
1. [구글 웹마스터 도구](https://www.google.com/webmasters/tools/home?hl=ko)에 접속한다.  
Google에 등록하기 위해서는 우선, 구글 웹마스터 도구에 접속하고 Google 계정으로 로그인한다.
2. [속성추가] 버튼을 선택한다.
3. 자신의 jekyll 블로그 주소를 입력하여 속성에 추가한다. (ex.http://[github 사용자명].github.io/)

![](/images/GoogleSearchEngine/GoogleSearchEngine-searchconsole-start.png)



## 블로그의 소유자임을 인증한다.
1. Google에서 제공하는 HTML을 다운받는다.  
![](/images/GoogleSearchEngine/GoogleSearchEngine-Before-authenticating-html.png)
2. 다운받은 HTML 파일을 자신의 GitHub Jekyll 블로그의 가장 상위 디렉터리에 올린다.  
<span style="background-color: #e1e1e1">*Git Commit & Push 작업이 필요하다.*</span>  
~~~javascript
$ git add .
$ git commit -m "Include in Google search engine"
$ git push origin master // 원격 저장소에 변경 내용을 올린다.
~~~
![](/images/GoogleSearchEngine/GoogleSearchEngine-github-authenticating-html.png)
3. Google Search Console 페이지로 돌아와 인증을 확인한다.  
![](/images/GoogleSearchEngine/GoogleSearchEngine-verify-blog-owner.png)



## sitemap.xml 파일 생성하기
- Sitemap 이란?
  - **웹 크롤링 로봇이 이용할 수 있는 웹사이트의 접근 가능한 페이지의 목록**
  - 웹사이트의 웹페이지를 계층별로 구분지어 웹사이트의 전체 구조를 보여주며, 검색엔진의 크롤링 로봇들이 크롤링 작업에 유용하게 사용할 수 있다.
  - sitemap.xml 파일을 사용하면 사이트 및 콘텐츠 구조를 Google 및 기타 검색엔진에 손쉽게 제출할 수 있다.
  - 검색엔진에 크롤링해야하는 URL을 알려줌으로써 색인을 생성하는 방법과 색인을 생성하는 방법을 제어한다.

- sitemap.xml 생성 방법  
Jekyll에는 sitemap.xml 파일을 만들 수 있는 ***두 가지 방법*** 이 있다. 두 가지 방법 중 원하는 방법을 선택하여 적용하면 된다. (필자는 두 번째 방법인 수동으로 sitemap.xml을 작성하였다.)   

  1. 플러그인 사용
    - Sitemap(sitemap.xml)을 자동으로 생성하려면 GitHub Jekyll 블로그의 *\_config.yml* 파일에 아래의 내용을 추가한다.
    - ![](/images/GoogleSearchEngine/GoogleSearchEngine-config-sitemap-plugin-check.png)
    - 플러그인 사용을 위한 추가적인 [참고 사이트](https://github.com/jekyll/jekyll-sitemap)


  2. sitemap.xml 파일을 수동으로 작성
    - sitemap.xml 파일을 자신의 GitHub Jekyll 블로그의 가장 상위 디렉터리에 생성한다.
    - 아래의 코드를 넣는다.(1~2 행의 --- 부분도 포함시켜야 한다.)  
    - [다음 페이지](https://github.com/gmlwjd9405/gmlwjd9405.github.io/blob/master/sitemap.xml)를 참고하자.
    - ![](/images/GoogleSearchEngine/GoogleSearchEngine-sitemap-contents.png)
    - 수동으로 작성하기 위한 추가적인 [참고 사이트](http://davidensinger.com/2013/11/building-a-better-sitemap-xml-with-jekyll/)

- Local에서 sitemap.xml 확인하기
  - jekyll serve 명령어를 이용하여 로컬 서버를 실행한다.
  - [http://127.0.0.1:4000/sitemap.xml](http://127.0.0.1:4000/sitemap.xml) 에 접속하여 내용을 확인한다.

- sitemap 설정하기
  - 포스트를 작성할 때 해당 포스트의 변경 주기나 우선순위 정보 등을 변경하고 싶으면, 아래와 같이 lastmod, sitemap.chagefreq, sitemap.priority 등의 태그 정보를 추가한다.
  - 각 포스트의 맨 위에 아래와 같이 sitemap의 옵션을 추가하여 설정할 수 있다.
  - 설정이 없을 때의 default 설정은 sitemap.xml에 정의되어 있다.
  - sitemap의 changefreq를 너무 짧게 하면 빈번한 접속으로 안좋은 영향을 미칠 수도 있으니 중요한 변동이 없는 포스트는 일주일 정도로 잡아준다.
  ~~~javascript
  ---
  layout: post
  title: '제목'
  date: 2017-10-20
  lastmod : 2017-10-20 12:00:00
  sitemap :
    changefreq : daily
    priority : 1.0
  ---
~~~

  ---
  <mark>주의사항</mark>  
  <span style="color:#4d0000">*\_config.yml 파일 설정 확인하기*</span>  
  루트 디렉터리(GitHub Jekyll 블로그의 가장 상위 디렉터리)에 존재하는 \_config.yml 파일 내의 url 부분에 자신의 블로그 url을 입력해야 sitemap.xml에서 site.url 부분을 사용 할 수 있다.  
  => \_config.yml 파일 내에 *url: 'http://[github 사용자명].github.io'* 이 있는지 확인한다.



## robots.txt 파일 생성하기
- robots.txt 이란?
  - **검색엔진의 웹 크롤러가 방문할 때 참고하는 정책을 명시**
  - robots.txt 파일에 sitemap.xml 위치를 등록해두면 검색엔진의 크롤러들이 홈페이지를 크롤링하는데 도움을 준다.

- robots.txt 생성하기
  - robots.txt 파일을 자신의 GitHub Jekyll 블로그의 가장 상위 디렉터리에 생성한다.
  - 아래의 코드를 붙여 넣는다.
  - ~~~javascript
  User-agent: *
  Allow: /
  Sitemap: http://[자신의 github 사용자명].github.io/sitemap.xml
  ~~~


## sitemap.xml, robots.txt 파일 GitHub에 올리기
  - 자신의 GitHub Jekyll 블로그의 가장 상위 디렉터리에 올린다.  
  <span style="background-color: #e1e1e1">*Git Commit & Push 작업이 필요하다.*</span>  

  ~~~javascript
  $ git add .
  $ git commit -m "Update for Including in Google search engine"
  $ git push origin master // 원격 저장소에 변경 내용을 올린다.
  ~~~
  ![](/images/GoogleSearchEngine/GoogleSearchEngine-github-check.png)



## 구글 웹마스터 도구(Search Console)에 sitemap.xml 제출하기
Google이 내 블로그를 크롤링하는 방식을 판단하고 검색하기 위해서는 sitemap.xml을 제출해야 한다.
1. [구글 웹마스터 도구](https://www.google.com/webmasters/tools/home?hl=ko)에 접속한다.
2. 자신이 추가한 속성(블로그)을 선택한다.
3. 대시보드의 왼쪽에 위치한 🔽 크롤링을 선택한 후 하위메뉴 [Sitemaps]를 선택한다.  
4. 우측 상단의 [SITEMAP 추가/테스트] 버튼을 선택한다.
5. 빈칸에 'sitemap.xml'를 입력한다. (ex.https://[github 사용자명].github.io/sitemap.xml)  
![](/images/GoogleSearchEngine/GoogleSearchEngine-add-sitemap.png)
6. 아래와 같이 제출된 화면을 볼 수 있다. 시간이 지나면 접수중 색인이 숫자로 변경된다.
![](/images/GoogleSearchEngine/GoogleSearchEngine-sitemap-success.png)



## References
> - [https://nesoy.github.io/articles/2017-01/blog-search](https://nesoy.github.io/articles/2017-01/blog-search)
> - [http://iwordpower.com/2016/06/how-to-add-to-google-search-engine/](http://iwordpower.com/2016/06/how-to-add-to-google-search-engine/)
> - [http://ogaeng.com/mkt-con04/](http://ogaeng.com/mkt-con04/)
> - [https://wayhome25.github.io/etc/2017/02/20/google-search-sitemap-jekyll/](https://wayhome25.github.io/etc/2017/02/20/google-search-sitemap-jekyll/)
> - [https://pravusid.github.io/lifelog/2017/08/28/jekyll%EC%84%A4%EC%A0%95.html](https://pravusid.github.io/lifelog/2017/08/28/jekyll%EC%84%A4%EC%A0%95.html)
> - [https://mijingo.com/blog/building-a-sitemap-with-jekyll](https://mijingo.com/blog/building-a-sitemap-with-jekyll)
> - [http://davidensinger.com/2013/11/building-a-better-sitemap-xml-with-jekyll/](http://davidensinger.com/2013/11/building-a-better-sitemap-xml-with-jekyll/)
> - [https://github.com/jekyll/jekyll-sitemap](https://github.com/jekyll/jekyll-sitemap)
