---
layout: post
title: '[자료구조] 트리(Tree)란'
subtitle: '트리(Tree)의 개념과 특징을 이해할 수 있다.'
date: 2018-08-12
author: heejeong Kwon
cover: '/images/data-structure-tree/data-structure-tree-main.png'
tags: data-structure tree
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 트리(Tree)의 기본 개념을 이해한다.
> - 트리(Tree)의 종류를 구분할 수 있다.
> - 트리(Tree)의 특징을 이해할 수 있다.

## 트리(Tree)의 개념
트리는 **노드로 이루어진 자료 구조**
1. 트리는 하나의 루트 노드를 갖는다.
2. 루트 노드는 0개 이상의 자식 노드를 갖고 있다.
3. 그 자식 노드 또한 0개 이상의 자식 노드를 갖고 있고, 이는 반복적으로 정의된다.

* 노드(node)들과 노드들을 연결하는 간선(edge)들로 구성되어 있다.
  * 트리에는 사이클(cycle)이 존재할 수 없다.
  * 노드들은 특정 순서로 나열될 수도 있고 그럴 수 없을 수도 있다.
  * 각 노드는 부모 노드로의 연결이 있을 수도 있고 없을 수도 있다.
  * 각 노드는 어떤 자료형으로도 표현 가능하다.
~~~java
class Node {
      public String name;
      public Node[] children;
}
~~~
* 비선형 자료구조로 계층적 관계를 표현한다. Ex) 디렉터리 구조, 조직도
* 그래프의 한 종류
  * **사이클(cycle)이 없는 하나의 연결 그래프(Connected Graph)**
  * 또는 **DAG(Directed Acyclic Graph, 방향성이 있는 비순환 그래프)의 한 종류** 이다.


## 트리(Tree)와 관련된 용어
![](/images/data-structure-tree/tree-terms.png)
* 루트 노드(root node): 부모가 없는 노드, 트리는 하나의 루트 노드만을 가진다.
* 단말 노드(leaf node): 자식이 없는 노드, '말단 노드' 또는 '잎 노드'라고도 부른다.
* 내부(internal) 노드: 단말 노드가 아닌 노드
* 간선(edge): 노드를 연결하는 선 (link, branch 라고도 부름)
* 형제(sibling): 같은 부모를 가지는 노드
* 노드의 크기(size): 자신을 포함한 모든 자손 노드의 개수
* 노드의 깊이(depth): 루트에서 어떤 노드에 도달하기 위해 거쳐야 하는 간선의 수
* 노드의 레벨(level): 트리의 특정 깊이를 가지는 노드의 집합
* 노드의 차수(degree): 하위 트리 개수 / 간선 수 (degree) = 각 노드가 지닌 가지의 수
* 트리의 차수(degree of tree): 트리의 최대 차수
* 트리의 높이(height): 루트 노드에서 가장 깊숙히 있는 노드의 깊이


## 트리(Tree)의 특징
* 그래프의 한 종류이다. **'최소 연결 트리'** 라고도 불린다.
* 트리는 **계층 모델** 이다.
* 트리는 DAG(Directed Acyclic Graphs, 방향성이 있는 비순환 그래프)의 한 종류이다.
  * loop나 circuit이 없다. 당연히 self-loop도 없다.
  * 즉, 사이클이 없다.
* 노드가 N개인 트리는 항상 N-1개의 간선(edge)을 가진다.
  * 즉, 간선은 항상 (정점의 개수 - 1) 만큼을 가진다.
* 루트에서 어떤 노드로 가는 경로는 유일하다.
  * 임의의 두 노드 간의 경로도 유일하다. 즉, 두 개의 정점 사이에 반드시 1개의 경로만을 가진다.
* 한 개의 루트 노드만이 존재하며 모든 자식 노드는 한 개의 부모 노드만을 가진다.
  * 부모-자식 관계이므로 흐름은 top-bottom 아니면 bottom-top으로 이루어진다.
* 순회는 Pre-order, In-order 아니면 Post-order로 이루어진다. 이 3가지 모두 DFS/BFS 안에 있다.
* 트리는 이진 트리, 이진 탐색 트리, 균형 트리(AVL 트리, red-black 트리), 이진 힙(최대힙, 최소힙) 등이 있다.


## 트리(Tree)의 종류
### 트리 VS 이진 트리
* 이진 트리(Binary Tree)
  * 각 노드가 최대 두 개의 자식을 갖는 트리
  * 모든 트리가 이진 트리는 아니다.
  * 이진 트리 순회
  1. 중위 순회(in-order traversal): 왼쪽 가지 -> 현재 노드 -> 오른쪽 가지
~~~java
void inOrderTraversal(TreeNode node) {
      if(node != null) {
          inOrderTraversal(node.left);
          visit(node);
          inOrderTraversal(node.right);
      }
}
~~~
  2. 전위 순회(pre-order traversal): 현재 노드 -> 왼쪽 가지 -> 오른쪽 가지
~~~java
void preOrderTraversal(TreeNode node) {
      if(node != null) {
          visit(node);
          preOrderTraversal(node.left);
          preOrderTraversal(node.right);
      }
}
~~~
  3. 후위 순회(post-order traversal): 왼쪽 가지 -> 오른쪽 가지 -> 현재 노드
~~~java
void postOrderTraversal(TreeNode node) {
      if(node != null) {
          postOrderTraversal(node.left);
          postOrderTraversal(node.right);
          visit(node);
      }
}
~~~

### 이진 트리 VS 이진 탐색 트리
* 이진 탐색 트리(Binary Search Tree)
  * 모든 노드가 아래와 같은 특정 순서를 따르는 속성이 있는 이진 트리
  * 모든 왼쪽 자식들 <= n < 모든 오른쪽 자식들 (모든 노드 n에 대해서 반드시 참)

