---
layout: post
title: '[알고리즘] 최소 신장 트리(MST, Minimum Spanning Tree)란'
subtitle: 'Spanning Tree, MST의 개념을 이해할 수 있다.'
date: 2018-08-28
author: heejeong Kwon
cover: '/images/algorithm-mst/mst-main.png'
tags: algorithm spanning-tree mst graph tree
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Spanning Tree의 개념과 특징을 이해할 수 있다.
> - Spanning Tree의 사용 사례
> - MST의 개념과 특징을 이해할 수 있다.
> - MST의 사용 사례
> - MST의 구현 방법에 대해 간단히 정리한다.


# Spanning Tree
## Spanning Tree란
그래프 내의 모든 정점을 포함하는 트리
* **Spanning Tree = 신장 트리 = 스패닝 트리**
* Spanning Tree는 그래프의 **최소 연결 부분 그래프** 이다.
  * 최소 연결 = 간선의 수가 가장 적다.
  * n개의 정점을 가지는 그래프의 최소 간선의 수는 (n-1)개이고, (n-1)개의 간선으로 연결되어 있으면 필연적으로 트리 형태가 되고 이것이 바로 Spanning Tree가 된다.
* 즉, 그래프에서 일부 간선을 선택해서 만든 트리

## Spanning Tree의 특징
![](/images/algorithm-mst/spanning-tree.png){: width="400" height="250"}
* DFS, BFS을 이용하여 그래프에서 신장 트리를 찾을 수 있다.
  * 탐색 도중에 사용된 간선만 모으면 만들 수 있다.
* 하나의 그래프에는 많은 신장 트리가 존재할 수 있다.
* Spanning Tree는 트리의 특수한 형태이므로 **모든 정점들이 연결** 되어 있어야 하고 **사이클을 포함해서는 안된다.**
* 따라서 Spanning Tree는 그래프에 있는 **n개의 정점을 정확히 (n-1)개의 간선으로 연결** 한다.

## Spanning Tree의 사용 사례
통신 네트워크 구축
![](/images/algorithm-mst/spanning-tree-example.png){: width="300" height="150"}
* 예를 들어, 회사 내의 모든 전화기를 가장 적은 수의 케이블을 사용하여 연결하고자 하는 경우
* n개의 위치를 연결하는 통신 네트워크를 최소의 링크(간선)를 이용하여 구축하고자 하는 경우, 최소 링크의 수는 (n-1)개가 되고, 따라서 Spanning Tree가 가능해진다.


# MST
## MST란
Spanning Tree 중에서 사용된 간선들의 가중치 합이 최소인 트리
* **MST = Minimum Spanning Tree = 최소 신장 트리**
* 각 간선의 가중치가 동일하지 않을 때 단순히 가장 적은 간선을 사용한다고 해서 최소 비용이 얻어지는 것은 아니다.
* MST는 간선에 가중치를 고려하여 최소 비용의 Spanning Tree를 선택하는 것을 말한다.
* 즉, 네트워크(가중치를 간선에 할당한 그래프)에 있는 모든 정점들을 가장 적은 수의 간선과 비용으로 연결하는 것이다.

## MST의 특징
1. 간선의 가중치의 합이 최소여야 한다.
2. n개의 정점을 가지는 그래프에 대해 반드시 (n-1)개의 간선만을 사용해야 한다.
3. 사이클이 포함되어서는 안된다.

## MST의 사용 사례
통신망, 도로망, 유통망에서 길이, 구축 비용, 전송 시간 등을 최소로 구축하려는 경우
![](/images/algorithm-mst/mst-example.png){: width="300" height="150"}
* 도로 건설
  * 도시들을 모두 연결하면서 도로의 길이가 최소가 되도록 하는 문제
* 전기 회로
  * 단자들을 모두 연결하면서 전선의 길이가 가장 최소가 되도록 하는 문제
* 통신
  * 전화선의 길이가 최소가 되도록 전화 케이블 망을 구성하는 문제
* 배관
  * 파이프를 모두 연결하면서 파이프의 총 길이가 최소가 되도록 연결하는 문제

## MST의 구현 방법
### 1. Kruskal MST 알고리즘
**탐욕적인 방법(greedy method)** 을 이용하여 네트워크(가중치를 간선에 할당한 그래프)의 모든 정점을 최소 비용으로 연결하는 최적 해답을 구하는 것
* MST(최소 비용 신장 트리) 가 *1) 최소 비용의 간선으로 구성됨 2) 사이클을 포함하지 않음* 의 조건에 근거하여 **각 단계에서 사이클을 이루지 않는 최소 비용 간선을 선택** 한다.
* 간선 선택을 기반으로 하는 알고리즘이다.
* 이전 단계에서 만들어진 신장 트리와는 상관없이 무조건 최소 간선만을 선택하는 방법이다.

[과정]
1. 그래프의 간선들을 가중치의 오름차순으로 정렬한다.
2. 정렬된 간선 리스트에서 순서대로 사이클을 형성하지 않는 간선을 선택한다.
  * 즉, 가장 낮은 가중치를 먼저 선택한다.
  * 사이클을 형성하는 간선을 제외한다.
3. 해당 간선을 현재의 MST(최소 비용 신장 트리)의 집합에 추가한다.

<mark>참고</mark>  구체적인 내용은 [Kruskal MST 알고리즘이란](https://gmlwjd9405.github.io/2018/08/29/algorithm-kruskal-mst.html) 참고

### 2. Prim MST 알고리즘
시작 정점에서부터 출발하여 신장트리 집합을 **단계적으로 확장** 해나가는 방법
* 정점 선택을 기반으로 하는 알고리즘이다.
* 이전 단계에서 만들어진 신장 트리를 확장하는 방법이다.

[과정]
1. 시작 단계에서는 시작 정점만이 MST(최소 비용 신장 트리) 집합에 포함된다.
2. 앞 단계에서 만들어진 MST 집합에 인접한 정점들 중에서 최소 간선으로 연결된 정점을 선택하여 트리를 확장한다.
  * 즉, 가장 낮은 가중치를 먼저 선택한다.
3. 위의 과정을 트리가 (N-1)개의 간선을 가질 때까지 반복한다.

<mark>참고</mark>  구체적인 내용은 [Prim MST 알고리즘이란](https://gmlwjd9405.github.io/2018/08/30/algorithm-prim-mst.html) 참고

## MST 관련 문제
* [최소 스패닝 트리-백준1197번](https://www.acmicpc.net/problem/1197)
* [네트워크 연결-백준1922번](https://www.acmicpc.net/problem/1922)


# 관련된 Post
* 자료구조 그래프(Graph)에 대해 알고 싶으시면 [그래프(Graph)란](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)을 참고하시기 바랍니다.
* 자료구조 트리(Tree)에 대해 알고 싶으시면 [트리(Tree)란](https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html)을 참고하시기 바랍니다.
* Kruskal MST 알고리즘에 대해 알고 싶으시면 [Kruskal 알고리즘이란](https://gmlwjd9405.github.io/2018/08/29/algorithm-kruskal-mst.html)을 참고하시기 바랍니다.
* Prim MST 알고리즘에 대해 알고 싶으시면 [Prim 알고리즘이란](https://gmlwjd9405.github.io/2018/08/30/algorithm-prim-mst.html)을 참고하시기 바랍니다.

# References
> - [C언어로 쉽게 풀어 쓴 자료구조](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&barcode=9788970506432)
