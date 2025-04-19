---
{"dg-publish":true,"permalink":"/== Study/System Programming/03 File and Directory in Disks/","created":"2025-04-15T11:36:21.051+09:00","updated":"2025-04-19T19:12:34.324+09:00"}
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

# Directory Implementation

디렉토리 또한 FCB를 갖는다.
디렉토리 내부 엔트리 들은 어떤 방식으로 존재할까?
앞서 파일이 디스크 내부에 어떤 방식으로 존재할 수 있는지 다양한 방법을 배웠듯,
디렉토리 내부 엔트리들도 다양한 구조로 연결된다.
(1) linked list
-> 이미 잘 알듯이 특정한 파일 접근에 오래 걸린다. O(n)
(2) hash table
-> 해시 테이블은 O(1) 접근 시간으로 매우 빠르게 접근할 수 있다.
세부적으로, 파일 이름을 통해 해시를 만들고, 해시 충돌은 부분적으로 linked list를 이용한다.
-> 하지만 파일간 정렬이 되어있지 않아, 별도의 sort를 해야 한다.
(3) tree
-> B-tree 구조를 이용하므로, 자동 정렬된다. 트리구조 이므로 접근시간도 짧다.

# Directory Entry
앞서 파일 이름이 디렉토리 엔트리를 가리킨다 했는데, 엄밀히 말하면, 엔트리에 포함되는 개념이다.
inode-based 파일 시스템에서의 디렉토리 엔트리 구성은 다음 그림과 같이 inode table에 매핑되는 inode number와 파일 이름으로 구성된다.

![Screenshot 2025-04-19 at 12.42.40 AM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-19%20at%2012.42.40%20AM.png)

하지만, 초창기에 등장한 MS DOS는 inode개념이 완전히 정립되지 않았기 때문에 디렉토리 **엔트리와 FCB가 분리되어 있지 않다.** (Linux에서 FCB가 inode를 가리키는 것과 달리)
그것이 아래의 FAT 그림이다.

![Pasted image 20250419004544.png](/img/user/z-Attached%20Files/Pasted%20image%2020250419004544.png)

# inode-based File Systems Flow
지금까지 배운 것을 토대로 inode-based에서 파일에 접근하는 흐름을 한번 살펴보자.

1. super block: 루트 디렉토리를 찾기 위한 파일 시스템의 메타데이타라고 할 수 있다. (루트의 inode number는 보통 2)
2. inode table은 array로 구현된다(inode\[2] = 루트의 데이터 블록위치). 데이터 블록에 접근한 다음, 데이터 블록에서 하위 파일/디렉토리 엔트리 정보를 얻음.
3. 찾고자 하는 디렉토리/파일 이름에 해당하는 inode 번호 알아냄 -> 데이터 블록 접근
4. ...

![Pasted image 20250419005747.png|500](/img/user/z-Attached%20Files/Pasted%20image%2020250419005747.png)

---
이전글: [[== Study/System Programming/02 File System\|02 File System]]
다음글: [[== Study/System Programming/04 In-kernel File System Implementation\|04 In-kernel File System Implementation]]