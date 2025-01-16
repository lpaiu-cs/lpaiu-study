---
{"dg-publish":true,"permalink":"/study/computer-network/lan/","created":"2023-12-17T23:52:48.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---


인터넷이 글로벌 네트워킹에 관한 것이라면, 이더넷은 로컬 네트워킹에 관한 것이다.
# Ethernet

최초의 이더넷 랜은 bus구조였다.
### bus
모든 노드들이 하나의 동일한 collision domain에 속한다.

### star topology (with switch)
처음에는 중심에 허브가 놓였기 때문에 충돌이 발생할 수 있었지만, 지금은 스위치가 놓인다. 이를 스위치 이더넷이라 한다.

- 포워딩을 하기 때문에 충돌이 없다.
- full-duplex
- Switching by MAC Table


# VLANs (Virtual LANs)

![Pasted image 20231215062242.png|500](/img/user/z-Attached%20Files/Pasted%20image%2020231215062242.png)

이처럼 VLAN을 쓰면 본래 물리적으로 하나의 로컬인 네트워크가 마치 두개의 스위치를 사용 중인 것처럼 가상의 논리적 로컬 두개로 분리된다.

이러한 로컬 분리는 서버와 PC가 같은 네트워크에 존재하지 않게 하는 등 보안 이슈나 효율성을 얻기 위해 행해진다.
