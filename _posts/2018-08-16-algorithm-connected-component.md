---
layout: post
title: '[알고리즘] 연결 성분(Connected Component) 찾는 방법'
subtitle: '연결 성분을 이해할 수 있다.'
date: 2018-08-16
author: heejeong Kwon
cover: '/images/data-structure-graph/connected-component-main.png'
tags: algorithm graph connected-component
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 연결 성분(Connected Component)이란
> - 연결 성분(Connected Component)을 찾는 방법

## 연결 성분(Connected Component)이란
나누어진 각각의 그래프
* 그래프는 여러 개의 고립된 부분 그래프(Isolated Subgraphs)로 구성될 수 있다.
* 즉, 서로 연결되어 있는 여러 개의 고립된 부분 그래프 각각을 연결 성분이라고 부른다.

## 연결 성분의 특징
* 연결 성분에 속한 모든 정점을 연결하는 경로가 있어야 한다.
* 또, 다른 연결 성분에 속한 정점과 연결하는 경로가 있으면 안된다.
* 연결 성분을 찾는 방법은 BFS, DFS 탐색을 이용하면 된다.

## 연결 성분을 찾는 방법
연결 성분을 찾는 방법은 너비 우선 탐색(BFS), 깊이 우선 탐색(DFS)을 이용하면 된다.
1. BFS, DFS를 시작하면 시작 정점으로부터 도달 가능한 모든 정점들이 하나의 연결 성분이 된다.
2. 다음에 방문하지 않은 정점을 선택해서 다시 탐색을 시작하면 그 정점을 포함하는 연결 성분이 구해진다.
3. 이 과정을 그래프 상의 모든 정점이 방문될 때까지 반복하면 그래프에 존재하는 모든 연결 성분들을 찾을 수 있다.

## 연결 성분의 개수 구하는 방법
~~~java
boolean[] visited = new boolean[N + 1]; // N: 정ㅇ점의 수
int cntComponent = 0;
// 각 정점에 대해서 한번씩 확인.
for (int i = 1; i <= N; i++) {
    if (!visited[i]) { // 방문했던 정점은 지나치므로, 연결이 떨어진 정점에 대해서만 카운트++
        dfs(arrayLists, i, visited); // dfs 탐색
        cntComponent++;
    }
}
~~~

# 관련된 Post
* 자료구조 그래프(Graph)에 대해 알고 싶으시면 [그래프(Graph)란](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)을 참고하시기 바랍니다.
* 깊이 우선 탐색(DFS, Depth-First Search): [깊이 우선 탐색(DFS)이란](https://gmlwjd9405.github.io/2018/08/14/algorithm-dfs.html) 을 참고하시기 바랍니다.
* 너비 우선 탐색(BFS, Breadth-First Search): [너비 우선 탐색(BFS)이란](https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html) 을 참고하시기 바랍니다.


# References
> - [C언어로 쉽게 풀어 쓴 자료구조](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&barcode=9788970506432)
