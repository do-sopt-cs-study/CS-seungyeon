## Process란?

- 실행 중인 프로그램
- 프로세스의 문맥(context)이란 프로세스의 현재 상태를 나타내는데 필요한 모든 요소
    - CPU 수행 상태를 나타내는 하드웨어 문맥
        - Program Counter
        - 각종 register
    - 프로세스의 주소 공간
        - code, data, stack
    - 프로세스 관련 커널 자료구조
        - PCB(Process Control Block)
        - Kernel stack

## 프로세스의 상태

프로세스는 상태를 가지고 이 상태가 변경되며 수행된다.

- Running
    - 프로세스가 CPU를 가지고 instruction을 수행 중인 상태
- Ready
    - CPU를 기다리고 있는 상태. 디스크에서 필요한 데이터들 메모리에까지 다 올라와있는데 단순히 CPU만 없어서 못 수행하고 있는 경우
- Suspended
    - 외부적인 이유로 프로세스의 수행이 정지된 상태. 프로세스가 통째로 디스크에 swap out 된다.
- New
    - 프로세스 생성 중

Blocked와 Suspended의 차이는 다음과 같다.
- Blocked: 자신이 요청한 event가 만족되면 Ready
- Suspended: 외부에서 resume 해줘야 Active

## PCB(Process Control Block)

- 운영체제가 각 프로세스를 관리하기 위해 프로세스당 유지하는 정보
    - OS가 관리상 사용하는 정보
        - Process state
        - Process ID
        - scheduling 정보, 우선순위
    - CPU 수행 관련 하드웨어 값
        - Program counter, register 
    - 메모리 관련 정보
        - Code, data, stack의 위치 정보
    - 파일 관련 정보

## 문맥 교환

- CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정
- CPU가 다른 프로세스에게 넘어갈 때 운영체제는
    - CPU가 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
    - CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어옴

## Thread

- 스레드란 프로세스 내부에 CPU 수행 단위가 여러 개 있는 경우
- 프로세스가 하나 주어지면 주소공간이 프로세스마다 만들어진다.
    - 같은 일을 하는 프로세스가 여러 개라면 주소공간을 공유하도록 하는게 효율적인데 이를 스레드라고 한다.
- 프로세스는 하나만 띄워놓고, PC(Program Counter, CPU가 코드의 어느 부분을 실행하고 있는가)만 여러 개 둔다. 프로세스 하나에 CPU 수행단위만 여러 개 두고 있는 것을 스레드라고 한다.
- 스레드는 최대한 프로세스 하나 내에서 공유할 수 있는 건 공유한다.
    - CPU 수행에 관련된 정보만 별도로 가지고 있음

- Thread는 다음의 것들을 독립적으로 가진다.
    - program counter
    - register set
    - stack space

- Thread는 다음의 것들을 공유한다.
    - code section
    - data section
    - OS resources


## Thread의 구현 방식

- Kernel Threads
    - 프로세스 안에 여러 개의 스레드를 가지고 있다는 걸 운영체제가 알고 있다.
    - 하나의 스레드에서 다른 스레드로 CPU 넘어갈 때 커널이 스케줄링 해서 넘겨줌

- User Threads
    - 프로세스 안에 스레드가 여러 개 있다는 걸 운영체제는 모른다.
    - 유저 프로그램이 알아서 스레드를 여러 개 관리한다.
    - 프로세스 본인이 내부에서 CPU 수행단위를 여러 개 두며 관리하는 것.















  








