---
{"dg-publish":true,"permalink":"/study/logic-design/memory-and-programmable-logic/","created":"2023-12-18T20:52:30.000+09:00","updated":"2025-01-14T15:33:45.000+09:00"}
---


SAM (Sequential Acess Memory)
RAM (Random Acess Memory)
Static RAM
Dynamic RAM
Volatile
Nonvolatile -> HDD, SSD
ROM (Read Only Memory)

---

PLD (Programmable Logic Device)
PLA (Programmable Logic Array)
PAL (Programmable Array Logic)
FPGA (Field Programmable Gate Array)

---

# Memory

![IMG_1205.jpeg](/img/user/z-Attached%20Files/IMG_1205.jpeg)
▲ 표기하기를, $2^k \times n$ Memory라 부른다.

word는 Memory contest의 1개 묶음을 의미한다.
메모리 주소 또한 인덱싱을 해야 하기 때문에 불러줄 이름이 필요하다.
2^k 개의 주소를 불러주려면 k개의 주소 표현 bit 수가 필요하다.

## Operation
### Write operation
1. 주소 접근
2. data 대기
3. 쓰기
### Read operation
1. 주소 접근
2. 읽기

# RAM (Random Acess Memory)

![IMG_1206.jpeg](/img/user/z-Attached%20Files/IMG_1206.jpeg)
▲ Memory Cell, 메모리의 기본 구성요소가 된다.


![IMG_1207.jpeg](/img/user/z-Attached%20Files/IMG_1207.jpeg)
▲ 보는 바와 같이 k개(k=2)의 Address input과 n개(n=4)의 word bit들로 이루어진 RAM이다. decorder를 이용해서 주소 접근을 구현하였다.

# ROM (Read Only Memory)

롬은 영구적으로 데이터를 저장해 두는, 비휘발성 메모리이다.
일반적으로, 수정되어선 안되는 데이터를 저장해두는 장소라고 보면 된다.


![IMG_1208.jpeg](/img/user/z-Attached%20Files/IMG_1208.jpeg)
▲ ROM은 input data를 할 필요가 없기 때문에, 주소를 읽는 k input과 n output 만이 존재한다.

![IMG_1209.jpeg](/img/user/z-Attached%20Files/IMG_1209.jpeg)▲ ROM의 세부적인 내부 구조이다. 그물망 처럼 생긴 이게 무엇인고 할 수 있는데, 여기서 필요한 점들을 골라서 퓨징fusing 하는 것이다. 즉, 필요한 부분을 이어붙이면, Input(주소의 index값)에 따라 저장된 데이터를 output한다.

또한 롬 또한 램과 마찬가지로 decoder를 통해 주소 인덱스를 구현하고 있다.


![IMG_1210.jpeg](/img/user/z-Attached%20Files/IMG_1210.jpeg)
▲ 이렇게 퓨징을 한다. 한번 퓨징하고 나면, 런타임 중에 바뀌지 않는다. 단 하나만의 State를 갖는다고 말할 수 있으며, 따라서 **Combinational Logic**이다.

## Types of ROMs
### PROM (Programmable ROM)
ROM의 가장 기본형. 한번만 프로그래밍할 수 있는 영구적 ROM으로 지금껏 본 유형.
### EPROM (Erasable PROM)
자외선을 이용해서 지운후, 다시 프로그래밍 가능한 ROM.
### EEPROM (Electrically EPROM)
전기 신호를 이용해서 지운후, 다시 프로그래밍 가능한 ROM.

# PAL (Programmable Array Logic)
사실상 Programmable AND Logic이라 외우는게 맞다.
여기서 programmable하다는 것은, 퓨징을 통해 사용자가 직접 프로그래밍 가능하다는 뜻이다. 위에서 본 PROM의 경우 OR gates가 programmable했음을 알 수 있다. 반면, AND gates의 경우 decorder로 구현되어 있기에 고정되어있다.

그런데, PAL은 반대로 AND gates를 프로그래밍 가능하도록 해놓고, OR gates를 고정시켜놓은 것이다.

# PLA (Programmable Logic Array)
이름이 존나 헷갈리니 주의. 이것은 AND와 OR가 모두 프로그래밍할 수 있다.

![IMG_1211.jpeg|600](/img/user/z-Attached%20Files/IMG_1211.jpeg)
▲ 지금까지의 설명을 정리한 것.

