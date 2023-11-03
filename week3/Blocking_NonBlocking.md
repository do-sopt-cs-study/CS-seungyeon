## I/O

Input/Output을 의미한다. 어떤 디바이스를 통해 입력과 출력이 이뤄지는 모든 작업을 I/O라고 한다.
- 네트워크를 통해서 다른 서버로 데이터를 전송하는 경우
- 다른 서버로부터 데이터를 전송 받는 경우
- 콘솔에 출력
- 파일 입출력

I/O는 어플리케이션 성능에 영향을 많이 미친다. I/O가 발생할 때 프로그램은 CPU를 사용하지 못하고 대기시간에 들어가게 된다. I/O가 많아지면 애플리케이션이 연산을 할 때 대기시간이 많아지고 애플리케이션 처리 속도도 느려짐.

## Blocking VS Non-Blocking

- Blocking
    - 자신의 작업을 진행하다가 다른 주체의 작업이 시작되면 다른 작업이 끝날 때까지 기다렸다가 자신의 작업을 시작하는 것.

- Non-Blocking
    - 다른 주체의 작업에 관련없이 자신의 작업을 하는 것

다른 주체가 작업할 때 자신의 제어권이 있는지 없는지!

## Blocking I/O

- I/O 처리되는 동안 사용자 프로세스가 작업하던 것을 중단하고 I/O 끝날 때까지 기다림.
- CPU 제어권을 커널에서 가지고 있어서 I/O 완료 전까지 다른 작업을 할 수 없다.

1. 사용자 프로세스가 작업을 하다가 I/O 작업이 필요하면 시스템콜을 요청해서 I/O를 요청함
2. Kernel이 I/O 작업을 하는 동안 사용자 프로세스는 block이 되어 다른 작업을 하지 못한다.
3. I/O 작업이 끝난 뒤에 다시 사용자 프로세스 작업 시작

## Non-Blocking I/O

- I/O 작업이 완료될 때까지 프로세스가 기다리지 않고 계속 작업을 진행.
- CPU 제어권을 사용자 프로세스로 바로 넘겨주기 때문에 사용자 프로세스가 I/O 작업 진행 중에 다른 작업을 할 수 있다.

1. 사용자 프로세스가 작업을 하다가 I/O 작업이 필요하면 시스템콜을 요청해서 I/O를 요청함
2. 커널의 I/O 작업 완료 여부와 상관없이 일단 응답해줌. 커널이 시스템콜 받자마자 CPU 제어권이 다시 사용자 프로세스에게 넘어감. 즉, 사용자 프로세스는 I/O가 끝나지 않았음에도 다른 작업을 할 수 있다.
3. 사용자 프로세스는 자신의 작업을 수행하다가 중간중간 시스템콜로 커널 I/O 진행사항을 확인.
4. 완료되었으면 I/O 작업을 완료한다.

## Synchronous VS Asynchronous

- Synchronous
    - 작업을 동시에 수행하거나 동시에 끝나거나, 끝나는 동시에 시작함.
    - Blocking과 다르게 다른 일을 할 수 있다. 다른 일이 끝나자마자 받아서 바로 처리하는 게 포인트

- Asynchronous
    - 시작과 종료가 일치하지 않으며, 끝나는 동시에 시작을 하지 않음.
    - 끝나자마자 일을 처리하지 않음. 처리를 하지 않을 수도 있다.

동기와 비동기는 순서와 결과에 관심이 있는지 아닌지로 판단 가능.

## Blocking/Non-Blocking VS Synchronous/Asynchronous

- Blocking & Sync
    - 다른 작업 진행되는 동안 작업 안함. `Blocking`
    - 다른 작업이 결과 반환하면 바로 작업 시작. `Sync`
    - 다른 작업이 끝나면 CPU 제어권과 결과를 같이 받아서 실행.
    - 시스템콜이 발생할 때마다 thread를 생성하기 때문에 I/O 요청이 많아지면 context switching 비용이 많이 발생함.

<img width="500" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/80fa5e18-275c-439c-8152-2d53ef16af7d">

    
- Blocking & Async
    - 다른 작업 진행되는 동안 작업 안함. `Blocking`
    - 결과 바로 처리하지 않아도 됨. `Async`
  
<img width="500" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/ebce046a-9a19-4ef4-a4d7-2ebc2e3b5818">

- Non-Blocking & Sync
    - 다른 작업이 있어도 자신의 제어권으로 일을 계속 함. `Non-Blocking`
    - 동기는 결과에 관심이 있는 것. 사용자 프로세스가 커널 I/O 작업이 완료되었는지 지속적으로 확인. `Sync`
    - Blocking & Sync와 차이가 거의 없다.
 
<img width="500" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/69716b7b-0111-45e3-8d39-fc428a7211d1">


- Non-Blocking & Async
    - 다른 작업이 있어도 자신의 제어권으로 일을 계속 함. `Non-Blocking`
    - 결과 바로 처리하지 않아도 됨. 사용자 프로세스가 지속적으로 커널에게 I/O 작업 완료 여부를 물어보지 않아도 됨. `Async`

<img width="500" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/1383ffc9-211e-4a15-89e5-46c4af0e30b7">


<img width="700" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/eddc3604-1222-4e5e-9d10-3a9773eb435c">
<img width="700" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/fa9cf5e5-724b-40d5-bac7-3730bdd08a69">
