---
{"dg-publish":true,"permalink":"/== Study/System Programming/01 IO Device/","created":"2025-04-12T15:15:15.581+09:00","updated":"2025-04-15T14:02:02.616+09:00"}
---

IO 디바이스는 컴퓨터가 주변과 상호작용할 수 있게 하는 것이다. (커널을 통해서)
즉, 데이터를 input, output 하는 것이다.
# How to access IO devices
CPU는 IO 디바이스에서 데이터를 읽어(read)오거나 IO 디바이스에 데이터를 쓴다(write).

당연히 CPU와 IO 디바이스 간의 인터페이스가 존재한다.
이를 **디바이스 레지스터**라 부르고, 전형적으로 3가지가 있다.
- **Status Reg**
- **Command Reg**
- **Data Reg**

이들 디바이스 레지스터는 CPU 내부에 존재하는 레지스터가 아닌, 외부 디바이스에 존재하는 레지스터로, CPU가 해당 디바이스와 소통하기 위한 인터페이스를 의미한다. (주의해야 할 점은 이름은 레지스터지만 일종의 관례상 표현으로, Data Reg 같은 경우 데이터 버퍼로서 작동하며, 수백 수천 바이트의 크기를 지닐 수도 있다.)

CPU가 인터페이스를 이용해 IO 디바이스와 통신하는 예시를 들어보자.

## Polling
Data Reg에 데이터를 저장한다.
Command Reg에 IO에게 전달할 명령어를 저장한다.
Status Reg 값을 BUSY로 만들고, BUSY가 끝날 때까지 기다린다. (IO 디바이스가 Data와 Command Reg를 참조하여 작업을 하는 동안 기다리다가, 디바이스가 작업을 끝내면 Device Reg 값을 IDLE로 변경.)
![Screenshot 2025-04-12 at 6.13.05 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-12%20at%206.13.05%20PM.png)
이 방법은 CPU가 IO 디바이스의 작업이 끝날 때까지 기다리는 것이기 때문에 시간 낭비가 크다는 문제가 있다.

## Interrupt
대안으로 CPU가 기다리지 않고 다른 작업을 하다가 인터럽트를 통해 IO 작업의 종료를 알림받는 방법이 있다.
다만, 다른 작업을 수행하기 위해 프로세스를 변경하는 과정에서 컨텍스트 스위칭에 따른 오버헤드가 발생한다.

![Pasted image 20250412181541.png](/img/user/z-Attached%20Files/Pasted%20image%2020250412181541.png)
많은 경우 인터럽트가 좋겠지만, 아주 특수한 시스템에서 IO가 매우 빠른 경우 컨텍스트 스위칭에 드는 비용이 더 클 수도 있다.

# Direct Memory Access (DMA)
한편, CPU가 너무 큰 데이터를 IO 디바이스에 보내는 경우에는 메인 메모리에서 디바이스로 데이터를 카피하는 데에 시간이 너무 오래걸린다는 문제도 발생할 수 있다.

![Screenshot 2025-04-12 at 6.20.17 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-12%20at%206.20.17%20PM.png)
이 경우 CPU는 낭비된다.
내가 하기에는 낭비고, 남이 대신 해주면 좋을 텐데 싶으니 IO 작업을 대신 해줄 유닛을 생각해 볼 수 있다. 이렇게 내게 부담스러운 일을 하청 주는걸 오프로드offload라 부른다.

그게 바로 **DMA controller** 이다.
![Pasted image 20250412182431.png](/img/user/z-Attached%20Files/Pasted%20image%2020250412182431.png)

# Memory-Mapped IO (MMIO)
가상 메모리는 CPU가 직접 접근할 수 있는 공간이지만, IO 디바이스는 그렇지 않다.
$$\text{CPU} ↔ \text{Virtual Memory} ↔ \text{Physical Memory} ↔ \text{I/O Devices}$$
따라서 CPU가 특정 IO 디바이스에 접근하기 위해서는 IO 디바이스를 메모리 공간에 매핑해야 한다. 이를 위해 초기에는 디바이스 접근에 **특권명령어(privileged instruction)** 를 사용했다. 이 명령어는 CPU가 DMA를 명시적으로 제어하여 특정 데이터를 PM에 로드하도록 명령하는 것이다. 하지만, 이 방법은 구현이 복잡하며 성능 면에서 문제가 생기기 때문에 외부 디바이스들을 메모리처럼 다루려는 설계 디자인이 탄생했다. 그것이 **Memory-Mapped IO (MMIO)** 이다. MMIO는 디바이스들을 **PM 주소 공간**에 미리 매핑시켜 놓는다. 이 경우 CPU는 DMA를 직접 제어할 필요가 없기 때문에 간단해진다.
참고로 데이터 로드(복사)를 미리 해놓는게 아니라, 주소만 매핑하는 것임에 유의한다.
이로써 CPU가 메모리 접근 명령만으로 장치를 제어할 수 있게 되었다.

이에 따라 디바이스 레지스터의 위치 또한 물리 메모리 공간에 미리 할당됨으로써 CPU와 DMA가 이 디바이스 레지스터에 접근할 수 있게 되는 것이다.

![Screenshot 2025-04-12 at 6.58.05 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-12%20at%206.58.05%20PM.png)

과거 32비트에서는 이로인한 RAM 손실 문제가 존재했지만, 64비트 시스템에서는 이런 문제가 발생하지 않는다. 왜냐하면, 64비트의 물리 메모리 공간이 실제 메모리 RAM에 모두 매핑될 수 없기 때문에, 물리 메모리 공간에서 평생 절대 쓰이지 않는 주소들로 매핑하면 되기 때문이다.

---
다음글: [[== Study/System Programming/02 File System\|02 File System]]