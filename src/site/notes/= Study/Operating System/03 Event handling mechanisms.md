---
{"dg-publish":true,"permalink":"/= Study/Operating System/03 Event handling mechanisms/","created":"2024-11-13T20:15:40.000+09:00","updated":"2025-04-10T22:01:43.516+09:00"}
---

이전 글: [[= Study/Operating System/02 OS structure\|02 OS structure]]
다음 글: [[= Study/Operating System/04 System Call 함수 정의하기\|04 System Call 함수 정의하기]]

앞서나온 전제로, CPU가 하나일때에 대해 생각해보자.
CPU에서 작동해야 하는 여러 프로그램들은 어떻게 서로에게 CPU 리소스 자리를 내어주고 가져갈까?

물론, 커널이 한다고 했었다. 하지만 우리는 한발짝 더 나아가 그것에 대한 구체적인 메커니즘에 대해 고민해볼 것이다.

그 전에 운영체제는 프로그램을 프로세스process로 추상화한다는 사실을 알아두자. 프로그램은 사실 정적인 코드에 불과한데, 이를 동적이고 실행 가능한 논리적 단위인 프로세스로서 다룬다는 뜻이다.
>프로그램: 정적인 코드
>프로세스: 실행 중인 프로그램의 **실행 상태를 포함한** 추상 논리 객체

# Context Switching
>Context Switching: CPU가 A라는 프로세스를 중단하고 B 프로세스를 실행할 때, 레지스터, 스택 포인터, 프로그램 카운터 등 전체적인 프로세스의 실행 상태를 저장/복원하는 작업.






