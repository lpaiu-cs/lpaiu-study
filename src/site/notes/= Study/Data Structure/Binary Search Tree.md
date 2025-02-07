---
{"dg-publish":true,"permalink":"/study/data-structure/binary-search-tree/","created":"2023-12-04T23:16:25.000+09:00","updated":"2023-12-04T23:16:25.000+09:00"}
---

# Binary Search Tree

[[= Study/Data Structure/Binary Tree\|Binary Tree]]의 일종으로, 탐색을 목적으로 정렬된 트리구조.
임의 노드의 왼쪽 서브트리에는 해당 노드보다 작은 값만이, 오른쪽 서브트리에는 큰 값만이 존재한다.
>Heap과 달리 왼쪽 subtree와 오른쪽 subtree 간에 위계가 있기 때문에 정렬면에서 더 나아간 트리라고 할 수 있다.

이 노드를 in-order로 순회하면 오름차순 정렬을 하는 셈이다.

>탐색: 이진 탐색; O($\log n$)
>삽입: Root Node에서 출발; O($\log n$) 
>제거: Left Sub의 최대 노드가 그 자리를 대체한다 또는 Right Sub의 최소 노드가 대체.


> [!warning] 문제점
> [[= Study/Data Structure/Binary Search Tree\|Binary Search Tree]]는 분명 획기적인 자료구조이다.
> 하지만 **균형이 깨질 수도 있다**는 치명적인 단점을 가지고 있다.
> ex) 일자로 연결된 트리.

이 단점을 보완하지 않는다면 자료구조가 제 성능을 낼 수 없을 것이다.
균형을 깨지 않는 규칙을 추가하기 앞서 먼저 균형을 정의하자.
### Balance Factor
= (Left Subtree의 높이) - (Right Subtree의 높이)

>즉, Balance Factor의 차이가 크면, 균형이 많이 깨진 상태라고 할 수 있다.
>아래는 이 **균형을** 삽입 및 삭제의 과정에서 **유지**할 수 있는 규칙을 가진 트리들이다.

모든 노드에서 B.F가 일정 이하라면, 그 트리는 균형있는 트리라고 할 수 있을 것이다.
아래는 그러한 규칙이 추가된 트리들이다.
- [[= Study/Data Structure/AVL Tree\|AVL Tree]]
- [[= Study/Data Structure/Red-Black Tree\|Red-Black Tree]]
- [[= Study/Data Structure/B-Tree\|B-Tree]]
