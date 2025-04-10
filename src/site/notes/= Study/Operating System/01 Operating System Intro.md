---
{"dg-publish":true,"permalink":"/= Study/Operating System/01 Operating System Intro/","created":"2024-11-13T20:14:00.000+09:00","updated":"2025-04-10T22:01:03.419+09:00"}
---

다음 글: [[= Study/Operating System/02 OS structure\|02 OS structure]]
# Operating System

>An **operating system** (**OS**) is system software that manages computer hardware and software resources, and provides common services for computer programs. - wikipedia에서 발췌

![Screenshot 2025-04-10 at 7.33.22 PM.png|300](/img/user/z-Attached%20Files/Screenshot%202025-04-10%20at%207.33.22%20PM.png)
이미지로 보면 좀 더 이해하기 쉽다.
한마디로, 하드웨어와 유저 애플리케이션 사이를 중재하는 역할의 프로그램이라고 보면 된다.

# Why OS?
OS가 필요한 이유는 간단하다. 하드웨어 종류가 너무 많고 복잡하기 때문이다. (CPU, GPU, RAM, HDD, SDD, NIC, etc.) 그래서 애플리케이션이 이러한 하드웨어를 쉽게 사용하기 위한 인터페이스가 바로 OS이다.

또한 하드웨어 리소스를 효과적으로 사용하기 위해서도 필요하다. 여러 프로그램이 동시에 실행되려고 할 때 가용한 하드웨어 자원은 누구에게 우선적으로 돌아가야 할까? 각각에 적절하게 분배하는 방법은 뭘까? 등에 대한 고민을 OS가 한다. 만약, 각각의 프로그램들이 이러한 고민을 해야한다면, 매우 비효율적이고 충돌이 나기 쉬우며 서로 자원을 차지하려는 경쟁이 충돌을 야기할 수도 있다.

또한 보안을 위해서도 중요하다. 만약, 자원 분배의 권한을 OS가 관리하지 않으면 악성 프로그램이 앞서 언급한 경쟁을 의도적으로 발생시키고, 시스템 자원을 독차지 하여 시스템을 마비시킬 수도 있다.

# Definition
이로써 운영체제의 목적을 이해했을 것이다. 그렇다면, OS의 역할은 어떻게 정의할 수 있을까?

## Abstraction
> Provide **"abstraction"** for easy and efficient use of hardware.

운영체제는 추상화를 제공한다.
추상화는 복잡한 하드웨어 자원을 단순화하여 제공하는 것을 의미한다.

## Policy
> Determine **"polices"** of the mechanisms.

운영체제는 정책을 실현한다.
정책은 가용한 자원을 어떻게 사용하고 배분하는 지에 대한 규칙이다.

## Mechansim
> Implement **"mechanisms"** to realize policies.

메커니즘은 정책을 실제로 구현하는 방법이다.
정책은 추상적인 규칙이고, 그 규칙을 정확히 어떤 방식으로 구현하는 지는 메커니즘이라고 부른다.





