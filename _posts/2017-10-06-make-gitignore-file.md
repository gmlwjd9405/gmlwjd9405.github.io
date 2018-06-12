---
layout: post
title: '[GitHub] .gitignore 설정하기'
subtitle: '.gitignore의 개념, github에 이미 올라가 있는 파일 삭제하기'
date: 2017-10-06
author: heejeong Kwon
cover: '/images/gitignore/gitignore-main.png'
tags: github gitignore
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - .gitignore의 개념 익히기
> - .gitignore 설정하기
> - github에 이미 올라가 있는 파일을 삭제하고 .gitignore 적용하기



## .gitignore 이란?
.gitignore 파일이란 **Git 버전 관리에서 제외할 파일 목록을 지정하는 파일** 이다.  
git으로 프로젝트를 관리할 때, 그 프로젝트 안의 특정파일들은 관리할 필요가 없는 경우가 있다. 예를 들면, 프로젝트 설정파일, 자동으로 생성되는 로그파일(ex.\*.log), 빌드할 때 생기는 컴파일된 파일(ex. \*.class) 등이 있다. 따라서 이런 관리할 필요가 없는 파일들을 git이 track 하지 않도록 .gitignore을 설정하는 것이다.



## .gitignore 설정하기
1. 우선 .git 파일이 있는 최상위 디렉터리로 이동한다. (.gitignore 파일은 종류에 상관없이 자신이 사용하는 에디터로, .git 파일이 존재하는 곳과 같은 디렉터리에 만들어 주면 된다.)
2. 터미널에서 아래의 명령어를 통해 .gitignore 파일을 만든다.
  * ~~~javascript
  // .gitignore 파일을 생성한다.
  $ touch .gitignore
  // .gitignore은 숨김 파일이므로 아래의 2가지 방법으로 제대로 생성됐는지를 확인한다.
  $ la
  $ ls -a
  // .gitignore 파일을 수정한다.
  $ vi .gitignore
  ~~~
3. .gitignore 파일에 내용 채우기
* GitHub에서 거의 모든 언어에 대한 .gitignore 파일을 미리 만들어서 제공하고 있다. [github/gitignore](https://github.com/github/gitignore)를 참고하여 .gitignore 안의 내용을 채우면 된다.
* 개인이 직접 .gitignore 파일을 설정하고 싶으면 [git-scm.com/docs/gitignore](https://git-scm.com/docs/gitignore)
를 참고하여 .gitignore 안의 내용을 채우면 된다.



## github에 이미 올라가 있는 파일을 삭제하고 .gitignore 적용하기
이미 버전 관리에 포함되어 있는 파일들을 .gitigore 파일에 기록한다고 해서 Git이 알아서 버전 관리에서 제외 하지는 않는다. 즉 Git이 계속해서 해당 파일을 track 하고 있다는 것이다. 예를 들어, 이미 자신의 github에 올라가 있는 파일을 삭제하고 더 이상 track 하고 싶지 않은 경우에는 수동으로 해당 파일들을 버전 관리에서 제외시켜줘야 한다.  
아래의 명령어를 사용하여 Git 버전 관리에서 수동으로 파일을 제외한다.
~~~javascript
// 현재 Repository의 cache를 모두 삭제한다.
$ git rm -r --cached .

// [File Name]에 해당하는 파일을 원격 저장소에서 삭제한다.
// (로컬 저장소에 있는 파일은 삭제하지 않는다.)
$ git rm -r --cached [File Name]
~~~

~~~javascript
// .gitignore에 넣은 파일 목록들을 제외하고 다른 모든 파일을 다시 track하도록 설정한다.
$ git add .
~~~
위의 작업 이후에 commit을 하지 않으면 아직 staged 상태로 남아있어, Git 버전 관리에서 완전히 빠지지 않으므로 아래의 명령어를 반드시 수행한다.
~~~javascript
$ git commit -m "Fixed untracked files"
~~~

## 관련된 Post
* github에 이미 올라가 있는 파일을 삭제하는 방법에 대해 더 자세히 알고 싶으시면 [github에 잘못 올라간 파일 삭제하기](https://gmlwjd9405.github.io/2018/05/17/git-delete-incorrect-files.html)를 참고하시기 바랍니다.

## References
> - [https://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository/](https://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)
> - [https://git-scm.com/docs/gitignore](https://git-scm.com/docs/gitignore)
> - [https://nesoy.github.io/articles/2017-01/Git-Ignore](https://nesoy.github.io/articles/2017-01/Git-Ignore)
> - [https://github.com/github/gitignore](https://github.com/github/gitignore)
> - [https://xho95.github.io/git/github/xcode/swift/2016/07/15/Making-a-.gitignore-file.html](https://xho95.github.io/git/github/xcode/swift/2016/07/15/Making-a-.gitignore-file.html)
