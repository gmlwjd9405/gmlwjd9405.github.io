---
layout: post
title: '[Error] Travis CI와 AWS s3 연동 시 오류'
subtitle: 'Oops, It looks like you tried to write to a bucket ...'
date: 2018-12-13
author: heejeong Kwon
cover: '/images/error/error-main.png'
tags: travis-ci aws error
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## 오류 내용
`Oops, It looks like you tried to write to a bucket that isn't yours or doesn't exist yet. Please create the bucket before trying to write to it.`

 * ![](/images/error/travisci-awss3-connect-erorr-msg.png)

## 해결 방법
1. AWS s3 버킷 퍼블릭 액세스 설정
* ![](/images/error/travisci-awss3-connect-sol-1.png)

2. 새 퍼블릭 ACL 및 퍼블릭 객체 업로드 차단 - **체크 해제** 후 저장 
* ![](/images/error/travisci-awss3-connect-sol-2.png)


<!-- # 관련된 Post
* []() -->


# References
> - [https://github.com/jojoldu/blog-comments/issues/8](https://github.com/jojoldu/blog-comments/issues/8)