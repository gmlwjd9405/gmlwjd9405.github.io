---
layout: post
title: '[Java] Comparable와 Comparator의 차이와 사용법'
subtitle: 'Comparable, Comparator을 이용하여 Java 객체를 정렬할 수 있다.'
date: 2018-09-06
author: heejeong Kwon
cover: '/images/java-programming/comparable-and-comparator-main.png'
tags: Java comparable comparator
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Comparable을 이용하여 Java 객체를 정렬할 수 있다.
> - Comparator을 이용하여 Java 객체를 정렬할 수 있다.

## 객체 정렬 예시
* 객체를 사용자가 정의한 정렬 기준에 맞춰 정렬해야 하는 경우
  * Ex) 좌표를 x좌표가 증가하는 순, x좌표가 같으면 y좌표가 감소하는 순으로 정렬
  * Ex) 국어점수는 증가하는 순, 수학점수는 감소하는 순으로 정렬

## 객체의 정렬 기준을 명시하는 두 가지 방법
### 1. Comparable interface 구현
* 정의
  * 정렬 수행 시 기본적으로 적용되는 정렬 기준이 되는 메서드를 정의하는 인터페이스
  * package: java.lang.Comparable
    * Java에서 제공되는 정렬이 가능한 클래스들은 모두 Comparable 인터페이스를 구현하고 있으며, 정렬 시에 이에 맞게 정렬이 수행된다.
    * Ex) Integer, Double 클래스: 오름차순 정렬
    * Ex) String 클래스: 사전순 정렬
* 구현 방법
  * 정렬할 객체에 Comparable interface를 implements 후, compareTo() 메서드를 오버라이드하여 구현한다.
  * compareTo() 메서드는 현재 객체가 파라미터로 넘어온 객체보다 작으면 음수 리턴, 같으면 0 리턴, 크면 양수 리턴 하도록 작성한다.
* 사용 방법
  * Collections.sort(list)

**<span style="background-color: #e1e1e1">Comparable interface</span>를 이용한 Java 객체를 정렬**
~~~java
class Point implements Comparable<Point> {
    int x, y;

    @Override
    public int compareTo(Point p) {
        if(this.x > p.x) {
            return 1;
        }
        else if(this.x == p.x) {
            if(this.y > p.y) {
                return 1;
            }
        }
        return -1;
    }
}

// main에서 사용법
List<Point> pointList = new ArrayList<>();
pointList.add(new Point(x, y));
Collections.sort(pointList);
~~~

// 내림차순 기준 정렬.
// 오름차순 기준 정렬.

### 2. Comparator interface 구현
* 정의
  * 정렬 가능한 클래스(omparable 인터페이스를 구현한 클래스)들의 기본 정렬 기준과 다르게 정렬하고 싶을 때 사용하는 인터페이스
  * package: java.util.Comparator
    * 주로 익명 클래스로 사용된다.
    * 기본적인 정렬 방법인 오름차순 정렬을 내림차순으로 정렬할 때 많이 사용한다.
* 구현 방법
  * Comparator interface를 implements 후 compare() 메서드를 오버라이드한 myComparator class 작성한다.
  * compare() 메서드는 첫 번째 파라미터로 넘어온 객체가 두 번째 파라미터로 넘어온 객체보다 작으면 음수 리턴, 같으면 0 리턴, 크면 양수 리턴 하도록 작성한다.
    * `Integer.compare(x, y)`와 동일
    * `return (x < y) ? -1 : ((x == y) ? 0 : 1);`
* 사용 방법
  * Collections.sort(list, myComparator)
  * Collections.sort() 메서드는 두 번째 인자로 Comparator interface를 받을 수 있다.
* 또한 **우선 순위 큐(PriorityQueue)** 생성자의 두 번째 인자로 Comparator interface를 받을 수 있다.
  * PriorityQueue(int initialCapacity, Comparator<? super E> comparator)
  * 지정된 Comparator의 정렬 방법에 따라 우선 순위를 할당한다.

**<span style="background-color: #e1e1e1">Comparator interface</span>를 이용한 Java 객체를 정렬**
~~~java
class MyComparator implements Comparator<Point> {
    @Override
    public int compare(Point p1, Point p2) {
        if (p1.x > p2.x) {
            return 1;
        }
        else if (p1.x == p2.x) {
            if (p1.y > p2.y) {
                return 1;
            }
        }
        return -1;
    }
}

// main에서 사용법
List<Point> pointList = new ArrayList<>();
pointList.add(new Point(x, y));
MyComparator myComparator = new MyComparator();
Collections.sort(pointList, myComparator);
~~~


<!-- # 관련된 Post
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다. -->


# References
> - [http://hochulshin.com/java-comparable-comparator/](http://hochulshin.com/java-comparable-comparator/)
> - [https://m.blog.naver.com/PostView.nhn?blogId=occidere&logNo=220918234464&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F](https://m.blog.naver.com/PostView.nhn?blogId=occidere&logNo=220918234464&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F)
