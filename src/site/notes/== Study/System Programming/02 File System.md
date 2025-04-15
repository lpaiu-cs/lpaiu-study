---
{"dg-publish":true,"permalink":"/== Study/System Programming/02 File System/","created":"2025-04-12T22:01:25.361+09:00","updated":"2025-04-15T14:02:10.671+09:00"}
---

# File Abstraction
저장 장치(raw storage)라는 것은 기실 바이트들이 연속적으로 놓여있는 공간일 뿐이다.
그런데 사람이나 프로그램이 그런 바이트 덩어리를 직접 다루기는 어렵기 때문에, **"파일"** 이라는 단위로 나눠서 이름을 붙이고, 여러 파일을 **"디렉토리"** 라는 폴더 구조로 관리한다.
즉, 파일과 디렉토리라는 논리적 개념이 바로 파일 추상화이다.

## Mapping
추상화 기법을 통해 얻은 논리적 공간이 의미를 가지려면 실제 공간에 매핑되어야 한다. (More precisely, group homomorphism - 군 동형 사상)

OS에서 배웠던 [[== Study/Operating System/05 Memory\|메모리]] 개념을 떠올려 보자. 이때 가상(논리) 메모리 주소를 물리 메모리 공간으로 매핑하는 주소 변환하는 과정에 대해 배웠다. 이는 논리 공간과 실제 공간 간의 매핑의 다른 예시이다.
![Screenshot 2025-04-14 at 2.32.26 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-14%20at%202.32.26%20PM.png)

즉, 가상 메모리가 **'가상 주소 (page) → 페이지 테이블 → 물리 메모리 (frame)'** 의 매핑 구조를 가졌듯이,
파일 시스템은 **'파일 이름 (file) → 파일 테이블 → 디스크 (blocks)'** 의 매핑 구조를 갖는다.

![Screenshot 2025-04-14 at 2.38.42 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-14%20at%202.38.42%20PM.png)
유의할 점은, 페이지와 프레임은 일대일 매핑 관계지만, 파일과 블록은 하나의 파일에 여러 블록이 연결될 수 있음으로써, 파일의 크기가 자유롭게 이뤄진다. (윈도우에서 폴더 속성을 들어가 보면, 디스크 할당 크기와 파일 크기가 다를 때가 있었는데 그 이유를 알것 같다.)

이러한 추상화를 통해 프로세스(유저)는 데이터를 더 쉽게 다룰 수 있게 된다.

## + Privileged Instruction / MMIO와의 관계
앞서 배운 개념과 혼동할 수 있기 때문에 정확히 구분하고 넘어가려 한다.
CPU는 반드시 메모리(RAM)에서만 데이터를 읽을 수 있다.
따라서 프로세스가 파일에 접근한다는 것은 결국 내부적으로 CPU를 통해 RAM에 저장된 데이터에 접근하는 것이다. 이전 시간에 다뤘던 부분은 IO 접근의 구체적인 메커니즘을 CPU가 바라보는 메모리 관점에서 배웠던 것이다.

# File Descriptor
파일 접근 시에 커널은 파일 디스크립터를 통해 접근한다.
파일 디스크립터는 작은 정수로 정의되는데, open file table의 인덱스를 가리킨다.
'작은' 정수인 이유는 테이블이 프로세스마다 정의되기 때문이다.

테이블이 프로세스마다 정의될 수 있는 이유는 무엇일까?
그것은 파일을 열 때, 프로세스가 이용되기 때문이다.
예컨대 텍스트 파일은 텍스트 뷰어로 열거나 코드 뷰어로 열거나 할 수 있다. 또한 이미지 파일이나 비디오 파일 등 다양한 파일 들은 프로세스를 통해 열린다.
이런식으로 구분함으로써 프로세스간의 프로텍트 도메인을 지킬 수 있다.

실제적인 구현에서 프로세스는 다음처럼 시스템 콜을 한다.
```c
int fd = open("foo.txt", O_CREAT | O_RDONLY | O_WRONLY | O_TRUNC);
```
여기서 fd가 파일 디스크립터이며, open()은 인자로 파일의 이름과 flag를 받는다.
코드상의 각 flag 의미는 생성, 읽기, 쓰기, 초기화 이다.
$$\begin{flalign*}
&\text{Table: }\hspace{6cm}\text{File Descriptor} \rightarrow \text{FCB(inode)}&
\end{flalign*}$$

