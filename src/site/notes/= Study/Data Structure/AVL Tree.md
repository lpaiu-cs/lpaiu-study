---
{"dg-publish":true,"permalink":"/study/data-structure/avl-tree/","created":"2024-02-06T00:33:06.000+09:00","updated":"2024-02-06T00:33:06.000+09:00"}
---

[[= Study/Data Structure/Binary Search Tree\|Binary Search Tree]]에는 균형이 깨질 수 있다는 치명적인 단점이 있었다.
균형을 어떻게 보완할 수 있었을까?
# AVL Tree
Binary Searh Tree의 일종으로서, 모든 노드의 Balance Factor의 절댓값이 1 이하인 형태.
*(Adelson-Velsky, Landis의 이름 앞글자를 따 AVL이라 명명)*

균형이 맞춰진 트리는 삽입 및 삭제에 의해 균형이 깨질 수 있다.
B.F의 값이 1을 초과하여 2가 되는 경우는 다음 4가지이다.
```
-1-         -2-          -3-         -4-
	   A      A             A          A
	  /        \           /            \
   B          B         B              B
  /            \         \            /
 C              C         C          C
```
위 4가지 모두 A의 B.F값이 2로서 문제가 발생하고 있다.
따라서 A의 위치를 조정하는 것이 중요하다.
1번과 2번은 쉽다.
3번과 4번은 C가 맨 위로가서 양옆에 B와 C를 자식으로 갖는 형태가 된다.

==결국 핵심은 트리의 총 높이를 낮추는 데에 있다.==

#play 파이썬으로 구현 직접 해볼것.
