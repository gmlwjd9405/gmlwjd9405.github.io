---
layout: post
title: '[GitHub] Git 브랜치의 종류 및 사용법 (5가지)'
subtitle: 'Gitflow Workflow에서 사용하는 Git Branch 종류를 이해한다.'
date: 2018-05-11
author: heejeong Kwon
cover: '/images/types-of-git-branch/types-of-git-branch-main.png'
tags: github 협업방법
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Gitflow Workflow에서 사용하는 Git Branch 종류를 이해한다.
> - Gitflow Workflow에서 사용하는 Git Branch 사용법을 이해한다.

# Git Branch 종류 (5가지)
Gitflow Workflow에서는 항상 유지되는 메인 브랜치들(master, develop)과 일정 기간 동안만 유지되는 보조 브랜치들(feature, release, hotfix)을 포함하여 총 5가지의 브랜치를 사용한다.
<br>
아래는 [Gitflow Workflow 방법](https://gmlwjd9405.github.io/2018/05/12/how-to-collaborate-on-GitHub-3.html)에서 사용하는 브랜치의 흐름이다.
![](/images/types-of-git-branch/total-branch.png)

## 1. Master Branch
<span style="color:#4d0000">**제품으로 출시될 수 있는 브랜치**</span>
<br>
배포(Release) 이력을 관리하기 위해 사용.
즉, 배포 가능한 상태만을 관리한다.
![](/images/types-of-git-branch/develop-branch.svg){: width="500" height="350"}



## 2. Develop Branch
<span style="color:#4d0000">**다음 출시 버전을 개발하는 브랜치**</span>
<br>
기능 개발을 위한 브랜치들을 병합하기 위해 사용.
즉, 모든 기능이 추가되고 버그가 수정되어 배포 가능한 안정적인 상태라면 develop 브랜치를 'master' 브랜치에 병합(merge)한다.
<br>
평소에는 이 브랜치를 기반으로 개발을 진행한다.
![](/images/types-of-git-branch/develop-branch.png){: width="380" height="550"}



## 3. Feature branch
<span style="color:#4d0000">**기능을 개발하는 브랜치**</span>
<br>
feature 브랜치는 새로운 기능 개발 및 버그 수정이 필요할 때마다 'develop' 브랜치로부터 분기한다.
feature 브랜치에서의 작업은 기본적으로 공유할 필요가 없기 때문에, 자신의 로컬 저장소에서 관리한다.
<br>
개발이 완료되면 'develop' 브랜치로 병합(merge)하여 다른 사람들과 공유한다.

1. 'develop' 브랜치에서 새로운 기능에 대한 feature 브랜치를 분기한다.
2. 새로운 기능에 대한 작업 수행한다.
3. 작업이 끝나면 'develop' 브랜치로 병합(merge)한다.
4. 더 이상 필요하지 않은 feature 브랜치는 삭제한다.
5. 새로운 기능에 대한 'feature' 브랜치를 중앙 원격 저장소에 올린다.(push)

* feature 브랜치 이름 정하기
  * master, develop, release-(RB_), or hotfix- 제외
  * [feature/기능요약] 형식을 추천  EX) feature/login

![](/images/types-of-git-branch/feature-branch.svg){: width="500" height="350"}

* feature 브랜치 생성 및 종료 과정

~~~javascript
// feature 브랜치(feature/login)를 'develop' 브랜치('master' 브랜치에서 따는 것이 아니다!)에서 분기
$ git checkout -b feature/login develop

/* ~ 새로운 기능에 대한 작업 수행 ~ */

/* feature 브랜치에서 모든 작업이 끝나면 */
// 'develop' 브랜치로 이동한다.
$ git checkout develop
// 'develop' 브랜치에 feature/login 브랜치 내용을 병합(merge)한다.
# --no-ff 옵션: 아래에 추가 설명
$ git merge --no-ff feature/login
// -d 옵션: feature/login에 해당하는 브랜치를 삭제한다.
$ git branch -d feature/login
// 'develop' 브랜치를 원격 중앙 저장소에 올린다.
$ git push origin develop
~~~

* <mark>--no-ff 옵션</mark>
  * 새로운 커밋 객체를 만들어 'develop' 브랜치에 merge 한다.
  * 이것은 'feature' 브랜치에 존재하는 커밋 이력을 **모두 합쳐서** 하나의 새로운 커밋 객체를 만들어 'develop' 브랜치로 병합(merge)하는 것이다.
  * ![](/images/types-of-git-branch/feature-branch-merge.png){: width="450" height="400"}


## 4. Release Branch
<span style="color:#4d0000">**이번 출시 버전을 준비하는 브랜치**</span>
<br>
배포를 위한 전용 브랜치를 사용함으로써 한 팀이 해당 배포를 준비하는 동안 다른 팀은 다음 배포를 위한 기능 개발을 계속할 수 있다. 즉, 딱딱 끊어지는 개발 단계를 정의하기에 아주 좋다.
<br>
예를 들어, '이번 주에 버전 1.3 배포를 목표로 한다!'라고 팀 구성원들과 쉽게 소통하고 합의할 수 있다는 말이다.

1. 'develop' 브랜치에서 *배포할 수 있는 수준의 기능이 모이면 또는 정해진 배포 일정이 되면,* release 브랜치를 분기한다.
  * release 브랜치를 만드는 순간부터 배포 사이클이 시작된다.
  * release 브랜치에서는 배포를 위한 최종적인 ***버그 수정, 문서 추가*** 등 릴리스와 직접적으로 관련된 작업을 수행한다.
  * 직접적으로 관련된 작업들을 제외하고는 release 브랜치에 새로운 기능을 추가로 병합(merge)하지 않는다.
