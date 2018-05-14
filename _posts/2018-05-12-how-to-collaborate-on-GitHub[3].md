---
layout: post
title: '[GitHub] GitHub로 협업하는 방법[3] - Gitflow Workflow'
subtitle: 'GitHub에서 Gitflow Workflow를 이용하여 협업하는 방법'
date: 2018-05-12
author: heejeong Kwon
cover: '/images/github-collaboration3/github-collaboration-main.png'
tags: github 협업방법
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

이 POST와 관련된 <mark>PDF 자료는</mark> [https://github.com/gmlwjd9405/git-collaboration](https://github.com/gmlwjd9405/git-collaboration)에서 다운받을 수 있습니다.
<br>(You can download the PDF file related to this POST from the following link.)

## Goal
> - Gitflow Workflow의 개념을 파악한다.
> - Gitflow Workflow로 협업하는 방법을 익힌다.

# Gitflow Workflow
* [nvie.com](nvie.com)의 '빈센트 드리센(Vincent Driessen)'이 제안한 것
* [Feature Branch Workflow](https://gmlwjd9405.github.io/2017/10/27/how-to-collaborate-on-GitHub-1.html)보다 복잡하지만, **대형 프로젝트에도 적용** 할 수 있는 좀 더 엄격한 작업 절차를 갖는다.
* Gitflow Workflow의 핵심 컨셉은 중앙 원격 저장소에서 **master와 develop, 두 개의 메인 브랜치** 를 이용한다는 것이다.
  * <span style="color:#4d0000">**master branch**</span>: 릴리스 이력을 관리하기 위해 사용. 즉, 배포 가능한 상태만을 관리한다.
  * <span style="color:#4d0000">**develop branch**</span>: 기능 개발을 위한 브랜치들을 병합하기 위해 사용.(모든 기능이 추가되고 버그가 수정되어 배포 가능한 상태라면 'master' 브랜치에 merge 한다.) 평소에는 이 브랜치를 기반으로 개발을 진행한다.
  * 다른 종류의 branch에 대한 이해는 다음의 POST를 참고하자. [<mark>branch의 종류</mark>](https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html)
* Gitflow Workflow도 Feature Branch Workflow와 같이 팀 구성원 간의 협업을 위한 창구로 중앙 원격 저장소를 사용한다. 즉, 다른 Workflow와 마찬가지로 로컬 브랜치에서 작업하고 중앙 원격 저장소에 푸시한다.


## 1. 중앙 원격 저장소, 자신의 원격 저장소, 로컬 저장소의 개념
* 중앙 원격(remote) 저장소  
  * 여러 명이 같은 프로젝트를 관리하는 데 사용하는 그룹 계정의 중립된 원격 저장소
    * Organization을 만드는 방법은 GitHub 페이지 오른쪽 위에 있는 "+" 아이콘을 클릭하고 메뉴에서 "New organization"을 선택하면 된다.
    * Organizatoin의 사용자와 저장소는 팀으로 관리되고 저장소의 권한 설정도 팀으로 관리한다.
* 자신의 원격(remote) 저장소
  * *remote repository* 라고 불린다.
  * 파일이 GitHub 전용 서버에서 관리되는 원격 저장소
* 로컬(local) 저장소
  * *local repository* 라고 불린다.
  * 내 PC에 파일이 저장되는 개인 전용 저장소, 지역 저장소
![](/images/github-collaboration3/github-collaboration-1.png)
![](/images/github-collaboration3/github-collaboration-2.png)

## 2. 프로젝트 참여자는 git clone 명령으로 로컬 저장소를 만든다.
git clone 명령으로 중앙 원격 저장소(remote repository)를 복제하여 자신의 로컬 저장소(local repository)를 만들 수 있다. 프로젝트 참여자는 이 로컬 저장소에서 작업을 수행한다.
~~~javascript
// 터미널에서 자신의 원하는 디렉터리 이동한 후 아래의 명령어를 입력
/**
해당 디렉터리
  --하위--> remote repository와 동일한 이름의 디렉터리 생성
**/
$ git clone [중앙 remote repository URL]
~~~

* git clone 명령은 아래의 명령들을 포함한 작업이다.

~~~javascript
//해당 디렉터리를 빈 Git 저장소로 만드는 작업
$ git init
// 현재 작업 중인 Git 저장소에 팀의 중앙 원격 저장소를 추가한다. 이름을 origin으로 짓고 긴 서버 주소(URL) 대신 사용한다.
$ git remote add origin [중앙 remote repository URL]
// 중앙 원격 저장소(origin)의 master 브랜치 데이터를 로컬에 가져오기만 하는 작업
$ git fetch origin master
~~~

<mark>fetch와 pull의 차이</mark>  
* fetch: 원격 저장소의 데이터를 로컬에 가져오기만 하기
* pull: 원격 저장소의 내용을 가져와 자동으로 병합 작업을 실행
* 즉, 단순히 원격 저장소의 내용을 확인만 하고 로컬 데이터와 병합은 하고 싶지 않은 경우에는 fetch 명령어를 사용한다.
* <span style="color:#4d0000">**pull = fetch + merge**</span>  

![](/images/github-collaboration3/github-collaboration-3.png)
![](/images/github-collaboration3/github-collaboration-4.png)


## 3. 가장 먼저 할 일은 Develop Branch를 만드는 것이다.
'master' 브랜치를 기준으로 develop 브랜치를 만든다.

### 방법 [1]: GUI 도구를 이용한 생성(이 방법을 추천한다!)
![](/images/github-collaboration3/github-collaboration-5.png)
* GitHub 페이지에서 'Branch:master'를 클릭한 후 새로 생성할 브랜치의 이름('develop')을 적는다.
  * (이때, Branch를 생성할 수 있는 사용자는 Owner 권한이 있는 사용자)
![](/images/github-collaboration3/github-collaboration-6-1.png)

* 새로 생성한 'develop' branch를 default branch로 설정해야한다.
  * ![](/images/github-collaboration3/github-collaboration-6-2.png)
  * ![](/images/github-collaboration3/github-collaboration-6-3.png)

* <mark>'develop' branch를 default branch로 설정하는 이유?</mark>
  * 평소에는 ‘develop’ branch를 기반으로 개발을 진행하기 때문에 <br> <span style="background-color: #e1e1e1">$ git push origin some-feature</span>(내 로컬 저장소의 some-feature branch를 중앙 원격 저장소로 올리는 명령)를 한 후, Github 페이지에서 해당 some-feature branch에 대해 merge를 할 때 중앙 원격 저장소의 'master' branch가 아닌 default로 설정되어 있는 ‘develop’에 병합하도록 설정하는 것이다.


### 방법 [2]: 팀 구성원 중 한 명이 자신의 로컬 저장소에 빈 develop 브랜치를 만들고 중앙 저장소로 푸시
![](/images/github-collaboration3/github-collaboration-7.png)
~~~javascript
$ git branch develop
$ git push -u origin develop
~~~
1. GitHub 페이지에서 자신이 push한 ‘develop’ branch를 병합해달라는 pull request를 날린다.
  * ![](/images/github-collaboration3/github-collaboration-7-1.png)
2. 프로젝트 관리자는 해당 pull request를 merge하여 새로운 ‘develop’ branch를 중앙 원격 저장소에 생성한다.
  * ![](/images/github-collaboration3/github-collaboration-7-2.png)
3. 방법 [1]과 같은 방법으로 새로 생성한 'develop' branch를 default branch로 설정한다.


## 4. 팀 구성원 모두가 Gitflow Workflow를 적용할 준비를 한다.
이제 팀 구성원들은 중앙 저장소를 복제(처음에 clone 했으면 넘어감)하고, 중앙 저장소와 연결된 개발 브랜치를 만들어야 한다.
![](/images/github-collaboration3/github-collaboration-8.png)
~~~javascript
$ git checkout -b develop origin/develop
~~~
이제 팀 구성원 모두가 이 워크플로우를 적용하기 위한 준비가 되었다고 가정하자.


## 5. 설명을 위해 현재 로컬에서 작업 중인 branch 위치를 표시한다.
중앙 원격 저장소에는 master, develop branch가 있고, 자신의 로컬 저장소에는 master, develop branch와 로그인 기능을 구현할 feature/login branch(아래에서 설명)가 있다고 가정한다.
<br>
또한 현재는 master branch에서 작업 중이라고 가정하고 아래와 같이 작업 중인 위치를 표시한다. (Gitflow Workflow에서는 대부분의 작업이 develop branch에서 이루어진다.)
![](/images/github-collaboration3/github-collaboration-9.png)


## 6. 새로운 기능 개발을 위해 격리된 branch를 만든다.
로컬 저장소에서 branch를 따고, 코드를 수정하고, 변경 내용을 커밋한다.
<br>
이때, 'master' branch에서 기능 개발을 위한 브랜치를 따는 것이 아니라, **'develop' branch에서** 따야한다.
~~~javascript
$ git checkout -b [branch name] develop

# 위의 명령어는 아래의 두 명령어를 합한 것
$ git branch [branch name] develop
$ git checkout [branch name]
~~~
![](/images/github-collaboration3/github-collaboration-10.png)



## 7. 로컬 저장소의 새로운 기능 브랜치를 중앙 원격 저장소(remote repository)에 푸시한다.
* 새로 만든 브랜치(feature/login branch)에 새로운 기능에 대한 내용을 커밋한다.

~~~javascript
$ git commit -a -m "Write commit message"

# 위의 명령어는 아래의 두 명령어를 합한 것
$ git add . # 변경된 모든 파일을 스테이징 영역에 추가
$ git add [some-file] # 스테이징 영역에 some-file 추가
$ git commit -m "Write commit message" # local 작업폴더에 history 하나를 쌓는 것
~~~

* 커밋을 완료했다면, 내가 작업한 내용을 포함한 브랜치(feature/login branch)를 중앙 원격 저장소에 올린다.
* 이는 로컬 저장소의 백업 역할을 할 뿐만 아니라, 다른 팀 구성원들이 나의 작업 내용과 진도를 확인할 수도 있어 좋은 습관이라 할 수 있다.

### 방법 [1]: 팀이 "풀 리퀘스트(pull request)"를 이용하는 경우,
* 로컬 저장소의 새로운 기능 브랜치를 중앙 원격 저장소(remote repository)에 푸시한 후, 프로젝트 관리자(소규모 팀에서는 모두가 관리자가 될 수 있음)에게 자신의 기여분을 반영해 달라는 풀 리퀘스트(pull request)를 던진다.
* 새로 만든 기능 개발용 브랜치도 중앙 저장소에 올려서 팀 구성원들과 개발 내용에 대한 의견(코드 리뷰 등)을 나눌 수 있다.
* 이후에는 모든 팀원이 변경한 코드 내용을 확인하고 마지막으로 확인한 팀원 또는 프로젝트 관리자가 변경 내용을 중앙 원격 코드 베이스에 병합(merge)하는 작업을 한다.
* 이는 일종의 로컬 저장소 백업 역할을 하기도 한다.

* **풀 리퀘스트(Pull requests)란?**
  * 기능 개발을 끝내고 master에 바로 병합(merge)하는 것이 아니라, 브랜치를 중앙 원격 저장소에 올리고 병합(merge)해달라고 요청하는 것

![](/images/github-collaboration3/github-collaboration-11-1.png)
* GUI 도구를 이용한 풀 리퀘스트(pull request)
1. GitHub 페이지에서 "Pull requests" 버튼을 이용하면, 어떤 branch를 제출할지 정할 수 있다.
2. 기능을 구현한 branch(여기서는 feature/login branch)를 프로젝트 중앙 원격 저장소의 'develop' branch에 병합해 달라고 요청한다.

![](/images/github-collaboration3/github-collaboration-11-2.png)
* GUI 도구를 이용한 병합(merge)
1. GitHub 페이지에서 "Pull requests" 버튼을 누른 후, File changed 탭에서 변경 내용을 확인한다.
2. Conversation 탭으로 이동하여 "Confirm merge"를 하면 중앙 원격 코드 베이스('develop' branch)에 병합(merge)된다.
  * 위에서 'develop' branch를 default branch로 설정했기 때문에 자동으로 'develop' branch로 merge된다.
3. 충돌이 일어난 경우는 팀원들고 합의 하에 충돌 내용을 수정한 후 병합을 진행한다.


### 방법 [2]: 팀이 "풀 리퀘스트(pull request)"를 이용하지 않는 경우,
* 기능 브랜치를 병합하기 전에 반드시 자신의 로컬 저장소 develop branch에 중앙 원격 저장소의 변경 내용을 반영해서 최신 상태로 만들어야 한다.
* 또한 자신이 직접 새로운 기능에 대한 병합을 할 때, 'master' branch에 바로 병합하지 않도록 주의해야 한다.
![](/images/github-collaboration3/github-collaboration-12-1.png)
![](/images/github-collaboration3/github-collaboration-12-2.png)
![](/images/github-collaboration3/github-collaboration-12-3.png)

~~~javascript
# -u 옵션: 새로운 기능 브랜치와 동일한 이름으로 중앙 원격 저장소의 브랜치로 추가한다.

// 로컬의 기능 브랜치를 중앙 원격 저장소 (origin)에 올린다.
$ git push -u origin feature/login branch

// -u 옵션으로 한 번 연결한 후에는 옵션 없이 아래의 명령만으로 기능 브랜치를 올릴 수 있다.
$ git push -origin feature/login branch
~~~

<!-- ![](/images/github-collaboration3/github-collaboration-13.png) -->

## 8. 중앙 원격 저장소와 자신의 로컬 저장소를 동기화하기 위해 로컬 저장소의 branch를 develop branch로 이동한다.
~~~javascript
// 로컬 저장소의 branch를 develop branch로 이동
$ git checkout develop
~~~
![](/images/github-collaboration3/github-collaboration-13.png)


## 9. 중앙 원격 저장소의 코드 베이스에 새로운 커밋이 있다면 다음과 같이 가져온다.
중앙 원격 저장소(origin)의 메인 코드 베이스('develop' branch)가 변경되었으므로, 프로젝트 참여하는 모든 개발자가 자신의 로컬 저장소를 동기화해서 최신 상태로 만들어야 한다.
~~~javascript
$ git pull origin develop
~~~
![](/images/github-collaboration3/github-collaboration-14.png)
![](/images/github-collaboration3/github-collaboration-15.png)


## 10. 새로운 기능을 추가하기 위해서 그 작업에 대한 branch를 생성하여 작업한다.
중앙 원격 저장소와 동기화된 로컬 저장소의 'develop' branch에서 새로운 작업에 대한 branch를 생성하여 다른 작업을 시작한다.
![](/images/github-collaboration3/github-collaboration-16.png)
* local에서 완성한 이전 작업 브랜치는 삭제한다.
~~~javascript
$ git branch -d feature/login
~~~

---
### <span style="color:#4d0000">***Feature Branch Workflow라면 develop 브랜치에 개발한 기능을 병합하는 것으로 모든 과정이 끝날테지만, Gitflow Workflow는 아직 할 일이 더 남아 있다.***</span>
---


## 11. 배포하기
만약 ‘develop’ 브랜치에서 버전 1.2에 대한 기능이 모두 구현이 되었으면 배포를 위한 전용 브랜치를 사용하여 배포 과정을 캡슐화한다. 이렇게 함으로써 한 팀이 해당 배포를 준비하는 동안 다른 팀은 다음 배포를 위한 기능 개발을 계속할 수 있다.
<br>
버전 번호를 부여한 새로운 'release' branch를 'develop' branch로부터 생성한다.

![](/images/github-collaboration3/github-collaboration-17-1.png)
~~~javascript
// 'develop' 브랜치로부터 release 브랜치(release-1.2)를 생성
$ git checkout -b release-1.2 develop
~~~
* 이렇게 release 브랜치를 만드는 순간부터 배포 사이클이 시작된다.
  * release 브랜치에서는 배포를 위한 최종적인 버그 수정, 문서 추가 등 릴리스와 직접적으로 관련된 작업을 수행한다.
  * 직접적으로 관련된 작업들을 제외하고는 release 브랜치에 새로운 기능을 추가로 병합(merge)하지 않는다.

![](/images/github-collaboration3/github-collaboration-17-2.png)
![](/images/github-collaboration3/github-collaboration-17-3.png)
* ‘release’ 브랜치에서 배포 가능한 상태가 되면(배포 준비가 완료되면),
  * 배포 가능한 상태: 새로운 기능을 포함한 상태로 모든 기능이 정상적으로 동작 하는 상태

1. ‘master’ 브랜치에 병합한다. (이때, 병합한 커밋에 Release 버전 태그를 부여!)
2. 배포를 준비하는 동안 release 브랜치가 변경되었을 수 있으므로 배포 완료 후 ‘develop’ 브랜치에도 병합한다.
3. 작업했던 release 브랜치는 삭제한다.
이때, 다음 번 배포(Release)를 위한 개발 작업은 ‘develop’ 브랜치에서 계속 진행해 나간다.

### 방법 [1]: 팀이 "풀 리퀘스트(pull request)"를 이용하지 않는 경우,
~~~javascript
/* release 브랜치에서 배포 가능한 상태가 되면 */
// 'master' 브랜치로 이동한다.
$ git checkout master
// 'master' 브랜치에 release-1.2 브랜치 내용을 병합(merge)한다.
# --no-ff 옵션: 위의 추가 설명 참고
$ git merge --no-ff release-1.2
// 병합한 커밋에 Release 버전 태그를 부여한다.
$ git tag -a 1.2
// 'master' 브랜치를 중앙 원격 저장소에 올린다.
$ git push origin master

/* 'release' 브랜치의 변경 사항을 'develop' 브랜치에도 적용 */
// 'develop' 브랜치로 이동한다.
$ git checkout develop
// 'develop' 브랜치에 release-1.2 브랜치 내용을 병합(merge)한다.
$ git merge --no-ff release-1.2
// 'develop' 브랜치를 중앙 원격 저장소에 올린다.
$ git push origin develop

// -d 옵션: release-1.2에 해당하는 브랜치를 삭제한다.
$ git branch -d release-1.2
~~~

### 방법 [2]: 팀이 "풀 리퀘스트(pull request)"를 이용하는 경우,
팀이 풀 리퀘스트를 통한 코드 리뷰하는 방식을 사용한다면 release 브랜치를 그대로 중앙 원격 저장소에 올린 후 다른 팀원들의 확인을 거쳐 'master'와 'develop' branch에 병합한다.



## 12. 버그 수정하기

배포한 버전에 긴급하게 수정을 해야 할 필요가 있을 경우, ‘master’ 브랜치에서 직접 브랜치('hotfix' 브랜치)를 만들어 필요한 부분만을 수정한 후 다시 ‘master’브랜치에 병합하여 이를 배포해야 한다.
<br>
‘develop’ 브랜치에서 문제가 되는 부분을 수정하여 배포 가능한 버전을 만들기에는 시간도 많이 소요되고 안정성을 보장하기도 어렵기 때문이다.

![](/images/github-collaboration3/github-collaboration-18-1.png)
![](/images/github-collaboration3/github-collaboration-18-2.png)
~~~javascript
// release 브랜치(hotfix-1.2.1)를 'master' 브랜치(유일!)에서 분기
$ git checkout -b hotfix-1.2.1 master

/* ~ 문제가 되는 부분만을 빠르게 수정 ~ */

/* 필요한 부분을 수정한 후 */
// 'master' 브랜치로 이동한다.
$ git checkout master
// 'master' 브랜치에 hotfix-1.2.1 브랜치 내용을 병합(merge)한다.
$ git merge --no-ff hotfix-1.2.1
// 병합한 커밋에 새로운 버전 이름으로 태그를 부여한다.
$ git tag -a 1.2.1
// 'master' 브랜치를 중앙 원격 저장소에 올린다.
$ git push origin master

/* 'hotfix' 브랜치의 변경 사항을 'develop' 브랜치에도 적용 */
// 'develop' 브랜치로 이동한다.
$ git checkout develop
// 'develop' 브랜치에 hotfix-1.2.1 브랜치 내용을 병합(merge)한다.
$ git merge --no-ff hotfix-1.2.1
// 'develop' 브랜치를 중앙 원격 저장소에 올린다.
$ git push origin develop

// -d 옵션: hotfix-1.2.1에 해당하는 브랜치를 삭제한다.
$ git branch -d hotfix-1.2.1
~~~

1. 배포한 버전에 긴급하게 수정을 해야 할 필요가 있을 경우,
  * ‘master’ 브랜치에서 hotfix 브랜치를 분기한다. (‘hotfix’ 브랜치만 master에서 바로 딸 수 있다.)
2. 문제가 되는 부분만을 빠르게 수정한다.
3. 다시 ‘master’ 브랜치에 병합(merge)하여 이를 안정적으로 다시 배포한다.
4. 새로운 버전 이름으로 태그를 매긴다.
5. hotfix 브랜치에서의 변경 사항은 ‘develop’ 브랜치에도 병합(merge)한다.
6. 작업했던 hotfix 브랜치는 삭제한다.



# 관련된 Post
* 소규모의 프로젝트에서 많이 사용하는 방식의 Github 협업 방법을 알고 싶으시면 [Feature Branch Workflow 방법](https://gmlwjd9405.github.io/2017/10/27/how-to-collaborate-on-GitHub-1.html) 을 참고하시기 바랍니다.
* 오픈 소스 프로젝트에서 많이 사용하는 방식의 Github 협업 방법을 알고 싶으시면 [Forking Workflow 방법](https://gmlwjd9405.github.io/2017/10/28/how-to-collaborate-on-GitHub-2.html) 을 참고하시기 바랍니다.
* branch의 종류에 대해 자세히 알고 싶으시면 [branch의 종류](https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html) 를 참고하시기 바랍니다.


# References
> - [http://blog.appkr.kr/learn-n-think/comparing-workflows/](http://blog.appkr.kr/learn-n-think/comparing-workflows/)
> - [https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
> - [https://github.com/Taeung/git-training](https://github.com/Taeung/git-training)
> - [https://backlog.com/git-tutorial/kr/stepup/stepup3_2.html](https://backlog.com/git-tutorial/kr/stepup/stepup3_2.html)
> -[https://mylko72.gitbooks.io/git/content/branch/branch_type.html](https://mylko72.gitbooks.io/git/content/branch/branch_type.html)
