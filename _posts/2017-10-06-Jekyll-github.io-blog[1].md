---
layout: post
title: 'Jekyll을 이용한 .github.io 블로그 만들기[1]'
subtitle: 'Jekyll에 대한 개념과 설치 및 실행 익히기'
date: 2017-10-06
author: Jekyll
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: jekyll
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---
## Requirements
> - GNU/Linux, Unix, or macOS
> - Ruby version 2.1 or above, including all development headers
> - RubyGems



## Jekyll 이란?
**아주 심플하고 블로그 지향적인 정적 사이트 생성기**  
- Jekyll은 다양한 포맷의 원본 텍스트 파일을 템플릿 디렉터리로부터 읽어서, (Markdown 등의) 변환기와 Liquid 렌더러를 통해 가공하여, 당신이 즐겨 사용하는 웹 서버에 곧바로 게시할 수 있는, 완성된 정적 웹사이트를 만들어낸다.  
- Jekyll 은 GitHub Pages 의 내부 엔진이기도 하다. 다시 말해, Jekyll 을 사용하면 자신의 프로젝트 페이지나 블로그, 웹사이트를 무료로 GitHub 에 Posting 할 수 있다는 뜻이다.  



## Jekyll 설치하기
터미널에서 RubyGems를 사용하여 아래 명령어만으로 Jekyll의 모든 gem 의존요소들이 자동적으로 설치된다.
~~~javascript
$ sudo gem install jekyll
~~~

---
<mark>Error</mark>  
<span style="color:#4d0000">*Error installing jekyll: public_suffix requires Ruby version >= 2.1.*</span>  
위의 오류는 현재 설치된 ruby가 최신 버전이 아니기 때문에 나타나는 오류이므로 아래의 명령어를 이용하여 최신 버전의 ruby를 설치한 후 다시 진행한다.  
=> `$ brew install ruby`




## Jekyll 실행하기
원하는 디렉터리 위치(ex. ~/blog)에서 아래의 명령어를 입력한다.
```javascript
~/blog$ jekyll new gmlwjd9405.github.io
~/blog$ jekyll new [github 사용자명].github.io
```
~~~c
~
├── blog
   ├── [github 사용자명].github.io
~~~
현재 위치(ex. ~/blog)에 <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span>라는 이름의 디렉터리가 생성되고 그 안에 Jekyll이 기본으로 제공하는 테마가 저장된다.
이렇게 만든 블로그를 실행시키기 위해서는 <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span> 디렉터리로 이동한 후에 아래의 명령어를 입력한다.
~~~javascript
~/blog/[github 사용자명].github.io$ jekyll serve
# 개발용 서버가 http://localhost:4000/ 에서 구동된다.
# 자동-재생성: 활성화됨. 비활성화 하려면 `$jekyll serve --no-watch`를 이용
~~~
이 명령으로 <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span> 디렉터리 하위에 \_site 디렉터리를 생성하여 그 안에 실제 웹에서 표현되는 html파일을 만든다. 이를 통해 로컬상에서 브라우저로 접속하여 **사이트가 어떻게 생성될지 미리 살펴볼 수 있다.**  
(GitHub에 호스팅할 경우 \_site 디렉터리는 없어도 되므로 .gitignore에 넣는다.)
![](/images/JekyllStart1/JekyllStart1-directory-structure.png)

브라우저에 [127.0.0.1:4000](http://127.0.0.1:4000/)나 [localhost:4000](http://localhost:4000/)를 입력한 후 아래와 같은 화면이 나오면 Jekyll의 기본테마 블로그는 정상적으로 완료되었음을 알 수 있다.
![](/images/JekyllStart1/JekyllStart1-success-page.png)

---
<mark>Error</mark>  
<span style="color:#4d0000">*Yikes! It looks like you don't have jekyll-paginate or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file - jekyll-paginate' If you run into trouble, you can find helpful resources at https://jekyllrb.com/help/!*</span>  
jekyll-paginate를 설치한 후 다시 진행한다.  
=> `$sudo gem install jekyll-paginate`




## 관련된 Post
1. 원하는 Jekyll Theme를 적용하고 싶으면  
    [Jekyll을 이용한 .github.io 블로그 만들기_2](https://gmlwjd9405.github.io/2017/10/06/Jekyll-github.io-blog-2.html) 를 참고하시기 바랍니다.
2. 로컬에 있는 블로그 내용들을 GitHub Pages를 이용하여 Posting 하고 싶으면  
    [Jekyll을 이용한 .github.io 블로그 만들기_3](https://gmlwjd9405.github.io/2017/10/06/Jekyll-github.io-blog-3.html) 를 참고하시기 바랍니다.


## References
> - [http://jekyllrb-ko.github.io/docs/home/](http://jekyllrb-ko.github.io/docs/home/)
