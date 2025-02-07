---
{"dg-publish":true,"permalink":"/= Study/Logic Design/Flip-Flop/","created":"2023-12-04T22:55:18.000+09:00","updated":"2025-01-14T15:33:45.000+09:00"}
---

# Flip-Flop
단순히 [[= Study/Logic Design/Latch\|Latch]]에 클럭(clock)을 추가하기만 한 Level-sensitive는 여전히 Asynchronous이다.
어쨋거나, data의 변동에 의해 제멋대로 아무때나 바뀔 수 있기 때문이다.
따라서, 정확한 순간에만 한 번씩 변하는 Edge-sensitive만이 타이밍 제어가 된다고 할 수 있다.
이를 Flip-Flop이라 한다.

타이밍 제어는 데이터가 정확한 순간에만 조작되도록 하기 때문에
오류를 줄이고, 예상된 동작만 일어나도록 보장할 수 있다는 장점이 있다.

가장 간단한 플립플롭으로는, D latch의 Enable에 클럭을 연결하는 것이다.
그러면 데이터는 **positive level**에 캐치되며, 그 외에는 항상 홀드 상태이다.

# Master-Slave D flip-flop
for. 데이터 처리 시점을 매우 정확하게 제어하기 위함.
두개의 D 래치가 지배-종속 관계로 묶여있는 플립플롭을 의미한다.

- Positive-edge 또는 Negative-edge일 때에 데이터를 캐치한다.

![Master-Slave D flip-flop.jpeg|700](/img/user/z-Attached%20Files/Master-Slave%20D%20flip-flop.jpeg)
▲D latch를 이용해 Circuit을 구현했다.

![Pasted image 20231113021108.png|600](/img/user/z-Attached%20Files/Pasted%20image%2020231113021108.png)
▲Master-Slave D flip-flop의 심볼

# JK flip-flop
for. **SR latch**의 플립플롭 버전을 안정적으로 구동하기 위함.
SR latch에서 1 1이 들어왔을때 error가 났던것과 달리 1 1 일때, 토글 기능을 제공한다. 따라서 유연성을 갖고 복잡한 제어를 할 수 있기 때문에, 상태 전이가 빈번한 로직에서 선호된다.

![JK flip-flop.jpeg|700](/img/user/z-Attached%20Files/JK%20flip-flop.jpeg)

J|K|D; Next State
--|--|--
0|0|Q; Hold 
0|1|0; Reset
1|0|1; Set
1|1|Q' ; Toggle: Undefine이 아니므로 error 없이 안정적인 구동이 가능하다.


# T flip-flop
for. toggle 즉, 상태 반전만을 위한 플립플롭.
JK flip-flop에서 토글 기능만을 남겨놓은 것이다.

![T flip-flop.png|700](/img/user/z-Attached%20Files/T%20flip-flop.png)
▲(a)와 (b)는 각각 다른 플립플롭을 통해 구현한 것이다. 하지만 JK안에 D가 포함되어 있다는 것을 생각하면, 실제로는 (b)가 더 간단한 써킷일것 같다.

T|Next State
--|--
0|Q
1|Q'
