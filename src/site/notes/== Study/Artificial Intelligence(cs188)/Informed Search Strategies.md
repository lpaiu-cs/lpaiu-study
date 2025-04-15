---
{"dg-publish":true,"permalink":"/== Study/Artificial Intelligence(cs188)/Informed Search Strategies/","created":"2024-04-01T15:12:38.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---

 
[[== Study/Artificial Intelligence(cs188)/Uninformed Search Strategies\|Uninformed Search Strategies]]보다 실세계의 정보를 반영하는 전략이다. 우리는 더 많은 것을 고려할 것이고, 최적해 또는 그에 가까운 해를 찾기 위해 더 효율적인 전략을 사용할 수 있다. 목표점에 가까운지 먼지 추론할 수 있는 이러한 힌트들은 **발견적 함수heuristic function**의 형태로 주어진다.

앞서 살펴본 tree search 들은 결국 priority queue의 우선순위함수 f(n)을 어떻게 정의하는지에 달려있었다. 우리는 이제 f(n)에 발견적 함수 h(n)의 정보를 녹여낼 것이다.

>**h(n) = 노드 n의 상태에서 목표 상태로 가는 최저 비용 경로의 비용 추정치**

# Search Algorihms

### Greedy Best-First Search
f(n) = h(n)

따라서, 유한 상태 공간에서 완결성을 갖지만 최적성을 보장하지 못한다.
DFS와 비슷하지만, 완전히 잘못된 반대방향으로 가는 일은 없을 것이다.
무한 상태 공간에서는 완결성을 보장할 수 없다.
휴리스틱 성능에 전적으로 의존하며 최악의 경우 시간 및 공간복잡도는 O(V)ㅡV는 정점 수ㅡ지만, 문제에 따라 O(bm)도 가능하다.

### A* Search
f(n) = g(n) + h(n)
g(n)은 S -> n 경로 비용이고,
h(n)은 n -> G 추정 경로 비용이다.

직전의 GBFS는 그리디한 특성ㅡn -> G의 비용만 생각하는ㅡ으로인해 최적성을 보장받을 수 없었다. 이를 해결하기 위해, A* 검색은 S -> n 이라는 정보 또한 포함하여, 현재 경로에 대한 평가 또한 고려하고자 하였다.

A\* 검색은 완결적이다. 그런데, 최적은 일반적으로 아니다. A\*가 최적이기 위해서는, 휴리스틱 함수가 **허용성admissibility**를 지녀야 한다. 허용성을 갖기 위해서는 목표까지의 추정 비용을 낙관적으로 바라봐야 한다. 만약, 너무 비관적으로 바라본다면, 우리는 최적경로에 위치한 상태를 확장시키지 못할 수 있다. 반면, 낙관적으로 바라본다면, 최적경로가 아닌 상태에서 목표 상태를 확장하기 전에 반드시, 최적경로에 위치한 상태를 확장하게 된다. 언제나 실제 경로에 비해 낙관적으로 바라보기 때문이다.

![Pasted image 20240311132812.png](/img/user/z-Attached%20Files/Pasted%20image%2020240311132812.png)

A\*가 UCS와 다른 점은, 휴리스틱에 의해 목표에서 멀수록 비용을 높게 평가하는 경향성을 띤다는 것이다. 만약, h(n) = 0이라면 A\*와 UCS는 완전히 동일하다.

## Memory-bounded Search

A\* 검색의 주된 문제점은 **메모리를 많이 사용한다**는 것이다.
A\*가 사용하는 메모리는 거의 대부분 *frontier*(전선을 담는 자료구조;priority queue)와 *reached*(도달된 상태들을 담는 자료구조;set)가 차지한다.
*reached*의 역할은 무한루프를 일으키는 U턴 동작의 실행을 막는 것이다. 이러한 *reached*의 크기는 *frontier*의 비해 작은 것이 보통이기 때문에 별 문제는 아니다. (만약 없앤다면, U턴을 방지하는 알고리즘을 짜야 하고, 이는 속도를 조금 느리게 만들 것이다.)

여기서는 주된 문제인 *frontier*와 관련하여 메모리를 아끼는 몇가지 알고리즘을 소개한다.

### Beam Search
전선의 크기를 제한하는 방법이다. 가장 간단한 구현으로는 상위 k개의 노드만 전선에 저장한다.
그러면 비완결적이고 준최적이 되지만, 적절한 k는 메모리를 절약하고 시간복잡도도 줄인다.

![Pasted image 20240311140850.png|600](/img/user/z-Attached%20Files/Pasted%20image%2020240311140850.png)

### IDA\*(Iterative-Deepening A\*)


### RBFS(Recursive Best-First Search)
