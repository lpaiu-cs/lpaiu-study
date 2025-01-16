---
{"dg-publish":true,"permalink":"/study/algorithm/dijkstra-algorithm/","created":"2024-02-04T18:21:04.000+09:00","updated":"2025-01-14T15:33:43.000+09:00"}
---

### Shortest Path 문제
Minimum Spanning Tree 문제와는 달리, **경로**를 가진다. 즉, 단순한 [[= Study/Data Structure/Graph\|Graph]]상에서의 문제가 아닌, Directed Graph 상에서의 최소비용 문제를 다루는 것이다.

# Dijkstra Algorithm (=uniform-cost search)
[[= Study/Algorithm/Prim Algorithm\|Prim Algorithm]]과 동일한 방법을 사용한다. 다만, 프림 알고리즘이 트리를 만들었다면, 다익스트라는 유향 그래프, 즉 경로를 만든다.

경로이기 때문에, 출발지점과 목표지점을 갖는다.
출발점에서 프림과 마찬가지로 문제에 접근한다. 가장 싸게 연결가능한 정점들을 탐색해 나간다.
후보(임시)들을 선정하고, 그 중 가장 싼놈과 연결(확정)하는 것을 반복하는 것이다.

# 한계
가중치가 음수인 경우가 존재하면 최적해를 보장하지 못한다. 이를 해결하기 위해서는 [[Bellman-Ford Algorithm.\|Bellman-Ford Algorithm.]]을 사용해야 한다.