2. 'release' 브랜치에서 *배포 가능한 상태가 되면(배포 준비가 완료되면),*
  * 배포 가능한 상태: 새로운 기능을 포함한 상태로 ***모든 기능이 정상적으로 동작*** 하는 상태
    1. 'master' 브랜치에 병합한다. (이때, 병합한 커밋에 Release 버전 태그를 부여!)
    2. 배포를 준비하는 동안 release 브랜치가 변경되었을 수 있으므로 배포 완료 후 'develop' 브랜치에도 병합한다.

이때, 다음 번 배포(Release)를 위한 개발 작업은 'develop' 브랜치에서 계속 진행해 나간다.


* release 브랜치 이름 정하기
  * release-RB_* 또는 release-* 또는 release/* 처럼 이름 짓는 것이 일반적인 관례
  * [release-* ] 형식을 추천  EX) release-1.2

![](/images/types-of-git-branch/release-branch.svg){: width="500" height="350"}

* release 브랜치 생성 및 종료 과정

~~~javascript
// release 브랜치(release-1.2)를 'develop' 브랜치('master' 브랜치에서 따는 것이 아니다!)에서 분기
$ git checkout -b release-1.2 develop

/* ~ 배포 사이클이 시작 ~ */

/* release 브랜치에서 배포 가능한 상태가 되면 */
// 'master' 브랜치로 이동한다.
$ git checkout master
// 'master' 브랜치에 release-1.2 브랜치 내용을 병합(merge)한다.
# --no-ff 옵션: 위의 추가 설명 참고
$ git merge --no-ff release-1.2
// 병합한 커밋에 Release 버전 태그를 부여한다.
$ git tag -a 1.2

/* 'release' 브랜치의 변경 사항을 'develop' 브랜치에도 적용 */
// 'develop' 브랜치로 이동한다.
$ git checkout develop
// 'develop' 브랜치에 release-1.2 브랜치 내용을 병합(merge)한다.
$ git merge --no-ff release-1.2
// -d 옵션: release-1.2에 해당하는 브랜치를 삭제한다.
$ git branch -d release-1.2
~~~


## 5. Hotfix Branch
<span style="color:#4d0000">**출시 버전에서 발생한 버그를 수정 하는 브랜치**</span>
<br>
배포한 버전에 긴급하게 수정을 해야 할 필요가 있을 경우, 'master' 브랜치에서 분기하는 브랜치이다.
'develop' 브랜치에서 문제가 되는 부분을 수정하여 배포 가능한 버전을 만들기에는 시간도 많이 소요되고 안정성을 보장하기도 어려우므로 바로 배포가 가능한 'master' 브랜치에서 직접 브랜치를 만들어 필요한 부분만을 수정한 후 다시 'master'브랜치에 병합하여 이를 배포해야 하는 것이다.
<br>

1. 배포한 버전에 긴급하게 수정을 해야 할 필요가 있을 경우,
  * 'master' 브랜치에서 hotfix 브랜치를 분기한다. (‘hotfix’ 브랜치만 master에서 바로 딸 수 있다.)
2. 문제가 되는 부분만을 빠르게 수정한다.
  * 다시 'master' 브랜치에 병합(merge)하여 이를 안정적으로 다시 배포한다.
  * 새로운 버전 이름으로 태그를 매긴다.
3. hotfix 브랜치에서의 변경 사항은 'develop' 브랜치에도 병합(merge)한다.

![](/images/types-of-git-branch/hotfix-branch.png){: width="420" height="550"}

버그 수정만을 위한 ‘hotfix’ 브랜치를 따로 만들었기 때문에, 다음 배포를 위해 개발하던 작업 내용에 전혀 영향을 주지 않는다. ‘hotfix’ 브랜치는 master 브랜치를 부모로 하는 임시 브랜치라고 생각하면 된다.


* hotfix 브랜치 이름 정하기
  * [hotfix-* ] 형식을 추천  EX) hotfix-1.2.1


* hotfix 브랜치 생성 및 종료 과정

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

/* 'hotfix' 브랜치의 변경 사항을 'develop' 브랜치에도 적용 */
// 'develop' 브랜치로 이동한다.
$ git checkout develop
// 'develop' 브랜치에 hotfix-1.2.1 브랜치 내용을 병합(merge)한다.
$ git merge --no-ff hotfix-1.2.1
~~~



## Branch의 전체 흐름
![](/images/types-of-git-branch/hotfix-branch.svg){: width="500" height="350"}


# 관련된 Post
* 소규모의 프로젝트에서 많이 사용하는 방식(간단한 협업 방법)의 Github 협업 방법을 알고 싶으시면 [Feature Branch Workflow 방법](https://gmlwjd9405.github.io/2017/10/27/how-to-collaborate-on-GitHub-1.html) 을 참고하시기 바랍니다.
* 오픈 소스 프로젝트에서 많이 사용하는 방식의 Github 협업 방법을 알고 싶으시면 [Forking Workflow 방법](https://gmlwjd9405.github.io/2017/10/28/how-to-collaborate-on-GitHub-2.html) 을 참고하시기 바랍니다.
* 대규모의 프로젝트에서 사용하는 엄격한 방식(git branch 종류를 모두 사용) Github 협업 방법을 알고 싶으시면 [Gitflow Workflow 방법](https://gmlwjd9405.github.io/2018/05/12/how-to-collaborate-on-GitHub-3.html) 을 참고하시기 바랍니다.


# References
> - [https://mylko72.gitbooks.io/git/content/branch/branch_type.html](https://mylko72.gitbooks.io/git/content/branch/branch_type.html)
> - [http://nvie.com/posts/a-successful-git-branching-model/](http://nvie.com/posts/a-successful-git-branching-model/)
> - [https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
> - [http://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html](http://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html)
