---
{"dg-publish":true,"permalink":"/== Study/System Programming/04 In-kernel File System Implementation/","created":"2025-04-19T01:07:12.000+09:00","updated":"2025-04-20T09:31:34.000+09:00"}
---

# on-disk structures
지난 시간에 배웠듯 디스크는 파일 시스템의 메타데이터 블록과 데이터 블록으로 나눠졌었다.
![Screenshot 2025-04-19 at 7.02.57 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-19%20at%207.02.57%20PM.png)

여기서 on-disk structure가 바로 파일 시스템에 대한 메타데이터 블록을 의미한다.

이 메타데이터 블록에 대해 좀 더 자세히 살펴보면 다음과 같다.
![Screenshot 2025-04-19 at 7.04.45 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-19%20at%207.04.45%20PM.png)

- Boot 블록: 부팅시에 필요한 내용을 담고 있다.
- Super 블록: 파일 시스템에 대한 여러 메타데이터를 담고 있는 핵심 블록이다. inode array의 위치 또한 저장하고 있다.
- inode/data bitmap: inode block과 data block에 대해 블록이 사용 중인지 아닌지에 대한 정보를 담고 있다.
- inode array (table): FCB의 집합이다.

Super 블록의 경우 정기적으로(e.g. every 8192 blocks) 복제되어 저장되며, I/O에 걸리는 시간을 줄이기 위해 일부 블록들은 메모리에 캐싱된다.

# Virtual File System (VFS)
파일 시스템은 하나뿐이 아니다. 다양한 목적과 구조의 파일 시스템이 존재한다.
이러한 파일 시스템을 하나의 통일된 인터페이스로 추상화하여 다루기 위해 VFS를 사용한다.

좀 더 자세히 말하자면, 커널에 존재하는 여러 FS는 모두 공통함수(e.g. read(), write())을 제공해야 한다.
하지만 실제로 읽고 쓰는 방법은 FS마다 완전히 다르다.
따라서 어떤 FS인지에 따라 함수가 동적으로 바꿔치기된다.(바인딩)

아래는 계층적(layer)으로 디바이스에 접근하는 루틴을 보여준다.
![Screenshot 2025-04-19 at 7.11.38 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-19%20at%207.11.38%20PM.png)
위 그림 마지막에 다양한 IO 디바이스 FAT, ext4, nfs에 따라 추상화된 함수인 read가 구체적인 구현에서 read_fat, read_ext4, read_nfs 등의 함수로 바꿔치기 되는 것을 알 수 있다.

유저(프로세스)가 디바이스에 접근하는 과정을 나타내면 다음과 같다.
(1) 유저는 시스템콜 인터페이스를 통해 High-level file system인 VFS로 진입한다.
(2) VFS에서 실행되는 함수는 동적으로 바꿔치기 되어 로우 시스템 함수가 되어 로우 시스템으로 진입한다.
(3) Low-level file system은 블록 번호, inode 정보 등을 통해 디바이스에 접근한다.
![Screenshot 2025-04-19 at 7.14.21 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-19%20at%207.14.21%20PM.png)

# File operation
02에서 유저가 파일에 접근하는 과정은 [[== Study/System Programming/02 File System#File Descriptor\|파일 디스크립터]]를 '프로세스마다 정의된 파일 테이블'에 매칭시켜서 FCB(inode) 주소를 알아내는 식으로 이뤄짐을 배웠었다.

이것을 좀 더 자세히 들여다 보면, 파일 테이블은 두 개다.
(1) Per-process open-file table
(2) System-wide open-file table
여기서 (1)은 이미 알고있었지만, (2)는 무엇일까?
(2)는 커널에 단 하나만 정의되는 것으로, 모든 오픈 파일에 대해 관리한다. 접근시간을 줄이기 위해 메모리에 FCB를 캐싱하고, 이때 중복으로 가져오는 것을 막기 위해 테이블링해서 관리한다.

유저가 파일을 open() 하는 것에 대한 플로우를 보자.
![Screenshot 2025-04-20 at 12.18.10 AM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-20%20at%2012.18.10%20AM.png)
1. 엔트리가 (2)에 존재하는지 확인.
2. 만약 없다면 해당하는 FCB 엔트리를 디스크에서 가져와서 (2)에 저장.
3. (1)에 엔트리를 추가하고 (2)의 FCB를 가리키게 연결.
4. 파일 디스크립터 반환.

이러한 과정을 거쳐 fd값이 테이블에 매핑된다.

# Caching in file systems
빈번하게 접근하는 데이터는 메모리에 캐싱하는게 국룰이다.

## Caching metadata
1) Cached superblock
슈퍼 블록은 캐싱해야한다.
2) Directory entry cach(dentry)
파일의 경로를 빠르게 찾기 위해 캐싱한다.
3) Cached inode(vnode)
위 플로우 그림에 나오는 메모리 공간에 위치한 FCB가 바로 접근 시간을 줄이기 위해 캐싱된 것이다.

