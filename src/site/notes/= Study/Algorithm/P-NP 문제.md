---
{"dg-publish":true,"permalink":"/study/algorithm/p-np/","created":"2023-12-07T20:32:40.000+09:00","updated":"2025-01-14T15:33:43.000+09:00"}
---


### 다항시간 알고리즘Polynomial-Time Alogrithm
다항시간 내에 해결 가능한 알고리즘

# P-문제: Polynomial
문제를 해를 **도출**하는 다항시간 알고리즘이 존재하는 경우 P-문제라고 부른다.

# NP-문제: Non-deterministic Ploynomial
문제의 해를 **검증**하는 다항시간 알고리즘이 존재하는 경우 NP-문제

>왜 도출과 검증으로 워딩 차이가 나는가?
>$\rightarrow$ 해의 검증조차 다항시간내에 불가능한 알고리즘은 고려할 가치조차 없다.

해를 도출한다는 것은 자명히 해에 대한 검증 또한 포함.
$\therefore \forall$ "P-문제" $\implies$ "NP-문제"

# P=NP vs P≠NP

### NP-hard
NP에 속하는 모든 문제를 다항시간 내에 X 문제로 환원가능하다면, X문제를 NP-hard라 한다.
NP-hard는 가장 어려운 문제로서, 어떤 문제가 NP-hard임을 알면, 다른 근사 알고리즘으로 문제를 해결하도록 유도할 수 있다.

### NP-complete
NP이면서 동시에 NP-hard인 문제를 이른다.
만약, 어떤 NP-complete 문제가 P문제임이 밝혀진다면, P=NP임이 증명된다는 의의를 지닌다.

![Pasted image 20231207203133.png](/img/user/z-Attached%20Files/Pasted%20image%2020231207203133.png)
#Q 왜이렇게 그려지는지 솔직히 이해안됨.