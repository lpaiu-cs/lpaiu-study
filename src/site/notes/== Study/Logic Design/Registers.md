---
{"dg-publish":true,"permalink":"/== Study/Logic Design/Registers/","created":"2023-12-18T05:39:16.000+09:00","updated":"2025-01-14T15:33:45.000+09:00"}
---


# Registers
A group of **binary cells** suitable for holding binary information.

![4-bit register.jpeg|250x650](/img/user/z-Attached%20Files/4-bit%20register.jpeg)

각 [[== Study/Logic Design/Flip-Flop\|FF]]는 이진 정보를 담는 **binary cell**이라 할 수 있다.
이러한 Register는 정보를 저장하는 Memory element라 부를 수 있다.

4-Bit Register의 ==단점==은, Input에 특정 값을 계속 넣어주고 있어야만, $A_n$에 값이 저장된다는 것이다.

이것을 ==해결==하기 위해 아래의 Register가 출현한다.
## Register with Parallel Load
여기서 Parallel은 Serial의 반댓말로서 Input이 연속이 아닌 병렬로 들어옴을 의미한다.
아래의 레지스터는 Parallel Load를 받아들임으로써, Load가 1일 때에만 input이 클럭에 동기화된다.

![IMG_1189.jpeg](/img/user/z-Attached%20Files/IMG_1189.jpeg)

이런 경우, Load가 0이면, Input에 뭐가 들어와도 상관없기 때문에 don't care condition이 늘어나는 이점이 있다.

> cf) 우상단에 D FF 대신에 JK FF를 사용한 경우를 간략하게 그려보았다. D FF는 JK FF의 일부 기능을 제한하여 직관적인 플립플롭이지만, 이전 State 정보를 활용하기 위해서는 hold 또는 toggle 기능이 없기 때문에 추가적인 wire를 통해 가져와야 한다. 따라서 몇몇 경우에 구조가 더 복잡해 질 수 밖에 없다.

# Shift Registers
목표: 일부러 정보를 지연시킨다. 일종의 버퍼 역할을 한다고 볼 수 있다.

![4-bit shift register.png|600](/img/user/z-Attached%20Files/4-bit%20shift%20register.png)

cf) 아래의 D flip-flop이 하나의 메모리 요소에 포함된다.

![Master-Slave D flip-flop.jpeg|400](/img/user/z-Attached%20Files/Master-Slave%20D%20flip-flop.jpeg)

만약 D FF 대신에 D latch 를 쓴다면 어떨까?
격으로 버퍼를 넣어주어야 할 것이다. 그리고 클럭에 버퍼 없는 것과 있는 것이 한 세트로 플립플롭이 되겠지. 버퍼를 넣어주지 않는다면, 한 타임에 모든 래치들이 동기화되고 말 것이다. 이것은 shifting을 시키지 못하므로 쓸모가 없다.

## Serial-Transfer
Shift Register의 대표적인 사용이다. Serial이란 연속된 비트들의 나열을 의미한다. Serial 방식은 병렬적으로 비트들을 받는 것이 한번에 여러 정보를 처리할 수 있는 데 반해, 느리지만 안정적이고 회로의 복잡성이 줄어든다.

비트를 **순차적으로 처리**해야하는 경우에 유용하다.
![IMG_1182.jpeg](/img/user/z-Attached%20Files/IMG_1182.jpeg)
▲(a) Block diagram은 A에 저장된 비트를 순차적으로 B로 보내고 있다. 예컨대 1100이 이 저장된 4비트 쉬프트 레지스터라면, LSB인 0을 출력하고 B가 입력으로 받게 한다. 동시에 A의 입력으로도 받아서 A는 0110을 저장한 상태가 된다. 이렇게 4번 반복하면, A와 B는 동일한 비트를 저장한 상태가 된다.

Shift Register는 연속된 비트열을 하나씩 내보내고, 동시에 하나씩 저장하는 형태이다.

## Serial Adder

![IMG_1184.jpeg](/img/user/z-Attached%20Files/IMG_1184.jpeg)
▲Full Adder를 Shift Register로써 구현한 모습이다. JK FF를 사용했지만 D FF를 이용할 수도 있다.

## Universal Shift Register

![Pasted image 20231202185730.png](/img/user/z-Attached%20Files/Pasted%20image%2020231202185730.png)
▲ 연두색으로 네모친 부분이 처음에 나온 가장 간단한 형태의 Shift Register이다. MUX를 이용하여 몇가지 옵션을 추가한 보편적인 Shift Register이다.

----
