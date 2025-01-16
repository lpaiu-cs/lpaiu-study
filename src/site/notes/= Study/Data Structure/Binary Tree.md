---
{"dg-publish":true,"permalink":"/study/data-structure/binary-tree/","created":"2023-12-04T23:02:34.000+09:00","updated":"2023-12-04T23:02:34.000+09:00"}
---

# Tree

연결 [[= Study/Data Structure/Graph\|Graph]] 이고, cycle을 가지지 않는 경우 Tree라 칭한다.
각각의 요소들을 노드Node라 부른다. (보통 연결된 요소들을 노드라 하는 것 같다.)

### Root Node - Leaf Node
부모 노드를 갖지 않는, 즉 최상위 노드를 Root Node라 한다.
반대로, 자식 노드를 갖지 않는, 최하위 노드를 Leaf Node라 한다.

# Binary Tree

자식이 최대 2개인 Tree를 칭함.
완전이진트리와는 달리, 보편적인 이진트리는 배열로 표현하기 어렵다.

## 탐색 (순회)
너비 우선 탐색과 깊이 우선 탐색으로 나눠진다.
전자는 큐를 통해 구현할 수 있고, 후자는 스택, 즉 재귀적으로 구현된다.

### Breadth First Search (너비 우선 탐색)
- 좌우로 같은 레벨에 위치한 노드를 우선 방문
- Queue를 이용하여 구현
#### level-order Traversal
동일한 깊이(level)을 갖는 노드를 좌측부터 차례대로 방문하고, 모두 순회한 경우 아래로 한단계 내려가는 방법
	e.g) 0 level 모두 좌우로 순회 한 다음, 1 level 순회, 2 level 순회 ....
- Queue를 이용해 구현. (dequeue한 노드의 자식을 enqueue 한다)

### Depth First Search (깊이 우선 탐색)
- 인접한 노드를 따라 walk할 수 있는 가장 깊은 곳까지 방문 
- 재귀를 이용하여 구현

#### Pre/In/Post-order Traversal
전위 순회: **Root Node** > Left Subtree > Right Subtree
중위 순회: Left Subtree > **Root Node** > Right Subtree
후위 순회: Left Subtree > Right Subtree > **Root Node**

