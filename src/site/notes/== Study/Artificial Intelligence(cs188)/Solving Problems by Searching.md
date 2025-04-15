---
{"dg-publish":true,"permalink":"/== Study/Artificial Intelligence(cs188)/Solving Problems by Searching/","created":"2024-02-15T14:04:24.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---


> 한번의 동작action만으로 에이전트가 목표goal를 즉시 달성achieve할 수 없다면, 에이전트는 어떻게 다음 동작을 선택해야 하는가?

### search
인간이라면 상황을 보고, 어떻게 목표를 달성할 수 있는지 계산한 다음, 행동할 것이다.

이처럼 에이전트는 이런 상황 하에서 미래를 계획할 필요가 있다. 즉, **동작열sequence of actions**를 고찰해야 한다. 이때, 목표로의 동작열을 산출하기 위해 에이전트가 수행하는 계산 과정을 **탐색search**이라 하며, 그런 에이전트를 **문제 해결 에이전트problem-sloving agent**라고 부른다.

### problem formulation
탐색을 수행하기 위해 필요한 것은 목표와 문제의 **형식화formulation**이다. 현실의 복잡한 세부사항들 중 많은 부분은 문제해결에 도움이 되지 않는다. 이처럼 쓸데없는 정보를 제거하는 절차를 **추상화abstration**이라 부른다.
우리는 이러한 추상화를 바탕으로 세계를 표현하는 적당한 모형을 만들어내고, 그렇게 함으로써 여러가지 수학적 알고리즘을 이용하여 문제를 해결을 할 수 있게 된다.

# Search Strategies

탐색 전략은 정보가 없는 것과 정보가 있는 것으로 나눌 수 있다.
여기서 정보란 주어진 상태에서 목표로까지의 어떠한 다른 추가적인 단서를 의미한다.
예컨대, 지도상에서 주어진 상태와 목표까지의 직선거리 정보같은 것이 그러한 것에 해당한다. 이러한 정보는 우리가 최단경로를 찾는 경우에 도움이 되는 정보이다. 하지만, 과도한 추상화를 했을 경우, 우리는 이러한 도움되는 정보 없이 문제를 풀게 된다. 이러한 정보의 필요성을 깨닫고, 어떻게 목표를 탐색하는 데에 도움이 되는지 체계적으로 이해하기 위해, 정보없는 탐색 즉, 가장 단순한 형태의 탐색 전략부터 시작할 것이다.

## 요약
- [[== Study/Artificial Intelligence(cs188)/Uninformed Search Strategies\|Uninformed Search Strategies]]: 정보없는 탐색은 3가지의 기본 탐색 형태를 갖는다. DFS, BFS, UCS가 바로 그것이다.
- [[== Study/Artificial Intelligence(cs188)/Informed Search Strategies\|Informed Search Strategies]]: 정보있는 탐색은