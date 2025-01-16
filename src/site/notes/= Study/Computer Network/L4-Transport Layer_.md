---
{"dg-publish":true,"permalink":"/= Study/Computer Network/L4-Transport Layer_/","created":"2023-12-14T17:07:14.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---


[[L5-Application Layer.\|애플리케이션 계층]]에서 애플리케이션은 소켓을 통해 네트워크 연결을 제공받았고, 또한 신뢰성과 보안성, 속도에 관한 서비스를 제공받았다. 트랜스포트 계층은 어떻게 이러한 서비스들을 지원할 수 있을까?

# Logical Communication for Process

먼저, 프로세스와 연결된 소켓으로 패킷을 전달하는 트랜스포트 계층은 프로세스간의 논리적 통신을 제공한다고 말할 수 있다. 반면, [[L3-Network Layer.\|네트워크 계층]]은 IP주소를 이용하여 호스트간의 논리적 통신을 제공한다.

# Transport-layer Multiplexing / Demultiplexing

네트워크 계층이 제공하는 **호스트 대 호스트 통신을 프로세스 대 프로세스 통신으로 확장하는 것**이 트랜스포트 계층의 가장 기본적인 역할이다.

이 때, 소켓에서 전달된 데이터를 네트워크로 보내는 경우 다중화Multiplexing이라 부른다. 여러 데이터 스트림을 하나의 네트워크를 통해 보내기 때문이다. 반면, 네트워크에서 전달받은 데이터를 소켓으로 전달하는 경우 역다중화Demultiplexing이라 부른다.

## Header / Payload
## encapsulation / decapsulation
Multiplexing의 경우 Segment를 Datagram으로 캡슐화하고,
Demultiplexing의 경우 Segment를 Message로 디캡슐화한다.

# UDP / TCP

# 어떻게 두 객체의 신뢰성을 보장하는가?

# 어떻게 혼잡 제어와 복구를 하는가?
