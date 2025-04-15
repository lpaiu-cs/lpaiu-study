---
{"dg-publish":true,"permalink":"/== Study/Logic Design/Sequential Logic/","created":"2023-12-18T02:43:46.000+09:00","updated":"2025-01-14T15:33:45.000+09:00"}
---


### Synchronous
동기화: 명확한 시간 단위로 행동이 발생하는 것. 즉, [[== Study/Logic Design/Flip-Flop\|Clock]]이 있다.

논리 순서가 꼬일리는 없지만 Asynchronous에 비해 속도는 느리다.

# Sequential Logic

이전까지 배워온 로직은 Combinational Logic으로 현재의 input에 의해서만 output이 결정되는 로직이다.

하지만 Sequential Logic(순차논리)은 과거의 input이 현재의 output에 영향을 미칠 수 있는 로직이다. 과거에 들어온 input 즉, 정보를 **기억**할 수 있다는 것이다.


![Pasted image 20231113004849.png|600](/img/user/z-Attached%20Files/Pasted%20image%2020231113004849.png)
▲ Sequential Circuit은 Combinational과 Memory 요소의 합이라고 할 수 있다.

이러한 메모리 소자를 구현하는 로직으로 [[== Study/Logic Design/Latch\|Latch]]와 [[== Study/Logic Design/Flip-Flop\|Flip-Flop]]가 있으며, 이들이 모여 일종의 메모리 군집인 [[== Study/Logic Design/Registers\|Register]]를 이룬다.