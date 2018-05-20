---
layout: post
title: '[Github] Github 저장소의 Description・Readme에 아이콘 넣기'
subtitle: 'Set image icon in Github Repository Description or Readme File'
date: 2018-05-16
author: heejeong Kwon
cover: '/images/github-repository-description-icon/github-repository-description-icon-main.png'
tags: github emoji
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 자신의 Github 저장소 설명(Repository's description)에 아이콘을 넣을 수 있다.
> - 자신의 Github 저장소의 Readme.md(markdown file)에 아이콘을 넣을 수 있다.
> - 다양한 아이콘 종류를 확인할 수 있다.

## 그림 아이콘(emoji)의 사용 방법
아래의 "그림 아이콘 전체 목록" 사이트에서 아이콘을 확인하여 해당하는 내용을 적어주기만 하면 된다.

* Github 저장소 설명(Repository's description)
![](/images/github-repository-description-icon/desctiption-before.png)
![](/images/github-repository-description-icon/desctiption-after.png)

* Github 저장소의 Readme.md(markdown file)
![](/images/github-repository-description-icon/readme-before.png)
![](/images/github-repository-description-icon/readme-total.png)


## 그림 아이콘(emoji)의 종류
* github markdown 그림 아이콘 전체 목록
  * [https://gist.github.com/rxaviers/7360908](https://gist.github.com/rxaviers/7360908)
  * [https://gist.github.com/AliMD/3344523](https://gist.github.com/AliMD/3344523)


## 그림 아이콘(emoji)의 사용
* 그림 아이콘은 github에서 markdown 형식으로 작성하는 곳에서는 모두 사용할 수 있다.

### 1. Github 저장소 설명(Repository's description)
* ![](/images/github-repository-description-icon/desctiption-total2.png)

### 2. Github 저장소의 Readme.md(markdown file)
*
~~~javascript
:bulb:  :blush:  :seedling:  :bell:  :balloon:  :octocat:
~~~
* ![](/images/github-repository-description-icon/readme-total.png)

### 3. 자신의 profile 설명
* ![](/images/github-repository-description-icon/profile-total.png)

### 4. Commit Message 관리
* 아이콘마다 자신만의 구분법을 설정하여 Commit Message를 관리할 수 있다.
*
~~~javascript
git commit -m ":seedling: Write about setting github markdown icon"
~~~
* ![](/images/github-repository-description-icon/commit-message-after.png)
* EX) [https://gist.github.com/parmentf/035de27d6ed1dce0b36a](https://gist.github.com/parmentf/035de27d6ed1dce0b36a)


# References
> - [https://stackoverflow.com/questions/37241480/how-to-add-image-icon-on-the-left-of-github-repository-description](https://stackoverflow.com/questions/37241480/how-to-add-image-icon-on-the-left-of-github-repository-description)
> - [https://gist.github.com/rxaviers/7360908](https://gist.github.com/rxaviers/7360908)
> - [https://gist.github.com/AliMD/3344523](https://gist.github.com/AliMD/3344523)
> - [https://gist.github.com/parmentf/035de27d6ed1dce0b36a](https://gist.github.com/parmentf/035de27d6ed1dce0b36a)
