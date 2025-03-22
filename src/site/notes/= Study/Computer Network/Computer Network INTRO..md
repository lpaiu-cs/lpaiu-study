---
{"dg-publish":true,"permalink":"/= Study/Computer Network/Computer Network INTRO./","created":"2023-12-14T17:34:58.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---


# 기본적인 용어 정리

**인터넷Internet**이란 무엇인가.
작은 단위의 네트워크들을 연결하여 더 큰 네트워크를 구성하던 것이, 발전을 거듭하여 연결방식의 표준을 이룩한 것이다.

**네트워크Network**란 무엇인가.
**시스템System**들을 **전송매체Transmission Media**를 통해 연결한 것이다.

시스템이란 무엇인가.
통신의 대상이 될 수 있는 것이다.
여기서, 데이터를 주고받을 수 있다면, **노드Node**라 불릴  수 있다. 이는 인터넷에 연결된 시스템을 의미하기도 한다.
노드 중에서 계산 즉, 컴퓨팅 기능이 있는 경우, **호스트Host**라 불릴 수 있다. Service를 주고 받을 수 있음을 의미한다.
호스트에는 2가지가 있는데, 바로 **클라이언트Client**와 **서버Server**이다. 클라는 Service를 이용하는 쪽을 의미하고, 서버는 Service를 제공하는 쪽을 의미한다. 즉, 이는 상대적인 관점으로 절대적으로 고정되어 있지 않다. 언제든 서버와 클라는 뒤바뀔 수 있다.

# Network Architecture

네트워크 구조는 기본적으로 하위 계층이 상위 계층에 서비스를 제공하는 방식으로 계층화된다. 상위 계층은 일반적으로 하위 계층에서의 작동에 영향을 끼칠 수 없고, 그래서도 안된다. 그것이 계층화의 의의 그 자체이기 때문이다. 대신 상위 계층은 인터페이스를 통해 서비스를 제공받을 때, 어떤 서비스를 선택할지 어느정도 결정할 수 있다.

### 상위의 하위 서비스 의존성:
기본적으로 상위 계층은 하위에서의 서비스에 의존하게된다.
예컨대,

### [[= Study/Computer Network/L5-Application Layer.\|L5-Application Layer.]]

### [[= Study/Computer Network/L4-Transport Layer.\|L4-Transport Layer.]]
### [[= Study/Computer Network/L3-Network Layer.\|L3-Network Layer.]]
### [[= Study/Computer Network/L2-DataLink Layer.\|L2-DataLink Layer.]]
### L1-Physical Layer

## Protocol vs Interface
![IMG_1177.jpeg](/img/user/z-Attached%20Files/IMG_1177.jpeg)

# Access Network
종단 시스템end system이 처음으로 만나는 라우터까지의 네트워크

Access Network를 구성하는 방식에는 여러가지가 있다. 목적, 사용하는 회선, 시대에 따라 갖가지 방식이 사용되기 때문이다.

### DSL
![IMG_1178.jpeg](/img/user/z-Attached%20Files/IMG_1178.jpeg)
### 케이블 인터넷 접속
![IMG_1179.jpeg](/img/user/z-Attached%20Files/IMG_1179.jpeg)
### FTTH
![IMG_1180.jpeg](/img/user/z-Attached%20Files/IMG_1180.jpeg)
### 위성링크
시골지역 등, 외지에서 사용될 수 있다.
### LAN (Local Area Network) / 3G, LTE
![IMG_1181.jpeg](/img/user/z-Attached%20Files/IMG_1181.jpeg)

이중에서 우리가 주로 따져볼 내용은 LAN이다. 이후에 무선인터넷에서 3G, LTE에 대해서도 약간 다룬다.

# Network of Networks