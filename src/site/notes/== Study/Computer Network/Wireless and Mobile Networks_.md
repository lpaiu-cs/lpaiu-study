---
{"dg-publish":true,"permalink":"/== Study/Computer Network/Wireless and Mobile Networks_/","created":"2023-12-17T23:53:02.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---


두 가지 중요한 과제가 있다.
- wireless
- mobility

802.11

### infrastructure mode
유선 기지국과 연결되어 네트워크를 형성
### ad hoc mode
기지국과 같은 base station 없이 인접한 node들과의 무선 연결을 통해 네트워크를 형성

---
이상적인 네트워크: mesh net
그러나 우리가 실질적으로 사용하는 것은 star형 인터넷 (비용이슈)

무선 링크의 특징:
유선 링크에 비해 신호세기 약함.


802.11 (wifi)
base station = access point (AP)
- BSS (Basic Service Set)

SSID: AP의 이름

### passive scanning
AP가 beacon frame을  브로드캐스트로 수시로 뿌림: 우리 폰에 wifi 목록이 뜨는 이유
-> 비콘에는 암호화 방식 등의 정보가 포함되므로, 보안이 중요한 경우에는 패시브를 끈다.
### active scanning
호스트가 Probe Request 패킷을 브로드캐스트로 수시로 뿌림: 한번 접속하면, 인근만 가도 자동으로 접속되는 이유

# 무선환경에서의 충돌
![[L2-DataLink Layer.#CSMA (Carrier Sense Multiple Access)\|L2-DataLink Layer.#CSMA (Carrier Sense Multiple Access)]]
CSMA/CA

# WIPS 무선보안
Authentication, Encryption

와이파이 연결 후 대칭키 유도
WPA3 handshake (8단원에서 했죠?)


# Home network, visited network
- Home network: 처음에 접속한 네트워크
- Visited network: 연결된 채로 이동해서 만나는 네트워크\
mobile handover -> 끊김 x