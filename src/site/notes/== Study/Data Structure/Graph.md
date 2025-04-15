---
{"dg-publish":true,"permalink":"/== Study/Data Structure/Graph/","created":"2023-12-04T23:02:30.000+09:00","updated":"2023-12-04T23:02:30.000+09:00"}
---

# Graph

- 두 대상 사이 관계를 나타낼 수 있는 자료구조
- 정점vertex
- 간선edge

### walk
두 정점을 잇는 간선이 존재하는 경우 둘을 **인접**한다고 정의
인접한 정점을 따라 이동한 족적을 walk라 함.

- 시작점과 끝점이 동일한 경우 닫힌 walk, 그렇지 않은 경우 열린 walk라 함.

### path
walk 중에 동일한 정점을 두번 지나지 않는 경우, path라 한다. (단, 시작점과 끝점 제외)

### cycle
시작점과 끝점이 동일한 path의 경우, cycle이라 한다. 
