---
{"dg-publish":true,"permalink":"/study/logic-design/counter/","created":"2023-12-18T06:23:36.000+09:00","updated":"2025-01-14T15:33:45.000+09:00"}
---


# Counter
시간의 흐름을 표현하는 것을 카운터라고 한다. [[= Study/Logic Design/Registers\|레지스터]]의 한 형태라 할 수 있다.
클록의 주기를 1초로 맞춰두면, 1초 단위의 시간 흐름을 표현할 수 있다.

# Asynchronous Counter
첫번째 [[= Study/Logic Design/Flip-Flop\|플립플롭]]만 외부 클록의 신호를 받고, 나머지 플립플롭은 이전 플립플롭의 출력을 클록으로 받는다. 따라서 구조적으로 단순하지만, 클록 엣지가 모든 플립플롭에 동시에 도달하지 못한다. 따라서 출력에 지연이 생기며, 높은 클록 주기에서는 이 지연에 의해 오류가 생길 수 있다.

![IMG_1185.jpeg](/img/user/z-Attached%20Files/IMG_1185.jpeg)
▲ BCD 코드를 이용하여 시간을 나타내는 10초카운터의 로직 다이어그램. 타이밍도에서 각 State들의 플립이 서로 다른 지연시간을 가지고 이뤄지는 것을 볼 수 있다.

## Three-Decade Decimal BCD Counter

![IMG_1190.jpeg](/img/user/z-Attached%20Files/IMG_1190.jpeg)
▲ BCD 리플 카운터의 간단한 활용

# Synchronous Counter
모든 플립플롭이 하나의 클록 신호에 동기화 되므로, 추가적인 지연이 없어서 높은 동작 속도에서도 안정적이다.

![IMG_1187.jpeg](/img/user/z-Attached%20Files/IMG_1187.jpeg)
▲ 우상단 타이밍도를 리플 플립플롭과 비교해보면, Synchronous 카운터의 경우 각 State가 동시에 플립된다.


![IMG_1202.jpeg|600](/img/user/z-Attached%20Files/IMG_1202.jpeg)
▲ 직전에 보았던 4-Bit Synchronous Binary Counter를 응용하여, Down을 추가한 것이다.

![IMG_1203.jpeg|600](/img/user/z-Attached%20Files/IMG_1203.jpeg)
▲ Parallel Load 기능이 추가된 카운터이다.

이를 BCD 카운터로 만들기 위해서는, 적절한 시기에 0000을 Load해 주거나 Clear를 해줘야 한다.
따라서 구현하는 방법이 2가지인데, 이는 아래와 같다.

![IMG_1204.jpeg](/img/user/z-Attached%20Files/IMG_1204.jpeg)
▲ (a)는 0000을 load해 줌으로써 초기화 하고, (b)는 clear에 0을 넣어줌으로써 초기화한다.

누가 더 빠른 템포로 초기화되겠는가? 왜 이렇게 짜였는가? 에 대해 고민해보면, 한층 더 이해가 깊어질 수 있다.