## Caching data blocks
일반적으로 1~10%의 물리 메모리 공간은 데이터 블록 캐싱에 할당된다.
예외적으로 캐싱이 불필요한 경우: 동영상, 오디오 등 DBMS는 일반적으로 한번 재생되고 말기 때문에 temporal locality를 띄지 않는다. 따라서 캐싱을 제한한다.

아래 그림은 유저(프로세스)가 데이터 블록에 접근하는 양상이다.
캐싱 여부를 체크하는 경우와 캐시를 우회bypass하는 경우로 나뉜다. 후자는 DBMS 등의 경우.
![Screenshot 2025-04-20 at 12.56.43 AM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-20%20at%2012.56.43%20AM.png)
여기서 동기화가 되는 과정 두가지가 그림에 나타난다.
하나는 새로운 블록으로 교체될 때, 디스크에 write back한다.
다른 하나는 주기적으로 write back한다.

이는 수정과 같은 일을 할때 디스크에 일일히 수정 내용을 업데이트 하는게 아니라, 메모리에 수정 내용을 적어두고, 추후 모아서 수정하는 전략인 것이다. 아래 그림은 wirte() 상황에서의 자세한 플로우이다.
![Screenshot 2025-04-20 at 1.00.42 AM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-20%20at%201.00.42%20AM.png)
여기서 kworker는 주기적으로 dirty bit(파일 수정 여부 확인 플래그)를 체크하여 디스크와의 동기화를 수행한다.

# File read/write operations
다시 오퍼레이션으로 돌아가자.
유저가 파일을 읽거나 쓸때, 데이터는 디스크에서 바로 유저 공간으로 이동하지 않고, 커널 메모리(캐시)를 경유한다.
$$\text{Disk → Kernel cache → Process buffer}$$
이처럼 우회적으로 동작하는 이유는 (1) 보안. 유저가 직접 디스크에 접근하지 못하게 하며, (2) 캐싱을 통해 성능을 최적화 하는 데에 목표가 있다.
![Screenshot 2025-04-20 at 1.12.35 AM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-20%20at%201.12.35%20AM.png)

여기에는 필연적으로 복사라는 오버헤드가 발생한다.
물리 [[== Study/Operating System/05 Memory\|메모리]] 공간(RAM)에 캐싱된 데이터 블록은 VM 상에서 커널 메모리 공간에 매핑되어 있다. 이것을 유저 메모리 공간에서 직접 참조할 수 없기 때문이다. (many to one 불가: 커널 보안 침해가 발생하게 되므로.)

# Memory-mapped file
위와 같은 복사 오버헤드를 없앨 수 있는 방법이 아예 없는 것은 아니다.
시스템콜 mmap()은 커널이 보유한 캐시를 가상 유저 공간에 매핑한다.

![Screenshot 2025-04-20 at 1.34.10 AM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-20%20at%201.34.10%20AM.png)


---
이전글: [[== Study/System Programming/03 File and Directory in Disks\|03 File and Directory in Disks]]
다음글: [[== Study/System Programming/05 Crash and recovery\|05 Crash and recovery]]