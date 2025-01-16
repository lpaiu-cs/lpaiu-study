---
{"dg-publish":true,"permalink":"/= Study/Computer Network/Security/","created":"2023-12-17T23:52:56.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---


### 보안의 3요소
- Confidentiality(기밀성): 비인가 접근 방지

- Integrity(무결성): 메시지 위변조 방지

- Availability(가용성): 언제든 원할 때 접속가능해야 함.

# 블록 암호화

## 일방향 암호화
평문을 암호문으로 바꿀 수는 있지만, 암호문을 평문으로 바꾸는 key가 존재하지 않는 것.
Hash가 이에 해당하며, 비밀번호를 검증하는 데에 사용된다.
### Hash
m을 받아 원본 메시지 길이와 무관한 일정한 길이의 문자열 H(m)을 생성한다.

메시지의 무결성을 검사하는데에 사용될 수 있다.
1. 메시지와 해시값을 함께 보낸다.
2. 상대방은 수신된 해쉬값을 암호화하여 돌려준다.
3. 복호화한 후 보내진 해시값과 같은지 확인한다.

ex) MD5, SHA-1, SHA-2

## 양방향 암호화
평문을 암호문으로 바꾸고, 다시 암호문을 평문으로 바꿀 수 있는 구조.
일반적인 data에 대해 사용된다.

### Symmetric Key
암호화 키 = 복호화 키
ex) DES, 3DES, AES

### 비대칭키(=공개키)
암호화 키 $\ne$ 복호화 키
발신자와 수신자가 사전에 키를 공유했어야함.
-> 그리고 [[Encryption Algorithm.\|RSA]]가 등장했다. >중간자 공격에 취약 > 인증서로 보완 可

### PKI (Public Key Infrastructure)
인증서

# SSL (Secure Socket Layer)
- 데이터 암호화, 무결성을 위한 메시지 인증 수행
- Handshake Protocol 구조임.

# TLS (Transport Layer Security)
SSL의 후속 버전인 TCP 보안 프로토콜
https

# IPsec
IP Security 프로토콜로서 오직 데이터그램의 payload(세그먼트에 해당)만 암호화한다.
cf) 위 설명은 transport mode라고 하고, tunnel mode는 전체를 암호화한후 헤더를 추가한다.

# VPN (Virtual Private Network)
전용선을 이용하면 중간자 공격 등의 위험에서 자유로울 수 있다. 하지만 고가의 사설망을 사용하는 것은 쉽지 않다. 이를 논리적으로 대체하기 위해 탄생한 기술이 가상 사설망인 VPN이다.

- SSL VPN
- IPsec VPN

# Firewall

Network 방화벽 -> 침입 탐지 시스템IDS -> 침입 방지 시스템IPS -> Web 방화벽

## Stateless packet filtering
개별 패킷 검사
간단하고 빠르다

## Stateful packet filtering
외부로 나가는 요청패킷의 정보를 방화벽이 기억하고 있음.
