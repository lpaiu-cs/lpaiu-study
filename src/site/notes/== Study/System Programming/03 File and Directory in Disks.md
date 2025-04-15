---
{"dg-publish":true,"permalink":"/== Study/System Programming/03 File and Directory in Disks/","created":"2025-04-15T11:36:21.051+09:00","updated":"2025-04-15T16:10:34.795+09:00"}
---

# How the File and Directory exists "on Disk"
파일과 디렉토리가 디스크 내부에 어떤 방식으로 존재하는 지에 대한 구체적인 구현을 알아보자.
## Contiguous Allocation
VM에서 배운 것과 객체만 달라진다.

파일이 필요할 것으로 예상되는 블록들을 연속으로 파일에 할당하는 간단한 방법이다.
inode에 적힌 위치정보: start, length
![Screenshot 2025-04-15 at 2.32.43 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-15%20at%202.32.43%20PM.png)

장점:
구현이 간단하다.
전체 파일을 한번에 읽을 때 좋은 성능을 갖는다. (물리적 거리가 떨어져 있는 것에 비해)

단점:
단편화 문제가 발생할 수 있다.
파일에 할당하는 블록의 수를 증가시키는 것이 어렵다.

## Linked List Allocation
단편화 문제 해결을 위해 고안되었다.

파일은 블록의 시작 인덱스와 끝 인덱스를 가리키고 있다.
데이터 블록 구조는 다음 블록의 포인터(인덱스)와 내용으로 이루어진다.
inode에 적힌 위치정보: start, end
![Screenshot 2025-04-15 at 3.22.22 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-15%20at%203.22.22%20PM.png)

당연하겠지만, 포인터의 사이즈는 디스크 내의 총 블록 수가 결정할 것이다.

장점:
단편화 문제가 거의 없기 때문에 디스크 공간 낭비가 적다.
파일에 할당하는 블록의 수를 증가시키기 쉽다.

단점:
어떤 파일의 랜덤 위치에 접근(random location access) 하려면 처음부터 읽어들여야 한다. -> O(n)
블록의 중간 부분이 소실되면 나머지 부분에 접근할 수 없다.

## Indexed Allocation with Linked List
random access 성능의 향상을 위해 고안되었다.

모든 블록에 포인터를 포함시키는 대신, 어떤 블록을 **index block**으로 지정하고 여러개의 데이터 블록 포인터를 담는다. 
inode에 적힌 위치정보: index block
![Pasted image 20250415153133.png](/img/user/z-Attached%20Files/Pasted%20image%2020250415153133.png)

장점:
random access의 속도 향상

단점:
가능한 파일의 최대 사이즈가 인덱스 블록에 저장 가능한 블록 수로 고정됨.
-> 여러개의 인덱스 블록사용하면 됨. 

## Multilevel Inndexed Allocation
복수의 인덱스 블록을 계층적으로 구조화하여 사이즈를쉽게 늘릴 수 있게 한다.

![Screenshot 2025-04-15 at 3.59.22 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-15%20at%203.59.22%20PM.png)

