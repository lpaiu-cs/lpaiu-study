---
{"dg-publish":true,"permalink":"/== Study/Algorithm/Insertion Sort/","created":"2023-12-04T23:03:46.000+09:00","updated":"2025-01-14T15:33:43.000+09:00"}
---

i번 요소를 앞 요소들과 비교하여(비교시점에 앞 요소들은 정렬되어 있음) 적절한 자리에 넣는 행위를 *for i in range(1, n)* 으로 수행한다.
앞 요소들과의 비교는 i-1, i-2, i-3 이렇게 순차적으로 비교해간다.

최악의 경우 O($n^2$)
최선의 경우 O(n)
