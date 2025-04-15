---
{"dg-publish":true,"permalink":"/== Study/Operating System/03 Event handling mechanisms/","created":"2024-11-13T20:15:40.000+09:00","updated":"2025-04-11T21:56:57.803+09:00"}
---


앞서나온 전제로, CPU가 하나일때에 대해 계속 생각해보자.
CPU에서 작동해야 하는 여러 프로그램들은 어떻게 서로에게 CPU 리소스 자리를 내어주고 차지할까?

물론, 하드웨어 리소스는 커널이 관리한다. 하지만 우리는 한발짝 더 나아가 그것에 대한 구체적인 메커니즘에 대해 고민해볼 것이다.

그 전에 운영체제는 프로그램을 **프로세스process**로 추상화한다는 사실을 알아두자. 프로그램은 사실 정적인 코드에 불과한데, 이를 동적이고 실행 가능한 논리적 단위인 프로세스로서 다룬다는 뜻이다.
>프로그램: 정적인 코드
>프로세스: 실행 중인 프로그램의 **실행 상태를 포함한** 추상 논리 객체

# Context Switching
>Context Switching: CPU가 A라는 프로세스를 중단하고 B 프로세스를 실행할 때, 레지스터, 스택 포인터, 프로그램 카운터 등 전체적인 프로세스의 실행 상태를 저장/복원하는 작업.

현재 실행중이던 프로세스가 CPU 리소스를 내어주고 다른 프로세스가 작업할 수 있게 해준다면, 컨텍스트 스위칭이 발생했다고 말할 수 있다.

(이러한 컨텍스트 스위칭이 빈번하게 발생한다면, 그 자체로 오버헤드가 커지는 문제를 일으킨다. 저장, 복원하는 과정 자체가 비용 즉, 오버헤드기 때문이다.)

# CPU event processing  techniques
컨텍스트 스위칭이 발생하기 위해서는 어떤 이벤트event가 발생해야 한다.
즉, 이 이벤트가 트리거가 되어 컨텍스트 스위칭을 발생시킨다고 이해할 수 있다. 

이벤트는 다음 두가지로 나뉜다.
>1. **Trap** (synchronous event): 현재 실행 중인 프로세스의 명령 실행 결과로 발생하는 이벤트
>2. **Interrupt** (asynchronous event): 현재 실행 중인 프로세스와 무관하게 다른 장치 또는 외부에 의해 발생하는 이벤트

## Trap processing
내가 코드를 잘못 짜서 divide by zero 이벤트가 발생하여 프로세스가 중지되었다 치자, 이것은 트랩이 발생했다고 할 수 있다.

![Screenshot 2025-04-10 at 10.37.03 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-10%20at%2010.37.03%20PM.png)

## Interrupt handling
유튜브 애플리케이션을 이용하던 중, 카카오톡 알림이 오는 상황을 생각해보자.
![Screenshot 2025-04-11 at 9.56.48 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-11%20at%209.56.48%20PM.png)
유튜브와 카카오톡은 서로 다른 프로그램이다. 따라서 카카오톡 알림을 받기 위해서는 실행 중이던 유튜브 프로세스의 상태를 저장한 후, 카카오톡 프로세스를 실행하는 context switching이 발생한다. 이런 경우 인터럽트가 발생했다고 말한다.

---
이전 글: [[== Study/Operating System/02 OS structure\|02 OS structure]]
다음 글: [[== Study/Operating System/04 CPU Scheduling\|04 CPU Scheduling]]

