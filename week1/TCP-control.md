## TCP의 특성

handshake 아티클에서 다루기도 했는데 TCP의 특징은 흐름제어와 혼잡제어를 지원한다는 것
- 혼잡제어(congestion control): 네트워크 내에 패킷 수가 너무 많아지지 않도록 조절하는 것. 송신자와 네트워크 사이 이야기
- 흐름제어(flow control): 송신자가 수신자 처리속도보다 더 빠르게 데이터를 보내지 못하게 하는 것. 송신자와 수신자 사이 이야기

## 혼잡제어(congestion control)

### 혼잡이란?

네트워크에서 말하는 혼잡(congestion)이란, 네트워크가 감당할 수 없을만큼 너무 많은 데이터를 너무 빠르게 보내는 것.

## 어떻게 제어할 것인가
1. Slow Start
2. Fast Retransmit
3. Fast Recovery

AIMD 방식: Additive Increase Multiplicative Decrease
<img width="773" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/d1ceeebb-a96f-4f5c-b4c2-8a1eeee4b35e">

패킷을 보내고 문제가 없다면 윈도우의 크기를 1씩 증가시켜가면서 전송한다. 그러다가 전송에 실패하면 윈도우 크기를 반으로 줄인다.
선형적으로 1씩 증가시키면서 윈도우를 늘리기 때문에 윈도우 크기를 올리는데 속도가 오래 걸린다.

그래서 나온 방법이 Slow Start 방식

일단 connection이 시작되면 윈도우 크기를 점차 증가시킨다. -> 많은 패킷을 보낸다. 전송률 높아진다. 이때 2배씩 증가하며 보낸다.
처음 cwnd=1이고 ACK 받으면 cwnd=2, 4, 8... 식으로 ACK 받은 숫자의 2배씩 늘어난다. 굉장히 빨리 증가합니다.
이렇게 빠른 속도로 윈도우 크기를 늘리다가 다음과 같은 상황이 발생했을 때 윈도우 크기를 조절합니다.

1. 타임아웃(timeout)
  - 네트워크가 혼잡하다고 판단
  - cwnd 다시 1
  - 윈도우 1로 갔다가 2배씩 늘려가면서 빠르게 증가하다가 ssthresh 도달하면 선형적으로 증가.

2. ACK 3번 중복
  - 사전에 네트워크 혼잡을 막기 위해서 3번 ACK가 중복되어 올 때마다 발생
  - cwnd를 반으로 줄이고 선형적으로 증가.
  - 모든 TCP가 이를 적용하는 것은 아님. TCP Reno, New Reno는 ACK 3번 중복될 때 cwnd를 반으로 줄이지만 TCP Tahoe는 이 경우도 cwnd 1로 set

`ssthresh`란? timeout 났을 때의 cwnd의 절반 값. 임계점으로 이 임계점 전까지는 윈도우 크기를 지수적으로 증가시키고 이후에는 선형적으로 증가시키겠다는 의미. 임계치를 두는 이유는 충돌을 사전에 피하기 위함

<img width="725" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/dc018882-9150-48b5-8c7b-e4a0811d25a2">

## 흐름제어(flow control)
흐름제어의 핵심은 송신자가 보내는 데이터량을 수신자에 따라서 조절해야 한다. sliding window 방식을 사용한다.

<img width="620" alt="image" src="https://github.com/do-sopt-cs-study/CS-seungyeon/assets/49530253/a34efedf-6365-44ba-aaa5-94b2dbb78f15">

이 방식에서는 송신자가 보냈지만 아직 ACK를 받지 못해서 수신자가 정확히 받았는지 파악되지 않은 패킷의 수를 구한다.
윈도우 수만큼 패킷을 모두 전송한 다음, ACK 받으면 그때 또 하나의 패킷을 보내는 식으로 제어된다.
이때 송신자 윈도우 크기는 수신자 윈도우 크기와 같게 된다. 송신자는 수신자의 윈도우 크기를 TCP connection을 할 때 알 수 있게 된다.




