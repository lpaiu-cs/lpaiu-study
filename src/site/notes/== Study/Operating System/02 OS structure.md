---
{"dg-publish":true,"permalink":"/== Study/Operating System/02 OS structure/","created":"2024-11-13T20:15:32.000+09:00","updated":"2025-04-11T00:33:10.000+09:00"}
---

# Design OS
운영체제를 디자인 하기 위해 고려해야 할 목표는 여러가지가 있다.
>Fairness, Real-time, High performance(higher throughput, low latency, high utilization), Scalability, Stability/reliability/robustness, Security/integrity, Usability, Compatibility, Energy consumption(power, heat), etc.

위와같이 매우 많은 디자인 목표가 있다. 이 모든 지표에서 동시에 좋은 퍼포먼스를 내는 것은 불가능하다. 그렇기 때문에 운영체제는 다양하고, 발전소, 군용무기, 항공기 등 특수한 분야에서 사용되는 운영체제가 따로 존재한다.
우리가 배울 것은 평균적으로 다방면에서 잘 작동하는 범용 운영체제이다.

운영체제를 디자인할 때는 먼저 **정책**을 결정하고, 구체적인 **메커니즘**(실현 방안)을 구현한다.

# OS structure
운영체제의 구조에는 대표적으로 두가지가 존재한다.
Layering과 Modularity이다.

## Layering
Layering은 위계적 구조가 특징이다. 각각의 레이어들은 상하로 인접한 레이어와만 통신할 수 있다.
![Screenshot 2025-04-10 at 8.20.27 PM.png|400](/img/user/z-Attached%20Files/Screenshot%202025-04-10%20at%208.20.27%20PM.png)
이러한 구조는 설계가 단순하고, 분석과 검증이 쉽다.
하지만 오버헤드overhead가 늘어날 수 있다. (overhead: 정보를 전달하고 통신하는 데에 필요한 비용)
왜냐하면, 아주 간단한 작업이더라도 반드시 중간 레이어를 거쳐서 전달되기 때문이다.

예컨대, '하드웨어 -> 커널 -> 시스템콜 -> 유저 프로그램' 이런 식이다.

## Modularity
Modularity는 비위계적 구조로 이루어져, 독립된 모듈들이 나란히 존재한다.
각 모듈들은 다른 모듈들과 유연하게 상호작용 할 수 있다.
즉, 레이어간의 통신이 상하 통신만 있는 데에 반해, 더 다양한 통신이 가능한 모듈화는 그 모든 통신 인터페이스를 정하고, 규약을 정해야 하기 때문에 구현이 매우 복잡하다. 하지만, 오버헤드가 적다는 이점을 갖는다.
-> 우리가 쓰는 운영체제는 모두 모듈러리티이다.

![Screenshot 2025-04-10 at 8.32.39 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-10%20at%208.32.39%20PM.png)

# Kernel
일각에서 운영체제와 커널을 동의어로 보기도 한다.
어쨌거나 커널은 운영체제의 가장 핵심적인 부분으로, 하드웨어와 유저 애플리케이션 간의 통신을 책임진다.
![Pasted image 20250410203659.png|400](/img/user/z-Attached%20Files/Pasted%20image%2020250410203659.png)
# CPU execution mode
원칙: 모든 프로그램은 CPU에서 실행된다.
그리고, 유저 애플리케이션과 마찬가지로 커널 또한 프로그램이다.

CPU는 하나 이상의 실행 모드를 갖는데, 그것은 시스템을 보호하기 위해서이다.
여기서는 커널 모드와 유저 모드 두가지만을 가정한다. 또한 하나의 CPU를 가정한다.

생각해보자, 모든 프로그램은 CPU에서만 작동하는데, 커널도 프로그램이고, 유저 앱도 프로그램이다.
그런데 CPU는 하나기 때문에 커널과 유저 앱이 동시에 작동하지는 않고 번갈아가며 작동할 것이다.
이때 커널이 동작하지 않고, 커널의 감독 없이 유저 앱이 작동하는 동안 시스템에 마음대로 접근하여 문제를 일으킬 수 있을 것이다. 이것을 어떻게 방지할 수 있을까?

바로 복수의 CPU 실행 모드를 갖는 것이다. 시스템에 접근하는 권한은 **커널 모드**에서만 허용되고, **유저 모드**에서는 허용되지 않는다. 커널 모드는 커널이 작동 중일 때를 의미하고, 유저 모드는 유저 앱이 작동 중일 때를 의미한다. 즉, 권한의 분리를 통해 해결할 수 있다.

> Kernel mode: run with **full privileges**
> User mode: **lower privileged** execution mode compared to kernel mode

두 실행 모드간에 스위칭을 **"mode switch"** 라고 한다.

# System call
그렇다면, 유저 앱이 하드웨어에 접근하려면 어떻게 할까?
물론 커널을 통해야 하고, 당연히 커널 모드에서만 하드웨어 접근이 가능하다.
따라서 커널에 **요청**을 보내고, 커널이 요청을 받아서 **수행**해야 한다. 그 다음 커널이 앱에 **응답**한다.

이 요청을 시스템 콜system call이라 부른다.
![Screenshot 2025-04-10 at 9.03.34 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-10%20at%209.03.34%20PM.png)

위 그림과 같은 과정을 통해 유저 앱은 이미 짜여져 있는 커널 코드를 통해 하드웨어와 상호작용 할 수 있다.
이때, High-level API는 커널 코드를 조금 더 유저 앱 개발자에게 친숙하게 만든 것이다. 그런 의미에서 시스템 콜의 커널 코드는 대략 Low-level API라고 볼 수 있을 것이다.

![Screenshot 2025-04-10 at 9.06.44 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-10%20at%209.06.44%20PM.png)

---
이전 글: [[== Study/Operating System/01 Operating System Intro\|01 Operating System Intro]]
다음 글: [[== Study/Operating System/03 Event handling mechanisms\|03 Event handling mechanisms]]

#
Monlithic kernel vs Microkernel
운영체제 기능을 어디에 배치하느냐에 따른 철학 차이 (커널 내부 vs 외부)

#
Virtualization, hypervisor