# Linking a File from Multiple Directories

## Tree-Structured Directories

디렉토리의 구조는 그림과 같이 [[== Study/Data Structure/Binary Tree#Tree\|트리]]구조로 이루어진다.
![Pasted image 20250415114830.png](/img/user/z-Attached%20Files/Pasted%20image%2020250415114830.png)

트리구조의 한계상 특정 디렉토리와 파일이 현재 디렉토리와 먼 경우 번거롭게 돌아가야 하는 경우가 발생한다.
아래 그림과 같이 다른 디렉토리나 파일에 쉽게 연결될 수 있는 방법은 없을까? (엄밀히 디렉토리 또한 파일이다.)
![Pasted image 20250415115901.png](/img/user/z-Attached%20Files/Pasted%20image%2020250415115901.png)
그것이 바로 링킹이다.
## Linking
파일 시스템의 논리적 구조에 대해 조금 더 깊게 들어가보자.
디렉토리 내부에서 파일이름은 디렉토리 엔트리를 가리킨다. 디렉토리엔트리는 파일 디스크립터를 통해 inode와 매핑되었다. 그리고 inode는 데이터 블록과 매핑된다. 링킹은 파일의 바로가기 같은 것인데, 스토리지 내부 데이터의 위치는 inode에 저장되는 것이 기본 구조이므로 data와 바로 연결되는건 아니다. 대신 나머지 두개인, 디렉토리 엔트리와 inode 이 둘 중 누구와 연결시키냐에 따라 전자는 Soft link, 후자는 Hard link로 나뉜다. 아래 도식도를 보면 이해가 쉽다.

![Screenshot 2025-04-14 at 10.58.02 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-14%20at%2010.58.02%20PM.png)

### 파일 삭제하기
하나 뿐인 디렉토리 엔트리(alias)를 삭제하면, 파일 데이터가 삭제된다.
만약 링킹에 의해 복수의 엔트리가 하나의 파일 데이터를 가리키고 있다면, inode와 연결된 하나의 엔트리를 삭제한다고 해서 데이터가 삭제되지는 않는다. 즉, 해당 파일의 inode와 연결된 엔트리가 하나라도 있으면 데이터는 지워지지 않는 것이다.
이는 사실 데이터의 삭제란 데이터에 접근할 방법을 잃어버리는 것을 의미하기 때문이다. data 자체는 스토리지 어딘가 여전히 남아있고, inode 또한 스토리지 어딘가에 여전히 있을 것이다. 그런데, 엔트리가 없어서 inode에 접근할 수 없으면 해당 데이터가 삭제된다고 볼 수 있기 때문이다.


# Mount

## Partition
파티션은 일종의 logical segmentation이다.
하나의 디스크 내부를 몇개의 디렉토리씩 묶어서 파티션을 만들거나, 복수의 디스크에 나눠져 있는 하나의 디렉토리를 모아서 하나의 파티션으로 구분한다. 구체적인 구현에 따라 양상은 갈리게된다.

## Mount
그림과 같이 디바이스에 있는 파티션을 기존 디렉토리에 연결하는 행위를 마운트라 한다.

![Screenshot 2025-04-15 at 1.27.10 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-15%20at%201.27.10%20PM.png)

# Protection on Files
이것은 저장된 데이터에 대한 접근을 허용하지 않는 것을 의미한다.
데이터는 파일 단위로 추상화되어 저장되어 있는데, 각각의 파일마다 접근을 제어한다.

### owenership
(1) user, (2) group (3) other
오너에 따라 접근 권한을 다르게 설정할 수 있다.

### permission
권한의 타입은 3가지이다.
- r: read
- w: write
- x: execute

![Screenshot 2025-04-15 at 1.52.14 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-15%20at%201.52.14%20PM.png)

---
이전글: [[== Study/System Programming/01 IO Device\|01 IO Device]]
다음글: 