### 균형 트리 VS 비균형 트리
* 균형 트리
  * O(logN) 시간에 insert와 find를 할 수 있을 정도로 균형이 잘 잡혀 있는 경우
  * Ex) 레드-블랙 트리, AVL 트리

### 완전 이진 트리 VS 전 이진 트리 VS 포화 이진 트리
![](/images/data-structure-tree/tree-types-example.png)
* 완전 이진 트리(Complete Binary Tree)
  * ![](/images/data-structure-tree/Complete-Binary-Tree.png)
  * 트리의 모든 높이에서 노드가 꽉 차 있는 이진 트리. 즉, 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져 있다.
  * 마지막 레벨은 꽉 차 있지 않아도 되자만 노드가 왼쪽에서 오른쪽으로 채워져야 한다.
  * 마지막 레벨 h에서 (1 ~ 2h-1)개의 노드를 가질 수 있다.
  * 또 다른 정의는 가장 오른쪽의 잎 노드가 (아마도 모두) 제거된 포화 이진 트리다.
  * 완전 이진 트리는 배열을 사용해 효율적으로 표현 가능하다.
* 전 이진 트리(Full Binary Tree 또는 Strictly Binary Tree)
  * ![](/images/data-structure-tree/Full-Binary-Tree.png)
  * 모든 노드가 0개 또는 2개의 자식 노드를 갖는 트리.
* 포화 이진 트리(Perfect Binary Tree)
  * ![](/images/data-structure-tree/Perfect-Binary-Tree.png)
  * 전 이진 트리이면서 완전 이진 트리인 경우
  * 모든 말단 노드는 같은 높이에 있어야 하며, 마지막 단계에서 노드의 개수가 최대가 되어야 한다.
  * 모든 내부 노드가 두 개의 자식 노드를 가진다.
  * 모든 말단 노드가 동일한 깊이 또는 레벨을 갖는다.
  * 노드의 개수가 정확히 2^(k-1)개여야 한다. (k: 트리의 높이) -> 흔하게 나타나는 경우가 아니다. 즉, 이진 트리가 모두 포화 이진 트리일 것이라고 생각하지 않는다.

### 이진 힙(최소힙과 최대힙)
* 최소힙(Min Heap)
  * 트리의 마지막 단계에서 오른쪽 부분을 뺀 나머지 부분이 가득 채워져 있는 완전 이진 트리이며, 각 노드의 원소가 자식들의 원소보다 작다.
    * 즉, key(부모 노드) >= key(자식 노드)인 완전 이진 트리
    * 가장 큰 값은 루트 노드이다.
    * N개가 힙에 들어가 있으면 높이는 log(N)이다.
* 최대힙(Max Heap)
  * 원소가 내림차순으로 정렬되어 있다는 점에서만 최소힙과 다르다.
  * 각 노드의 원소가 자식들의 원소보다 크다.
* [힙(heap)이란](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html) 참고

### 트라이(trie) ('접두사 트리(Prefix Tree)'라고도 부른다.)
* 트라이
  * n-차 트리(n-ary Tree)의 변종
  * 각 노드에 문자를 저장하는 자료구조
  * 따라서 트리를 아래쪽으로 순회하면 단어 하나가 나온다.
  * **접두사를 빠르게 찾아보기 위한 흔한 방식** 으로, 모든 언어를 트라이에 저장해 놓는 방식이 있다.
  * **유효한 단어 집합을 이용** 하는 많은 문제들은 트라이를 통해 최적화할 수 있다.
  <!-- * [트라이(trie)이란]() 참고 -->


## 트리(Tree)의 구현 방법
기본적으로 트리는 그래프의 한 종류이므로 그래프의 구현 방법(인접 리스트 또는 인접 배열)으로 구현할 수 있다.<br>

**인접 배열 이용**
1. 1차원 배열에 자신의 부모 노드만 저장하는 방법
  * 트리는 부모 노드를 0개 또는 1개를 가지기 때문
  * 부모 노드를 0개: 루트 노드
2. 이진 트리의 경우, 2차원 배열에 자식 노드를 저장하는 방법
  * 이진 트리는 각 노드가 최대 두 개의 자식을 갖는 트리이기 때문
  * Ex) A[i][0]: 왼쪽 자식 노드, A[i][1]: 오른쪽 자식 노드

**인접 리스트 이용**
1. 가중치가 없는 트리의 경우
  * ArrayList< ArrayList<Integer> > list = new ArrayList<>();
2. 가중치가 있는 트리의 경우
  * 1) class Node { int num, dist; // 노드 번호, 거리 } 정의
  * 2) ArrayList<Node>[] list = new ArrayList[정점의 수 + 1];



<mark>그래프와 트리의 차이</mark>
![](/images/data-structure-graph/graph-vs-tree.png)

# 관련된 Post
* 자료구조 힙(heap)에 대해 알고 싶으시면 [힙(heap)이란](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html) 를 참고하시기 바랍니다.
* 자료구조 스택(Stack)에 대해 알고 싶으시면 [스택(Stack)이란](https://gmlwjd9405.github.io/2018/08/03/data-structure-stack.html)을 참고하시기 바랍니다.
* 자료구조 큐(Queue)에 대해 알고 싶으시면 [큐(Queue)란](https://gmlwjd9405.github.io/2018/08/02/data-structure-queue.html)을 참고하시기 바랍니다.
* 자료구조 그래프(Graph)에 대해 알고 싶으시면 [그래프(Graph)란](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)을 참고하시기 바랍니다.

# References
> - [코딩 인터뷰 완전 분석, 프로그래밍인사이트](https://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788966263080)
