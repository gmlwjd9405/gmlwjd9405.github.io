---
layout: post
title: '[자료구조] 힙(heap)이란'
subtitle: '우선순위 큐를 위하여 만들어진 자료구조'
date: 2018-05-10
author: heejeong Kwon
cover: '/images/data-structure-heap/data-structure-heap-main.png'
tags: data-structure 힙 heap
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 우선 순위 큐를 위하여 만들어진 자료구조, 힙(heap)에 대해 이해한다.
> - 배열을 이용하여 힙(heap)을 구현할 수 있다.
> - 힙(heap)의 삽입과 삭제를 이해한다.


<br>
<br>
**[들어가기 전]**
* 우선순위 큐: 우선순위의 개념을 큐에 도입한 자료 구조
  * 데이터들이 우선순위를 가지고 있고 우선순위가 높은 데이터가 먼저 나간다.
  * ![](/images/data-structure-heap/data-structure-heap-compare.png){: width="400" height="110"}
  * 우선순위 큐의 이용 사례
    1. 시뮬레이션 시스템
    2. 네트워크 트래픽 제어
    3. 운영 체제에서의 작업 스케쥴링
    4. 수치 해석적인 계산
  * 우선순위 큐는 배열, 연결리스트, **힙** 으로 구현이 가능하다. 이 중에서 힙(heap)으로 구현하는 것이 가장 효율적이다.
    * ![](/images/data-structure-heap/data-structure-heap-priorityqueue.png){: width="380" height="160"}


## 자료구조 '힙(heap)'이란?
* **완전 이진 트리의 일종으로** 우선순위 큐를 위하여 만들어진 자료구조이다.
* 여러 개의 값들 중에서 최댓값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조이다.
* 힙은 일종의 **반정렬 상태(느슨한 정렬 상태)** 를 유지한다.
  * 큰 값이 상위 레벨에 있고 작은 값이 하위 레벨에 있다는 정도
  * 간단히 말하면 부모 노드의 키 값이 자식 노드의 키 값보다 항상 큰(작은) 이진 트리를 말한다.
* 힙 트리에서는 중복된 값을 허용한다. (이진 탐색 트리에서는 중복된 값을 허용하지 않는다.)



## 힙(heap)의 종류
* 최대 힙(max heap)
  * 부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진 트리
  * key(부모 노드) >= key(자식 노드)
* 최소 힙(min heap)
  * 부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은 완전 이진 트리
  * key(부모 노드) <= key(자식 노드)
* ![](/images/data-structure-heap/types-of-heap.png)


## 힙(heap)의 구현
* 힙을 저장하는 표준적인 자료구조는 **배열** 이다.
* 구현을 쉽게 하기 위하여 배열의 첫 번째 인덱스인 0은 사용되지 않는다.
* 특정 위치의 노드 번호는 새로운 노드가 추가되어도 변하지 않는다.
  * 예를 들어 루트 노드의 오른쪽 노드의 번호는 항상 3이다.
* 힙에서의 부모 노드와 자식 노드의 관계
  * 왼쪽 자식의 인덱스 = (부모의 인덱스) * 2
  * 오른쪽 자식의 인덱스 = (부모의 인덱스) * 2 + 1
  * 부모의 인덱스 = (자식의 인덱스) / 2
  * ![](/images/data-structure-heap/heap-index-parent-child.png)

<br>
* c언어를 이용한 힙(heap)의 구현

~~~ javascript
#define MAX_ELEMENT 200 // 힙 안에 저장된 요소의 개수

typedef struct{
  int key;
} element;

typedef struct{
  element heap[MAX_ELEMENT];
  int heap_size;
} HeapType;

# 힙의 생성
HeapType heap1;

~~~


## 힙(heap)의 삽입
1. 힙에 새로운 요소가 들어오면, 일단 새로운 노드를 힙의 마지막 노드에 이어서 삽입한다.
2. 새로운 노드를 부모 노드들과 교환해서 힙의 성질을 만족시킨다.

* 아래의 최대 힙(max heap)에 새로운 요소 8을 삽입해보자.
<br>
<br>
![](/images/data-structure-heap/maxheap-insertion.png)

<br>
* c언어를 이용한 최대 힙(max heap) 삽입 연산

~~~ javascript
/* 현재 요소의 개수가 heap_size인 힙 h에 item을 삽입한다. */
// 최대 힙(max heap) 삽입 함수
void insert_max_heap(HeapType *h, element item){
  int i;
  i = ++(h->heap_size); // 힙 크기를 하나 증가

  /* 트리를 거슬러 올라가면서 부모 노드와 비교하는 과정 */
  // i가 루트 노트(index: 1)이 아니고, 삽입할 item의 값이 i의 부모 노드(index: i/2)보다 크면
  while((i != 1) && (item.key > h->heap[i/2].key)){
    // i번째 노드와 부모 노드를 교환환다.
    h->heap[i] = h->heap[i/2];
    // 한 레벨 위로 올라단다.
    i /= 2;
  }
  h->heap[i] = item; // 새로운 노드를 삽입
}
~~~

