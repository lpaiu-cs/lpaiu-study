---
{"dg-publish":true,"permalink":"/== Study/Algorithm/Kruskal Algorithm/","created":"2024-02-04T13:01:28.000+09:00","updated":"2025-01-14T15:33:43.000+09:00"}
---

[[== Study/Algorithm/Prim Algorithm\|Prim Algorithm]]을 이용해 Minimum Spanning Tree를 만들었다. 하지만 이 문제를 해결하는 방법이 이것뿐은 아니다. 프림 알고리즘에서 정점에 집중했듯이, 이번엔 간선에 집중할 수도 있다.

# Kruskal Algorithm
가장 싼 간선들을 차례대로 연결하면서, Cycle만 생성되지 않도록 하면 된다.

![IMG_1232.jpeg](/img/user/z-Attached%20Files/IMG_1232.jpeg)

위 그림에서 가장 싼 비용순으로 정리를 해보자.
15억 / 아름 - 도담
17억 / 보람 - 고운
19억 / 고운 - 도담
22억 / 고운 - 아름
28억 / 보람 - 아름
30억 / 보람 - 해밀
42억 / 해밀 - 고운

이렇게 될 것이다. 이제, 가장 싼 비용부터 차례대로 꺼내보면, 아름동과 도담동이 연결된다. 그 다음엔 보람동과 고운동이 연결될 것이다. 그 다음에는 고운동과 도담동이 연결되어 아래와 같은 그림이 만들어진다.

![Pasted image 20240204125917.png](/img/user/z-Attached%20Files/Pasted%20image%2020240204125917.png)

이제, 그 다음 연결을 생각해보자. 22억 / 고운 - 아름 이다. 하지만, 이 연결을 수행하면 Cycle이 만들어진다. 즉, 비효율적이다. 따라서 최소신장트리의 조건에 위배되므로 넘긴다.

이런식으로 순서대로 연결을 수행해 나가는 것이 크루스칼 알고리즘이다.