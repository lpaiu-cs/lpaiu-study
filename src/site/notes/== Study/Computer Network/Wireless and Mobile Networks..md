---
{"dg-publish":true,"permalink":"/== Study/Computer Network/Wireless and Mobile Networks./","created":"2023-12-17T23:53:02.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
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

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/study/computer-network/l2-data-link-layer/#csma-carrier-sense-multiple-access" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### CSMA (Carrier Sense Multiple Access)
- 말하기 전에 듣는다.
**노드는 전송하기 전에 채널을 듣는다.** 이를 **캐리어 감지carrier sensing**라 하며, 다른 노드가 채널을 사용 중일 시 랜덤한 시간 후에 감지를 반복한다. 채널을 사용할 수 있게 되면, 즉시 패킷(프레임)을 전송한다.

- 다른 사람이 동시에 말하기 시작하면 말을 멈춘다.
**노드는 전송하면서 동시에 채널을 듣는다.** 이를 **충돌 검출collision detection**이라 하며, 다른 노드가 채널을 사용 중일 시 랜덤한 시간 후에 충돌 검출을 반복한다.

- CSMA/CD (/Collision Detect)  <- 유선 충돌 탐지
1. 캐리어 감지 후 채널 사용 중일 시 잼 신호 브로드캐스트.
2. CRC값을 검사 후 이상이 있을 경우 폐기 (collision detection)
3. 전송을 못했을 경우, Back-off 알고리즘에 따라 랜덤한 시간 후 재시도.

- CSMA/CA (/Collision Avoid)   <- 무선 충돌 회피
1. Carrier Sense: 통신 전 채널 듣기(탐색)
2. Multiple Access: Data의 흐름 감지
3. Collision Avoidance: Data 흐름 x -> 예비신호를 통해 충돌 회피


</div></div>

CSMA/CA

# WIPS 무선보안
Authentication, Encryption

와이파이 연결 후 대칭키 유도
WPA3 handshake (8단원에서 했죠?)


# Home network, visited network
- Home network: 처음에 접속한 네트워크
- Visited network: 연결된 채로 이동해서 만나는 네트워크\
mobile handover -> 끊김 x