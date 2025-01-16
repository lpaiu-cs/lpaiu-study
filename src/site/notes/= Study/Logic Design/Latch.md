---
{"dg-publish":true,"permalink":"/= Study/Logic Design/Latch/","created":"2023-12-18T02:49:10.000+09:00","updated":"2025-01-14T15:33:45.000+09:00"}
---

# Latch
정보를 catch하고 hold할 수 있는 메모리 소자.
# SR latch

![Pasted image 20231113013216.png|500](/img/user/z-Attached%20Files/Pasted%20image%2020231113013216.png)
Set과 Reset이라는 두개의 입력을 사용한다.
- S = 1, R = 0 이면, Q = 1
- S = 0, R = 1 이면, Q = 0
- S = 0, R = 0 이면, hold
- S = 1, R = 1 이면, undefine **오류**가 발생한다.

처음 두개의 인풋이 들어올 때, 정보를 catch하고,
세번째의 경우, 정보를 hold 한다고 볼 수 있다.

>[!warning] 문제점
![IMG_1147.jpeg|150](/img/user/z-Attached%20Files/IMG_1147.jpeg)
▲S와 R의 input은 동시에 변하는게 아니다. 따라서 오류가 발생할 가능성이 있다.


# SR latch with control input
목표: Enable을 추가함으로써 SR 11을 방지한다.
control input 즉, Enable을 추가함으로써, 잠시 신호를 차단해둘 수 있게되었고, 안정성이 확보되었다.

![Pasted image 20231113013531.png](/img/user/z-Attached%20Files/Pasted%20image%2020231113013531.png)

C | S | R | Next State of Q
--|-- |-- |--
0 | X | X | Hold 
1 | 0 | 0 | Hold
1 | 0 | 1 | Q = 0; Reset state
1 | 1 | 0 | Q = 1; Set state
1 | 1 | 1 | Undefined

어차피 C가 홀드를 결정하기 때문에 더 이상 SR로 홀딩할 이유가 없어졌다.
따라서 100과 111을 없애 구조를 단순화 시킨 latch가 탄생했다.
그것이 바로 D latch이다.

# D latch
구조적 단순성을 갖춘 직관적인 latch.

![IMG_1150.jpeg](/img/user/z-Attached%20Files/IMG_1150.jpeg)

C | D | Next State of Q
--|-- |--
0 | X | Hold  
1 | 0 | Q = 0; Reset State
1 | 1 | Q = 1; Set state