* java언어를 이용한 최대 힙(max heap) 삽입 연산
~~~java
/* 최대힙 삽입 */
void insert_max_heap(int x){
  maxHeap[++heapSize] = x; // 힙 크기를 하나 증가하고 마지막 노드에 x를 넣는다.

  for (int i=heapSize; i>1; i/=2) {
    // 마지막 노드가 자신의 부모 노드보다 크면 swap
    if (maxHeap[i/2] < maxHeap[i]) {
      swap(i/2, i);
    } else {
      break;
    }
  }
}
~~~


## 힙(heap)의 삭제
1. 최대 힙에서 최댓값은 루트 노드이므로 루트 노드가 삭제된다.
  * 최대 힙(max heap)에서 삭제 연산은 최댓값을 가진 요소를 삭제하는 것이다.
2. 삭제된 루트 노드에는 힙의 마지막 노드를 가져온다.
3. 힙을 재구성한다.

* 아래의 최대 힙(max heap)에서 최댓값을 삭제해보자.
<br>
<br>
![](/images/data-structure-heap/maxheap-delete.png)

<br>
* c언어를 이용한 최대 힙(max heap) 삭제 연산

~~~ javascript
// 최대 힙(max heap) 삭제 함수
element delete_max_heap(HeapType *h){
  int parent, child;
  element item, temp;

  item = h->heap[1]; // 루트 노드 값을 반환하기 위해 item에 할당
  temp = h->heap[(h->heap_size)--]; // 마지막 노드를 temp에 할당하고 힙 크기를 하나 감소
  parent = 1;
  child = 2;

  while(child <= h->heap_size){
    // 현재 노드의 자식 노드 중 더 큰 자식 노드를 찾는다. (루트 노드의 왼쪽 자식 노드(index: 2)부터 비교 시작)
    if( (child < h->heap_size) && ((h->heap[child].key) < h->heap[child+1].key) ){
      child++;
    }
    // 더 큰 자식 노드보다 마지막 노드가 크면, while문 중지
    if( temp.key >= h->heap[child].key ){
      break;
    }

    // 더 큰 자식 노드보다 마지막 노드가 작으면, 부모 노드와 더 큰 자식 노드를 교환
    h->heap[parent] = h->heap[child];
    // 한 단계 아래로 이동
    parent = child;
    child *= 2;
  }

  // 마지막 노드를 재구성한 위치에 삽입
  h->heap[parent] = temp;
  // 최댓값(루트 노드 값)을 반환
  return item;
}
~~~

* java언어를 이용한 최대 힙(max heap) 삭제 연산
~~~java
/* 최대힙 삭제 */
int delete_max_heap(){
  if (heapSize == 0) // 배열이 빈 경우
    return 0;

  int item = maxHeap[1]; // 루트 노드의 값을 저장한다.
  maxHeap[1] = maxHeap[heapSize]; // 마지막 노드의 값을 루트 노드에 둔다.
  maxHeap[heapSize--] = 0; // 힙 크기를 하나 줄이고 마지막 노드를 0으로 초기화한다.

  for (int i=1; i*2<=heapSize;) {
    // 마지막 노드가 왼쪽 노드와 오른쪽 노드보다 크면 반복문을 나간다.
    if (maxHeap[i] > maxHeap[i*2] && maxHeap[i] > maxHeap[i*2+1]) {
      break;
    }
    // 왼쪽 노드가 더 큰 경우, 왼쪽 노드와 마지막 노드를 swap
    else if (maxHeap[i*2] > maxHeap[i*2+1]) {
      swap(i, i*2);
      i = i*2;
    }
    // 오른쪽 노드가 더 큰 경우, 오른쪽 노드와 마지막 노드를 swap
    else {
      swap(i, i*2+1);
      i = i*2+1;
    }
  }
  return item;
}
~~~

### 참고
[Java] PriorityQueue, Comparator interface를 이용한 방법: [https://gist.github.com/Baekjoon/5b1349b9159e7548bb54]()


# 관련된 Post
* 힙 정렬(heap sort): [힙 정렬(heap sort)](https://gmlwjd9405.github.io/2018/05/10/algorithm-heap-sort.html) 을 참고하시기 바랍니다.
* 자료구조 스택(Stack)에 대해 알고 싶으시면 [스택(Stack)이란](https://gmlwjd9405.github.io/2018/08/03/data-structure-stack.html)을 참고하시기 바랍니다.
* 자료구조 큐(Queue)에 대해 알고 싶으시면 [큐(Queue)란](https://gmlwjd9405.github.io/2018/08/02/data-structure-queue.html)을 참고하시기 바랍니다.
* 자료구조 트리(Tree)에 대해 알고 싶으시면 [트리(Tree)란](https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html)을 참고하시기 바랍니다.
* 자료구조 그래프(Graph)에 대해 알고 싶으시면 [그래프(Graph)란](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)을 참고하시기 바랍니다.


# References
> - [힙(자료구조) - 위키백과](https://ko.wikipedia.org/wiki/%ED%9E%99_(%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0))
> - [C언어로 쉽게 풀어 쓴 자료구조](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&barcode=9788970506432)
