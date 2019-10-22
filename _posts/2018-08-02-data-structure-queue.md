---
layout: post
title: '[자료구조] 큐(Queue)란'
subtitle: 'java로 큐(Queue)를 구현할 수 있다.'
date: 2018-08-02
author: heejeong Kwon
cover: '/images/data-structure-queue/data-structure-queue-main.png'
tags: data-structure queue
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 큐(Queue)의 기본 연산을 이해한다.
> - java로 큐(Queue)를 구현할 수 있다.


## 큐(Queue)의 개념
컴퓨터의 기본적인 자료 구조의 한가지로, 먼저 집어 넣은 데이터가 먼저 나오는 FIFO(First In First Out)구조로 저장하는 형식


## 큐(Queue)의 연산
큐(Queue)는 **FIFO(First-In-First-Out)** 를 따른다.
* add(item): item을 리스트의 끝부분에 추가한다.
* remove(): 리스트의 첫 번째 항목을 제거한다.
* peek(): 큐에서 가장 위에 있는 항목을 반환한다.
* isEmpty(): 큐가 비어 있을 때에 true를 반환한다.


## 큐(Queue)의 구현
큐(Queue)는 **연결리스트** 로 구현할 수 있다. 연결리스트의 반대 방향에서 항목을 추가하거나 제거하도록 구현한다.
~~~java
public class MyQueue {
  private static class QueueNode {
    private T data;
    private QueueNode next;

    public QueueNode(T data) {
      this.data = data;
    }
  }

  private QueueNode first;
  private QueueNode last;

  public void add(T item) {
    QueueNode t = new QueueNode(item);

    if (last != null) last.next = t;
    last = t;
    if (first == null) first = last;
  }

  public T remove() {
    if (first == null) throw new NoSuchElementException();
    T data = first.data;
    first = first.next;

    if (first == null) last = null;
    return data;
  }

  public T peek() {
    if (first == null) throw new NoSuchElementException();
    return first.data;
  }

  public boolean isEmpty() {
    return first == null;
  }
}
~~~
* <mark>주의!</mark>
  * 큐(Queue)에서 처음과 마지막 노드를 갱신할 때 실수가 나오기 쉽다.


## 큐(Queue)의 사용 사례
데이터가 입력된 시간 순서대로 처리해야 할 필요가 있는 상황에 이용한다.
* 너비 우선 탐색(BFS, Breadth-First Search) 구현
  * 처리해야 할 노드의 리스트를 저장하는 용도로 큐(Queue)를 사용한다.
  * 노드를 하나 처리할 때마다 해당 노드와 인접한 노드들을 큐에 다시 저장한다.
  * 노드를 접근한 순서대로 처리할 수 있다.
* 캐시(Cache) 구현
* 우선순위가 같은 작업 예약 (인쇄 대기열)
* 선입선출이 필요한 대기열 (티켓 카운터)
* 콜센터 고객 대기시간
* 프린터의 출력 처리
* 윈도 시스템의 메시지 처리기
* 프로세스 관리

## java 라이브러리 큐(Queue) 관련 메서드
* 두 가지 형태의 메서드
  1. 수행이 실패했을 때 exception을 발생
    * add(e), element(), remove()
  2. 수행이 실패했을 때 null 또는 false를 반환
    * offer(e), peek(), poll()

1. 수행이 실패했을 때 exception을 발생하는 메서드
* boolean add(E item)
  * 해당 item을 Queue에 삽입
  * 삽입에 성공하면 true를 반환, 삽입할 공간이 없는 경우는 예외(IllegalStateException)를 발생
* E element()
  * Queue의 Head에 있는 item을 삭제하지않고 해당 item을 반환
  * 만약 Queue가 비어있으면 예외를 발생
* E remove()
  * Queue의 Head에 있는 item을 삭제하고 해당 item을 반환
  * 만약 Queue가 비어있으면 예외를 발생

2. 수행이 실패했을 때 null 또는 false를 반환하는 메서드
* boolean offer(E item)
  * add(e)와 동일한 기능
  * 삽입에 성공하면 true를 반환, 실패하면 false를 반환
* E peek()
  * element()와 동일한 기능
  * 만약 Queue가 비어있으면 null을 반환
* E poll()
  * remove()와 동일한 기능
  * 만약 Queue가 비어있으면 null을 반환

* Methods inherited from interface ***java.util.Collection***
  * addAll, clear, contains, containsAll, equals, hashCode, isEmpty, iterator, remove, removeAll, retainAll, size, toArray, toArray


## java 라이브러리 큐(Queue) 사용법
* Implementing Class
  * 제네릭 형태로 사용할 때, 아래와 같이 큐를 구현한 클래스를 생성하여 사용
  * LinkedList
    * 끝에 요소를 추가하기에 용이하다.
    * List interface 구현
    * 요소에 null을 허용한다.
  * priorityQueue
    * 우선순위 큐
    * PIPO(Priority-In Priority-Out)
    * 정렬된 순서에 의해 반복
    * 요소에 null을 허용하지 않는다.           
  * priorityBlockingQueue
    * priorityQueue의 동기화된 버전

* [Queue Implementations 참고](https://docs.oracle.com/javase/tutorial/collections/implementations/queue.html)


# 관련된 Post
* 자료구조 힙(heap)에 대해 알고 싶으시면 [힙(heap)이란](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html) 를 참고하시기 바랍니다.
* 자료구조 스택(Stack)에 대해 알고 싶으시면 [스택(Stack)이란](https://gmlwjd9405.github.io/2018/08/03/data-structure-stack.html)을 참고하시기 바랍니다.
* 자료구조 트리(Tree)에 대해 알고 싶으시면 [트리(Tree)란](https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html)을 참고하시기 바랍니다.
* 자료구조 그래프(Graph)에 대해 알고 싶으시면 [그래프(Graph)란](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)을 참고하시기 바랍니다.

# References
> - [코딩 인터뷰 완전 분석, 프로그래밍인사이트](https://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788966263080)
> - [https://docs.oracle.com/javase/tutorial/collections/implementations/queue.html](https://docs.oracle.com/javase/tutorial/collections/implementations/queue.html)
> - [C언어로 쉽게 풀어 쓴 자료구조](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&barcode=9788970506432)