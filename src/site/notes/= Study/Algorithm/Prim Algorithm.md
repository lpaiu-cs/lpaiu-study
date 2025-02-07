---
{"dg-publish":true,"permalink":"/= Study/Algorithm/Prim Algorithm/","created":"2024-02-04T13:21:02.000+09:00","updated":"2025-01-14T15:33:43.000+09:00"}
---

Minimum Spanning Tree 문제를 해결하기 위한 알고리즘 중 하나.

### Minimum Spanning Tree
주어진 가중 Graph에 존재하는 모든 Node를 연결하는 [[= Study/Data Structure/Binary Tree\|Tree]]를 만들되, 최소 비용으로 연결하는 것.
(이때, Graph가 아닌 Tree인 이유는 Cycle은 낭비이기 때문이다.)

# Prim Algorithm

임의의 정점에서 연결 가능한 가장 싼 정점을 연결한다.
그렇게 연결된 덩어리에서 또 다시 연결 가능한 가장 싼 정점을 연결한다. 이것을 반복하여, 덩어리를 계속 확장시켜 나간다. 단, Cycle은 만들어지지 않게 한다.

![IMG_1230.jpeg](/img/user/z-Attached%20Files/IMG_1230.jpeg)
예컨대 위와 같은 상황에서, "보람동"을 선택한 후, 확실히 가장 싼 연결은 고운동이다.
그렇게 보람동 고운동 연결이 만들어진 후에는

![IMG_1231.jpeg](/img/user/z-Attached%20Files/IMG_1231.jpeg)

이 덩어리에서 가장 싼 연결은 고운동-도담동이 된다. 이런식으로 확장해 나가면 최소신장트리 문제가 해결된다.