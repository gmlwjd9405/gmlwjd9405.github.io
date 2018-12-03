---
layout: post
title: '[IntelliJ] IntelliJ에서 Lombok 설정하기' 
subtitle: 'IntelliJ에서 Lombok을 설정하여 사용할 수 있다.'
date: 2018-11-29 
author: heejeong Kwon
cover: '/images/lombok/lombok-setting-main.png'
tags: intellij lombok setting
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - IntelliJ에서 Lombok Dependency 설정하기 
> - IntelliJ에서 Lombok Plugin 설정하기
> - IntelliJ에서 Enable annotation 설정하기 

## 1. Lombok Dependency 설정
* [https://mvnrepository.com/artifact/org.projectlombok/lombok](https://mvnrepository.com/artifact/org.projectlombok/lombok)  참고

### Maven
```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.4</version>
    <scope>provided</scope>
</dependency>
```

### Gradle
```xml
// https://mvnrepository.com/artifact/org.projectlombok/lombok
provided group: 'org.projectlombok', name: 'lombok', version: '1.18.4'
```

## 2. Lombok Plugin 설정
* 최초로만 설정하면 된다.

1. 설정
    * **Windows**: File > Setting (Ctrl+Alt+S)
    * **MacOS**: Preferences (Cmd + ,)
2. Plugins 선택 후 Browse repositorie에서 lombok 검색
    * ![](/images/lombok/plugin1.png)
3. Lombok Plugin (TOOLS INTEGRATION) Install
    * ![](/images/lombok/plugin2.png)
4. IntelliJ Restart
    * ![](/images/lombok/plugin3.png)
   
## 3. Enable annotation 설정 
1. 설정
    * **Windows**: File > Setting (Ctrl+Alt+S)
    * **MacOS**: Preferences (Cmd + ,)
2. Build, Execution, Deployment > Compiler > Annotation Processings 
    * ![](/images/lombok/enable1.png)
3. Enable annotation processing 체크
    * ![](/images/lombok/enable2.png)


# References
> - [https://mvnrepository.com/artifact/org.projectlombok/lombok](https://mvnrepository.com/artifact/org.projectlombok/lombok)
> - [http://ilovejinwon.tistory.com/49](http://ilovejinwon.tistory.com/49)
