## TCP란?

OSI 7계층 중에서 transport 계층에 해당하는 프로토콜. 대표적인 transport layer 프로토콜에는 TCP와 UDP가 있다.
- TCP
    - 안정적이다. 신뢰도 있는 전송. date 전달 100% 보장한다. 100이거나 0이거나.
    - In-order delivery. 송신자가 보낸 순서대로 수신자가 데이터를 받는다.
    - connection을 데이터 전송 전에 미리 세팅한다.
    - 혼잡 제어(congestion control)를 지원한다. 즉, 네트워크가 현재 너무 바쁘다면 이를 조절해서 패킷을 느리게 보낸다.
    - 흐름 제어(flow control)를 지원한다. 즉, 수신자가 바쁘다면 송신자가 느리게 보낸다.

- UDP
    - 신뢰도 없는 전송. TCP처럼 100% 전달된 것인지 확인할 수 없다.
    - 순서도 보장하지 않는다.
    - 사전에 connection 설정 과정 불필요
    - 혼잡 제어(congestion control), 흐름 제어(flow control) 없음!

<img width="500" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/0dd2b3b5-ceb3-4c81-a567-44160a3f8d8e">

## TCP 3-way handshake

TCP의 특징이 통신하기 전에 사전 연결 작업을 진행한다는 것!
TCP에서 connection을 만드는데 쓰이는 3-way handshake에 대해 알아봅시다.

<img width="588" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/789a1351-6c72-4497-92e1-ffb79d5dc9b4">

1. 서버와 connection을 만들고 싶은 클라이언트가 `TCP SYN` 메시지를 서버에 보냅니다. 이때 seq 숫자를 담아서 보냅니다.
2. 서버에서 SYN 메시지를 받으면 `TCP SYNACK` 메시지를 보냅니다. 이 `TCP SYNACK`는 두 가지 숫자를 담아서 보냅니다.
첫번째는 클라이언트가 서버에서 보낸 `TCP SYN`에 대한 `ACK`입니다. 이때 `ACKnum`은 클라이언트가 보낸 숫자+1입니다.
두번째는 서버에서 또 클라이언트에게 `SYN` 메세지를 보내게 됩니다. 서버에서도 클라이언트가 했던 것처럼 숫자를 클라이언트에게 보냅니다.
3. 클라이언트는 서버에서 보내준 `SYNACK`를 받게 됩니다. 이때부터 클라이언트 상태는 connection이 성립된 것으로 봅니다. -> 클라이언트는 서버가 내 요청을 잘 받았구나! connection이 잘 되었구나! 판단합니다.
클라이언트는 서버가 보낸 `SYN` 메시지에 대한 `ACK`를 한번 더 보냅니다. -> 서버는 이 `ACK`를 받고 클라이언트가 연결이 잘 되었구나! 판단합니다.
이때 `ACK`만 보내는 게 아니라 데이터도 포함시켜서 전송할 수 있습니다. 클라이언트는 본인이 보낸 `SYN`에 대한 `ACK`를 받은 시점부터 연결이 성립되었다고 보기 때문!

클라이언트와 서버가 각각 한번씩 SYN 메시지와 ACK 메시지를 주고 받으면서 서로의 연결을 확인합니다.

## TCP 4-way handshake

3-way hankshake가 TCP 연결을 할 때 사용했던 방법이라면 TCP 연결을 해지할 때 사용하는 4-way handshake를 알아봅시다.

<img width="437" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/f31f3b1f-1be0-43d6-9b32-192c2b5e4e12">

1. 일단 클라이언트가 connection을 닫고싶다는 신호를 서버에 보냅니다. 이때 `FIN` 값을 서버에 보냅니다. 그리고 클라이언트는 `FIN_WAIT_1` 즉, 대기상태로 들어갑니다. 
일방적으로 닫아버릴 수는 없습니다.. 서버가 클라이언트가 닫고싶어한다는 패킷을 받기 못했을 수도 있기 때문!
이때 클라이언트는 데이터를 보낼 수는 없지만 받을 수는 있습니다.
2. 서버는 FIN을 받으면 이에 대한 ACK를 보냅니다. 이때 ACK 숫자는 FIN 숫자+1입니다. 서버는 `CLOSE_WAIT`, 역시 대기상태가 됩니다. connection을 바로 닫을 수는 없습니다. 
아직 서버에서는 데이터를 보내고 있는 중일 수 있기 때문!
3. 서버는 보내고 있던 데이터가 있으면 다 보낸 다음, 클라이언트 쪽에 이제 진짜 닫겠다는 의미의 FIN을 또 보냅니다. 이때 클라이언트가 했던 것처럼 FIN 숫자를 담아 보냅니다. 그리고 `LAST_ACK` 상태로 바뀝니다. 마지막으로 ACK를 받아야 한다는 의미입니다.
4. 클라이언트는 서버에 마지막으로 잘 받았다는 의미의 ACK를 보냅니다. 이때 ACK 숫자는 서버에서 보낸 숫자+1입니다. 이 ACK를 받으면 서버는 연결을 종료합니다. 
클라이언트는 ACK를 보내고 충분한 시간을 기다린 뒤에 연결을 종료합니다. 왜냐면 만약 클라이언트가 보낸 마지막 ACK가 안 갔다면 서버도 timeout 걸어놨기에 또 FIN 메시지 보냅니다. 그런데 바로 클라이언트 연결을 닫아버렸다면 서버에서 보낸거 못받고 그럼 또 서버에서 보내고...

## 마지막 정리


<img width="594" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/bc2b5969-2410-4e4c-8a2d-6ce6a09cdfcd">

