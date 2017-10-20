---
layout: post
title: 'Jekyll을 이용한 .github.io 블로그 만들기[2]'
subtitle: 'Jekyll Theme 적용하기'
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



## Jekyll Theme 고르기
**아래의 사이트에서 자신이 원하는 Jekyll Theme를 선택한다.**  
[http://jekyllthemes.org/](http://jekyllthemes.org/)
![](/images/JekyllStart2/JekyllStart2-jekyll-theme.png)


## Jekyll 적용하기
1. 원하는 Jekyll Theme를 다운로드(.zip)한다.
  * Jekyll 사이트에서 직접 다운로드한다.
  * 혹은 연결된 GitHub에서 Clone or download -> Download ZIP 을 통해 다운로드한다.
1. 압축을 푼다.
1. Jekyll 블로그를 저장한 로컬 디렉터리에 압축 푼 내용을 모두 복사하여 넣는다.
  * ex. ~/blog/[github 사용자명].github.io
  * Gemfile, Gemfile.lock은 삭제한다.
   ![](/images/JekyllStart2/JekyllStart2-directory-structure.png)
1. 아래 명령어를 통해 적용된 테마를 확인할 수 있다.
~~~javascript
~/blog/[github 사용자명].github.io$ jekyll serve
# => 개발용 서버가 http://localhost:4000/ 에서 구동된다.
# 자동-재생성: 활성화됨. 비활성화 하려면 `$ jekyll serve --no-watch`를 이용
~~~
* 이 명령어를 통해 Jekyll이 제공하는 서버를 실행시킬 수 있다.
* 자동으로 내용의 변화를 감지하고 재생성해준다.
* 블로그 사이트가 제대로 구성되어 있는지 로컬에서 미리 확인할 수 있다.  

![](/images/JekyllStart2/JekyllStart2-jekyll-H2OTheme.png)

---
<mark>Error</mark>  
<span style="color:#4d0000">*만약 아래와 같이 빈 화면이 나타난다면*</span>  
![](/images/JekyllStart2/JekyllStart2-empty-screen.png)
![](/images/JekyllStart2/JekyllStart2-directory-structure2.png)
=> index.md를 삭제한 후 다시 진행한다.

---
<mark>Error</mark>  
<span style="color:#4d0000">*Error: Address already in use - bind(2) for 127.0.0.1:4000*</span>  
=> 터미널을 재실행한다.


## Jekyll 기본 디렉터리 구조
![](/images/JekyllStart2/JekyllStart2-directory-structure3.png)
* \_config.yml  
  * 환경설정 정보를 보관한다.
* \_layouts  
  * 포스트를 포장할 때 사용하는 템플릿이다. 즉, 기본 레이아웃이다.
* \_posts
  * 자신이 작성한 Post들을 보관한다. 중요한 것은 파일들의 명명규칙인데, 반드시 다음 형식을 따라야 한다: 'YEAR-MONTH-DAY-title.md' 고유주소는 포스트 별로 각각 정의할 수 있지만, 날짜와 마크업 언어 종류는 오로지 파일명에 의해 결정된다.
* \_site
  * 생성된 블로그 사이트가 저장되는 경로이다. 즉, Jekyll 이 변환 작업을 마친 뒤 생성된 사이트가 저장되는 (디폴트) 경로이다. 대부분의 경우, 이 경로를 .gitignore 에 추가한다.
* .jekyll-metadata
  * Jekyll 은 이 파일을 참고하여, 마지막으로 빌드한 이후에 한번도 수정되지 않은 파일은 어떤 것인지, 다음 빌드 때 어떤 파일을 다시 생성해야 하는지 판단할 수 있다. 생성된 사이트에 이 파일이 복사되지는 않는다. 대부분의 경우, 이 파일을 .gitignore 에 추가한다.  

Jekyll 기본 디렉터리 구조에 대한 자세한 개념은 [http://jekyllrb-ko.github.io/docs/structure/](http://jekyllrb-ko.github.io/docs/structure/)를 참고하자.

## Posting 하기
\_posts 디렉터리 안에 반드시 'YYYY-MM-DD-[Post 이름].md'라는 파일명으로 파일을 생성한 후,
markdown 문법을 이용하여 Post를 작성한다. (물론, Textile 또는 일반 HTML 등 자신이 즐겨 사용하는 마크업 언어로 문서를 작성할 수 있다.)  


## 관련된 Post
1. Jekyll에 대한 개념과 설치 및 실행 방법을 익히고 싶으면  
    [Jekyll을 이용한 .github.io 블로그 만들기_1](https://gmlwjd9405.github.io/2017/10/06/Jekyll-github.io-blog-1.html) 를 참고하시기 바랍니다.
2. 로컬에 있는 블로그 내용들을 GitHub Pages를 이용하여 Posting 하고 싶으면  
    [Jekyll을 이용한 .github.io 블로그 만들기_3](https://gmlwjd9405.github.io/2017/10/06/Jekyll-github.io-blog-3.html) 를 참고하시기 바랍니다.


## References
> - [http://jekyllrb-ko.github.io/docs/home/](http://jekyllrb-ko.github.io/docs/home/)
> - [http://jekyllthemes.org/](http://jekyllthemes.org/)
> - [http://jekyllrb-ko.github.io/docs/structure/](http://jekyllrb-ko.github.io/docs/structure/)
