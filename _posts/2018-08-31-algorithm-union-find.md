---
layout: post
title: '[알고리즘] Union-Find 알고리즘'
subtitle: 'Union-Find 알고리즘을 이해하고 구현할 수 있다.'
date: 2018-08-31
author: heejeong Kwon
cover: '/images/algorithm-union-find/union-find-main.png'
tags: algorithm union-find disjoint-set
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Disjoint Set과 Union-Find의 개념을 이해할 수 있다.
> - Union-Find 알고리즘을 구현할 수 있다.


## Disjoint Set과 Union-Find의 개념
### Disjoint Set이란
서로 중복되지 않는 부분 집합들로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조
* 즉, 공통 원소가 없는, 즉 "상호 배타적" 인 부분 집합들로 나눠진 원소들에 대한 자료구조이다.
* Disjoint-set = 서로소 집합 자료구조
* 예를 들어, S = {1, 2, 3, 4}, A = {1, 2}, B = {3, 4}, C = {2, 3, 4}, D = {4}라면
  * A와 B는 S의 분할 O. A와 B는 Disjoint Set
  * A와 C는 S의 분할 X. 겹치는 원소가 존재
  * A와 D는 S의 분할 X. 두 집합을 합해도 S가 되지 않음

### Union-Find란
Disjoint Set을 표현할 때 사용하는 알고리즘
* 집합을 구현하는 데는 비트 벡터, 배열, 연결 리스트를 이용할 수 있으나 그 중 가장 효율적인 **트리 구조** 를 이용하여 구현한다.

<!-- 1. 배열
  * 시간 복잡도
2. 트리 -->

## Union-Find 연산과 사용 예시
// 백준 자료 보고 그리기
<!-- ![](/images/algorithm-union-find/union-find-example.png) -->

### 연산 정의
* make-set(x)
  * 초기화
  * x를 유일한 원소로 하는 새로운 집합을 만든다.
* union(x, y)
  * 합하기
  * x가 속한 집합과 y가 속한 집합을 합친다. 즉, x와 y가 속한 두 집합을 합치는 연산
* find(x)
  * 찾기
  * x가 속한 집합의 대표값(루트노드 값)을 반환한다. 즉, x가 어떤 집합에 속해 있는지 찾는 연산

### 사용 예시
전체 집합이 있을 때 구성 원소들이 겹치지 않도록 분할(partition)하는 데 자주 사용된다.
1. 초기에 {0}, {1}, {2}, ... {n} 이 각각 n+1개의 집합을 이루고 있다. 여기에 합집합 연산과, 두 원소가 같은 집합에 포함되어 있는지를 확인하는 연산을 수행하려는 경우
  * [집합의 표현-백준1717번](https://www.acmicpc.net/problem/1717)
2. 어떤 사이트의 친구 관계가 생긴 순서대로 주어졌을 때, 가입한 두 사람의 친구 네트워크에 몇 명이 있는지 구하는 프로그램을 작성하는 경우
  * [친구 네트워크-백준4195번](https://www.acmicpc.net/problem/4195)
3. Kruskal MST 알고리즘에서 새로 추가할 간선의 양끝 정점이 같은 집합에 속해 있는지(사이클이 형성되는지)에 대해 검사하는 경우



## Union-Find의 기본적인 구현 방법
~~~java
/* 초기화 */
int root[MAX_SIZE];
for (int i = 0; i < MAX_SIZE; i++)
    parent[i] = i;

/* find(x) */
int find(int x) {
    // 루트 노드는 부모 노드 번호로 자기 자신을 가진다.
    if (root[x] == x) {
        return x;
    } else {
        // 각 노드의 부모 노드를 찾아 올라간다.
        return find(root[x]);
    }
}

/* union(x, y) */
void union(int x, int y){
    // 각 원소가 속한 트리의 루트 노드를 찾는다.
    x = find(x);
    y = find(y);

    root[y] = x;
}
~~~


## Union-Find의 최적화한 구현 방법
~~~java
/* 초기화 */
int root[MAX_SIZE];
for (int i = 0; i < MAX_SIZE; i++)
    root[i] = i;

/* find(x) */
int find(int x) {
    if (root[x] == x) {
        return x;
    } else {
        // 경로 압축
        // find 하면서 만난 모든 값의 부모 노드를 root로 만든다.
        root[x] = find(root[x]);
    }
}

/* union(x, y) */
 void union(int x, int y){
     x = find(x);
     y = find(y);

     // 두 값의 root가 같으면(이미 같은 트리) 합치지 않는다.
     if(x == y)
         return;

     root[y] = x; // y의 root를 x로 변경
 }

 /* union2(x, y): 두 원소가 속한 트리의 전체 노드의 수 구하기 */
 int nodeCount[MAX_SIZE];
 for (int i = 0; i < MAX_SIZE; i++)
     nodeCount[i] = 1;

 int union2(int x, int y){
     x = find(x);
     y = find(y);

     // 두 값의 root가 같지 않으면
     if(x != y) {
         root[y] = x; // y의 root를 x로 변경
         nodeCount[x] += nodeCount[y]; // x의 node 수에 y의 node 수를 더한다.
         nodeCount[y] = 1; // x에 붙은 y의 node 수는 1로 초기화
     }
     return nodeCount[x]; // 가장 root의 node 수 반환
 }
~~~


# 관련된 Post
* 자료구조 트리(Tree)에 대해 알고 싶으시면 [트리(Tree)란](https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html)을 참고하시기 바랍니다.


# References
> - [http://bowbowbow.tistory.com/26](http://bowbowbow.tistory.com/26)
> - [https://ratsgo.github.io/data%20structure&algorithm/2017/11/12/disjointset/](https://ratsgo.github.io/data%20structure&algorithm/2017/11/12/disjointset/)
