---
layout: post
title: '[자료구조] 스택(Stack)이란'
subtitle: 'java로 스택(Stack)을 구현할 수 있다.'
date: 2018-08-03
author: heejeong Kwon
cover: '/images/data-structure-stack/data-structure-stack-main.png'
tags: data-structure stack
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 스택(Stack)의 기본 연산을 이해한다.
> - java로 스택(Stack)을 구현할 수 있다.


## 스택(Stack)의 개념
한 쪽 끝에서만 자료를 넣고 뺄 수 있는 LIFO(Last In First Out) 형식의 자료 구조


## 스택(Stack)의 연산
스택(Stack)는 **LIFO(Last In First Out)** 를 따른다. 즉, 가장 최근에 스택에 추가한 항목이 가장 먼저 제거될 항목이다.
* pop(): 스택에서 가장 위에 있는 항목을 제거한다.
* push(item): item 하나를 스택의 가장 윗 부분에 추가한다.
* peek(): 스택의 가장 위에 있는 항목을 반환한다.
* isEmpty(): 스택이 비어 있을 때에 true를 반환한다.


## 스택(Stack)의 구현
문제의 종류에 따라 배열보다 스택에 데이터를 저장하는 것이 더 적합한 방법일 수 있다.
* 배열과 달리 스택은 상수 시간에 i번째 항목에 접근할 수 없다.
* 하지만 스택에서 데이터를 추가하거나 삭제하는 연산은 상수 시간에 가능하다.
* 배열처럼 원소들을 하나씩 옆으로 밀어 줄 필요가 없다.

스택(Stack)은 **연결리스트** 로 구현할 수 있다. 연결리스트의 같은 방향에서 아이템을 추가하고 삭제하도록 구현한다.
~~~java
public class MyStack {
  private static class StackNode {
    private T data;
    private StackNode next;

    public StackNode(T data) {
      this.data = data;
    }
  }

  private StackNode top;

  public T pop() {
    if (top == null) throw new NoSuchElementException();
    T item = top.data;
    top = top.next;

    return item;
  }

  public void push(T item) {
    StackNode t = new StackNode(item);
    t.next = top;
    top = t;
  }

  public T peek() {
    if (top == null) throw new NoSuchElementException();
    return top.data;
  }

  public boolean isEmpty() {
    return top == null;
  }
}
~~~

## 스택(Stack)의 사용 사례
재귀 알고리즘을 사용하는 경우 스택이 유용하다.
* 재귀 알고리즘
  * 재귀적으로 함수를 호출해야 하는 경우에 임시 데이터를 스택에 넣어준다.
  * 재귀함수를 빠져 나와 퇴각 검색(backtrack)을 할 때는 스택에 넣어 두었던 임시 데이터를 빼 줘야 한다.
  * 스택은 이런 일련의 행위를 직관적으로 가능하게 해 준다.
  * 또한 스택은 재귀 알고리즘을 반복적 형태(iterative)를 통해서 구현할 수 있게 해준다.
* 웹 브라우저 방문기록 (뒤로가기)
* 실행 취소 (undo)
* 역순 문자열 만들기
* 수식의 괄호 검사 (연산자 우선순위 표현을 위한 괄호 검사)
  * Ex) 올바른 괄호 문자열(VPS, Valid Parenthesis String) 판단하기
* 후위 표기법 계산


## java 라이브러리 스택(Stack) 관련 메서드
* push(E item)
    * 해당 item을 Stack의 top에 삽입
    * Vector의 addElement(item)과 동일
* pop()
    * Stack의 top에 있는 item을 삭제하고 해당 item을 반환
* peek()
    * Stack의 top에 있는 item을 삭제하지않고 해당 item을 반환
* empty()
    * Stack이 비어있으면 true를 반환 그렇지않으면 false를 반환
* search(Object o)
    * 해당 Object의 위치를 반환
    * Stack의 top 위치는 1, 해당 Object가 없으면 -1을 반환

<br>
* Methods inherited from class ***java.util.Vector***
    * add, add, addAll, addAll, addElement, capacity, clear, clone, contains, containsAll, copyInto, elementAt, elements, ensureCapacity, equals, firstElement, get, hashCode, indexOf, indexOf, insertElementAt, isEmpty, iterator, lastElement, lastIndexOf, lastIndexOf, listIterator, listIterator, remove, remove, removeAll, removeAllElements, removeElement, removeElementAt, removeRange, retainAll, set, setElementAt, setSize, size, subList, toArray, toArray, toString, trimToSize

* Methods inherited from class ***java.lang.Object***
  * finalize, getClass, notify, notifyAll, wait, wait, wait


# 관련된 Post
* 자료구조 힙(heap)에 대해 알고 싶으시면 [힙(heap)이란](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html) 를 참고하시기 바랍니다.
* 자료구조 큐(Queue)에 대해 알고 싶으시면 [큐(Queue)란](https://gmlwjd9405.github.io/2018/08/02/data-structure-queue.html)을 참고하시기 바랍니다.
* 자료구조 트리(Tree)에 대해 알고 싶으시면 [트리(Tree)란](https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html)을 참고하시기 바랍니다.
* 자료구조 그래프(Graph)에 대해 알고 싶으시면 [그래프(Graph)란](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)을 참고하시기 바랍니다.

# References
> - [코딩 인터뷰 완전 분석, 프로그래밍인사이트](https://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788966263080)
> - [https://docs.oracle.com/javase/7/docs/api/java/util/Stack.html](https://docs.oracle.com/javase/7/docs/api/java/util/Stack.html)
> - [C언어로 쉽게 풀어 쓴 자료구조](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&barcode=9788970506432)