---
layout: post
title: '[알고리즘] Prim 알고리즘 이란'
subtitle: 'Prim의 MST 알고리즘을 이해할 수 있다.'
date: 2018-08-30
author: heejeong Kwon
cover: '/images/algorithm-mst/prim-main.png'
tags: algorithm mst prim
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Prim 알고리즘이란
> - Prim 알고리즘의 동작을 이해할 수 있다.
> - Prim 알고리즘을 구현할 수 있다.
> - Prim 알고리즘의 시간 복잡도


## Prim 알고리즘이란
시작 정점에서부터 출발하여 신장트리 집합을 단계적으로 확장해나가는 방법


## Prim 알고리즘의 동작
1. 시작 단계에서는 시작 정점만이 [MST(최소 비용 신장 트리)](https://gmlwjd9405.github.io/2018/08/28/algorithm-mst.html) 집합에 포함된다.
2. 앞 단계에서 만들어진 MST 집합에 인접한 정점들 중에서 최소 간선으로 연결된 정점을 선택하여 트리를 확장한다.
  * 즉, 가장 낮은 가중치를 먼저 선택한다.
3. 위의 과정을 트리가 (N-1)개의 간선을 가질 때까지 반복한다.


## Prim 알고리즘의 구체적인 동작 과정
Prim 알고리즘을 이용하여 MST(최소 비용 신장 트리)를 만드는 과정
  * **정점 선택을 기반** 으로 하는 알고리즘
  * 이전 단계에서 만들어진 신장 트리를 확장하는 방법
<br><br>
![](/images/algorithm-mst/prim-example.png){: width="450" height="700"}


## Prim 알고리즘 구현
~~~java
~~~


## Prim 알고리즘의 시간 복잡도
* 주 반복문이 정점의 수 n만큼 반복하고, 내부 반복문이 n번 반복
  * Prim의 알고리즘의 시간 복잡도는 **O(n^2)** 이 된다.
* Kruskal 알고리즘의 시간 복잡도는 **O(elog₂e)** 이므로
  * 그래프 내에 적은 숫자의 간선만을 가지는 '희소 그래프(Sparse Graph)'의 경우 Kruskal 알고리즘이 적합하고
  * 그래프에 간선이 많이 존재하는 '밀집 그래프(Dense Graph)' 의 경우는 Prim 알고리즘이 적합하다.


# 관련된 Post
* 최소 신장 트리(MST, Minimum Spanning Tree)에 대해 알고 싶으시면 [최소 신장 트리(MST)란](https://gmlwjd9405.github.io/2018/08/28/algorithm-mst.html)을 참고하시기 바랍니다.
* Kruskal MST 알고리즘에 대해 알고 싶으시면 [Kruskal 알고리즘이란](https://gmlwjd9405.github.io/2018/08/29/algorithm-kruskal-mst.html)을 참고하시기 바랍니다.
* 자료구조 그래프(Graph)에 대해 알고 싶으시면 [그래프(Graph)란](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)을 참고하시기 바랍니다.
* 자료구조 트리(Tree)에 대해 알고 싶으시면 [트리(Tree)란](https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html)을 참고하시기 바랍니다.


# References
> - [C언어로 쉽게 풀어 쓴 자료구조](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&barcode=9788970506432)
