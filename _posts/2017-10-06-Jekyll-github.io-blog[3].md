---
layout: post
title: 'Jekyll을 이용한 .github.io 블로그 만들기[3]'
subtitle: 'GitHub Pages에 .github.io 블로그 만들기'
date: 2017-10-06
author: Jekyll
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: jekyll github
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Requirements
> - GNU/Linux, Unix, or macOS
> - Ruby version 2.1 or above, including all development headers
> - RubyGems  


## Github에 저장소(Repository) 생성하기
GitHub Pages에 .github.io 블로그를 만드는 방법은 단순히 <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span>라는 이름의 원격 저장소(Repository)를 만들면 된다.  
방법은 [GitHub Pages](https://pages.github.com/)에 있는 ①번을 참고하자. 우선 ②~④번은 수행하지 않는다. 또한 Initialize this repository with a README도 체크하지 않는 것으로 설정한다.   

![](/images/JekyllStart3/JekyllStart3-create-repository.png)

이때, Jekyll 블로그를 로컬에서 실행시킨 사람은 이미 <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span>라는 디렉터리가 있기 때문에 github 원격 저장소(Repository)를 clone 하지 않는다.
아래에서 자세하게 설명한다.

## .gitignore 설정하기
Jekyll 블로그를 로컬에서 실행시킨 경우 git이 추적 할 필요가 없는 파일들(ex. \_site)이 많이 생성되기 때문에 github에 Post를 올리기 전에 미리 .gitignore 설정해 두는 것이 좋다.  
터미널에서 <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span> 디렉토리로 이동한 후 .gitignore 파일을 생성한다.
.gitignore 파일에 아래의 내용을 넣고 저장한다.
~~~javascript
$ cd ~/blog/[github 사용자명].github.io
~/blog/[github 사용자명].github.io$ vi .gitignore
~~~

~~~javascript
## MacOS
*.DS_Store
.bundle
.sass-cache
node_modules
package.json

## Jekyll stuff
/_site/
_site/
.sass-cache/
.jekyll-metadata
.jekyll
Gemfile
Gemfile.lock
~~~

---
<mark>TIP</mark>  
<span style="color:#4d0000">*만약 이미 commit했거나 push(이미 github에 파일이 올라간 경우)한 경우 .gitignore 적용하기*</span>  
=> 만약 이미 commit했거나 push 했어도 상관없다. 아래의 Post를 참고하여 github에 올라가 있던 것을 삭제하고 .gitignore 파일을 적용할 수 있다.  
=> [github에 이미 올라가 있는 파일을 삭제하고 .gitignore 적용하기](https://gmlwjd9405.github.io/2017/10/06/make-gitignore-file.html)에 대한 Post를 참고하자.



## .github.io 블로그의 로컬 저장소와 github 저장소를 연결하기
이미 로컬 <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span> 디렉터리에 아래의 그림과 같은 내용이 있는지 없는지에 따라 다음으로 수행할 작업이 달라진다.
  ![](/images/JekyllStart3/JekyllStart3-directory-structure.png)

#### [1.으로 이동]
- <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span> 디렉터리 안에 위와 같은 내용이 이미 존재하는 사용자
- `'jekyll new [github 사용자명].github.io'`를 이용하여 Jekyll 블로그를 로컬에서 실행시킨 사용자

#### [2.으로 이동]
- <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span> 디렉터리 안에 위와 같은 내용이 존재하지 않은 사용자
- 아예 <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span> 디렉터리가 존재하지 않은 사용자
- Jekyll 블로그를 로컬에 설치하지 않은 사용자
- 먼저 remote 저장소를 clone한 후에 Jekyll 블로그를 설정하려는 사용자

1. 단순히 git의 remote 저장소와 연결해주는 작업만 하면 된다.  
  * ***~/blog/[github 사용자명].github.io***
  로 이동하여 아래의 명령어를 입력하면 앞으로 git은 해당 디렉터리의 변화를 감지하여 track할 수 있고 로컬에서 작성한 블로그를 GitHub에 호스팅할 수 있다.
  (git init 명령을 실행한 디렉터리를 working directory라고 부른다.)
  ~~~javascript
  $ git init // 해당 디렉터리 안에 .git이라는 하위 디렉토리를 만든다.
  $ git remote add origin [원격 저장소 URL] // git의 remote 저장소와 연결한다.
  ~~~
  * 3번으로 이동하여 계속 진행한다.
2. 위에서 생성한 remote 저장소를 clone 하는 작업을 수행한다.
  * 우선 ***~/blog***
  아래에 <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span> 디렉터리를 삭제한다.(clone을 하면 자동으로 디렉터리가 생성되므로 안에 아무 내용이 없다면 삭제해놓자.)
  * ***~/blog***
  로 이동하여 remote 저장소를 Clone하면 ~/blog 하위에 remote 저장소와 동일한 이름(<span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span>)의 디렉터리가 생긴다. 또한 origin이라는 이름으로 remote 저장소가 자동으로 등록되고 해당 디렉터리 안에 .git이라는 하위 디렉토리를 만들어 주기 때문에 위의 명령어 두 개를 모두 포함한다고 생각하면 된다.
  ~~~javascript
  // 서버에 있는 모든 데이터를 복사한다. 즉 Git 저장소를 복사한다.
  $ git clone [원격 저장소 URL]
  ~~~
  * clone한 후에 Jekyll 블로그를 설정하기 위해서는 해당 디렉터리(<span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span>)가 비어있어야 한다.
  * Jekyll 블로그를 설정하기 위해서는 ***~/blog***
  로 이동하여 `'jekyll new [github 사용자명].github.io'` 명령어를 통해 Jekyll 블로그를 생성한다.
  * 이외의 Jekyll 설정에 대한 내용은 아래의 관련 Post를 참고하자.
  * 3번으로 이동하여 계속 진행한다.
3. Git이 파일을 관리하게 하려면 저장소에 파일을 추가하고 커밋해야 한다. 아래의 명령으로 파일을 추가하고 커밋한다.  
~~~javascript
$ git add .
$ git commit -m "Initial commit"
$ git push origin master // 원격 저장소에 변경 내용을 올린다.
~~~


## .github.io 블로그 확인하기
브라우저에서 <span style="background-color: #e1e1e1">'[github 사용자명].github.io'</span>라고 주소를 입력하면 자신의 .github.io 블로그를 확인할 수 있다.
![](/images/JekyllStart3/JekyllStart3-heejeong-blog-main.png)


## 관련된 Post
1. Jekyll에 대한 개념과 설치 및 실행 방법을 익히고 싶으면  
    [Jekyll을 이용한 .github.io 블로그 만들기_1](https://gmlwjd9405.github.io/2017/10/06/Jekyll-github.io-blog-1.html) 를 참고하시기 바랍니다.
2. 원하는 Jekyll Theme를 적용하고 싶으면  
    [Jekyll을 이용한 .github.io 블로그 만들기_2](https://gmlwjd9405.github.io/2017/10/06/Jekyll-github.io-blog-2.html) 를 참고하시기 바랍니다.
3. .gitignore 파일에 대한 설정에 대해 알고 싶으면  
    [.gitignore 설정하기](https://gmlwjd9405.github.io/2017/10/06/make-gitignore-file.html) 를 참고하시기 바랍니다.



## References
> - [https://pages.github.com/](https://pages.github.com/)
> - [https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/)
> - [https://git-scm.com/book/ko/v1/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-Git-%EC%A0%80%EC%9E%A5%EC%86%8C-%EB%A7%8C%EB%93%A4%EA%B8%B0](https://git-scm.com/book/ko/v1/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-Git-%EC%A0%80%EC%9E%A5%EC%86%8C-%EB%A7%8C%EB%93%A4%EA%B8%B0)
