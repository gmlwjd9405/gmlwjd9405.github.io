---
layout: post
title: '[Git] Github에 잘못 올라간 파일 삭제하기'
subtitle: '.gitignore을 설정하지 않고 github remote에 push한 경우'
date: 2018-05-17
author: heejeong Kwon
cover: '/images/git-delete-incorrect-files/git-delete-incorrect-files-main.png'
tags: git 명령어
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - github에 잘못 올라간 파일을 삭제할 수 있다.
> - .gitignore을 설정하지 않고 github remote에 push한 경우, 잘못 올라간 파일을 삭제할 수 있다.

<br>
**들어가기 전**

예를 들어, IntelliJ IDE를 사용할 때 .out 폴더를 .gitignore에 넣지 않고 원격 저장소에 push했다고 가정하자.
(*IntelliJ의 .out 폴더:*  빌드/컴파일 시 .class 파일이 포함된 프로젝트의 출력이 포함되는 위치)
<br>
<br>


## Github에 잘못 올라간 파일 삭제 과정
### 1. 원격 저장소에서 파일 삭제하기

이미 github remote에 push를 했기 때문에 로컬의 저장소에서 파일을 삭제해도 원격 저장소에서는 삭제되지 않는다.
* git rm VS git rm --cached
~~~javascript
// 원격 저장소와 로컬 저장소에 있는 파일을 삭제한다.
$ git rm [File Name]
// 원격 저장소에 있는 파일을 삭제한다. 로컬 저장소에 있는 파일은 삭제하지 않는다.
$ git rm --cached [File Name]
~~~

따라서 아래와 같이 <span style="background-color: #e1e1e1">git rm --cached [File Name]</span> 명령어를 이용하여 원격 저장소에서 잘못 올라간 파일을 삭제해야 한다.

~~~javascript
// .idea/modules.xml 파일 삭제
$ git rm --cached .idea/modules.xml
// .idea 폴더 하위의 모든 파일 삭제 
$ git rm --cached -r .idea/
~~~

### 2. .gitignore 설정하기
만약 .gitignore가 제대로 설정되어 있지 않다면 .gitigore 설정하여 다음에는 개인이 관리해야되는 파일들이 원격 저장소에 올라가지 않도록 해야한다. [.gitignore 설정하기](https://gmlwjd9405.github.io/2017/10/06/make-gitignore-file.html) 를 참고하여 .gitignore를 설정한다.

<mark>TIP)</mark> 또한 .gitignore는 git add 명령어 전에 설정되어 있어야 적용이 가능하다는 것을 알아두자.

### 3. 원격 저장소에 적용하기
버전 관리에서 완전히 제외하기 위해서는 반드시 commit 명령어와 push를 수행해야 한다.

~~~javascript
// 버전 관리에서 완전히 제외하기 위해서는 반드시 commit 명령어를 수행해야 한다.
$ git commit -m "Fixed untracked files"
// 원격 저장소(origin)에 push
$ git push origin master
~~~

### [참고] Github에 올라간 Commit 내역까지 삭제하는 방법 
[git 명령어 실수 해결하기](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html)에서 **git push 취소하기**를 참고하시기 바랍니다. 

## git rm 옵션 참고
![](/images/git-delete-incorrect-files/rm-options.png)


# 관련된 Post
* .gitignore 설정하는 방법에 대해 알고 싶으시면 [.gitignore 설정하기](https://gmlwjd9405.github.io/2017/10/06/make-gitignore-file.html)를 참고하시기 바랍니다.
* git add / git commit / git push를 취소하는 방법에 대해 알고 싶으시면 [git 명령어 실수 해결하기](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html)를 참고하시기 바랍니다. 


# References
> - [https://devlog.github.io/git/2015/12/23/git-ignore-tracked-files.html](https://devlog.github.io/git/2015/12/23/git-ignore-tracked-files.html)
> - [http://mobicon.tistory.com/266](http://mobicon.tistory.com/266)
> - [http://mygumi.tistory.com/103](http://mygumi.tistory.com/103)
