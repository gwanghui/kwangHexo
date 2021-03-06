---
title: network-tcp-basic
date: 2020-04-07 08:49:59
tags: 
- TCP

categories:
- Network
- TCP
---

## TCP Overview
### 점대점(Point-to-Point) : 단일 송/수신자간의 통신   ex) 일대일 통신, Unicast 전송

- Unicast : 고유 주소로 식별된 하나의 네트워크 목적지에 메시지를 전송하는 방식
- Broadcast : 송신 가능한 모든 목적지에 동일한 데이터를 전송
- Multicast : 특별한 주소 지정 방식을 통해 정해진 여러 목적지로 데이터를 전송
  호스트가 무수히 많은 경우, Unicast로 데이터를 전송하면, 각각의 네트워크 연결마다 호스트의 컴퓨팅 리소스(자원)을 소비할 뿐 아니라 각각 다른 네트워크 대역폭을 필요로 하기 때문에 전송 비용이 많이 든다는 단점이 있다.

### 파이프라인(Pipeline) : TCP의 혼잡 제어 및 흐름 제어가 윈도우 크기(Window size)를 결정한다.

- Window size : ACK받지 않은 데이터 중 최대 송신가능한 바이트 수, 수신자가 한 번에 버퍼링할 수 있는 최대 데이터 크기
- Sliding Window : 두 개의 네트워크 호스트 간의 패킷의 흐름을 제어하기 위한 방법으로, TCP 프로토콜은 데이터의 전달을 보증해야 하므로 패킷 하나 하나가 정상적으로 전달되었음을 알리는 확인 신호(Acknowledgement, ACK)를 수신자가 송신자에게 보내야 한다.
    패킷에 오류가 생겼을 경우, 송신측에서 해당 패킷을 재전송해야 하는데 이 때 Sliding Window가 Window Size(메모리 버퍼의 일정 영역)에 포함되는 모든 패킷을 전송하고, 패킷의 전달이 확인되는 대로 이 Window를 옆으로 옮김(Sliding)으로써 그 다음 패킷들을 전송할 준비를 한다.
- 전이중성 데이터(Full-duplex Data) : 같은 연결 상에서 양방향 데이터 흐름

 

### 최대 세그먼트 크기 (Maximum Segment Size, MSS)

- 헤더를 제외하고 TCP가 실을 수 있는 최대 데이터 크기
    기본값은 IPv4 -> 536 Byte, IPv6 -> 1220 Byte이다. 
    최대 전송 단위(MTU)에 의해 값이 결정되며, MSS값은 헤더의 MSS 옵션 필드에 저장된다.
    연결에 참여하는 두 장비가 서로 다른 MSS값을 갖을 수도 있다.
 

- 최대 전송 단위(Maximum Transmission Unit, MTU)
    데이터 링크(2계층) 또는 네트워크(3계층)에서 하나의 프레임 또는 패킷에 담아 운반할 수 있는 헤더를 포함한 최대 데이터 크기
    최소 권고값은 2계층 기준 1500Byte이며, 3계층 기준 IPv4는 MSS 크기에서 40byte(IP헤더+TCP헤더)를 추가한 576Byte이고, IPv6는 1280Byte이다.
    실제로는 최대 65,646바이트 범위까지 생성가능하다.

- MTU가 IP 기반의 정보인 반면, Maximum Segment Size는 TCP 기반의 정보이다.
- TCP는 연결지향형으로 제어 메시지들의 교환(Handshaking)이 이루어진다. 핸드쉐이킹을 통해 데이터 교환 전에 송신자 및 수신자의 상태를 초기화한다.

### TCP Segment
![](/images/network/tcp/tcp_segment.png)

- Source port number : 출발지 포트 번호
- Destination port number : 도착지 포트 번호
- Sequence number : 순서번호(Seq#)로, 세그먼트에서 첫 번째 바이트의 바이트 스트림 번호값이 저장된다.
- Response number : 확인응답(누적된 ACK)번호로, 상대방으로부터 받아야할 다음 바이트의 순서번호값이 저장된다.
- Header length : 헤더의 길이로, Option이 없으면 헤더값에 5개의 비트값이 들어간다. 그러나 일반적으로 32비트이다.
- Reservation : 나중에 다시 설명
- Control flag 
- Window : 수신측의 윈도우 크기(Window size)로, 받을 수 있는 최대 데이터 크기값이 저장되어 있다.
- TCP Checksum :체크섬값이란 세그먼트의 합에 1의 보수를 취한 값을 말하며, 수신측에서 체크섬과 세그먼트의 실제 데이터를 더한 결과를 통해 오류를 검출한다.
- Emergency Data point : 생략
- Option : 변수의 길이로 설명은 생략.
- Data : 헤더를 제외한 실제 세그먼트에 들어있는 데이터의 크기로 MSS에 의해 제한되어진다.

#### FLAG 순서

- 구체적으로 flag순서는 URG, PSH, RST, SYN, FIN이고 각각 1비트로 표현되는 값이 저장된다. 
- 각각 1비트로 TCP 세그먼트 필드 안에 cONTROL BIT 또는 FLAG BIT 로 정의 되어 있다.
    +-------+-------+------+-------+-------+------+
    | URG | ACK | PSH | RST | SYN | FIN |
    +-------+-------+------+-------+-------+------+

- SYN(Synchronization:동기화) - S : 연결 요청 플래그
    TCP 에서 세션을 성립할 때  가장먼저 보내는 패킷, 시퀀스 번호를 임의적으로 설정하여 세션을 연결하는 데에 사용되며 초기에 시퀀스 번호를 보내게 된다.
- ACK(Acknowledgement) - Ack : 응답
    상대방으로부터 패킷을 받았다는 걸 알려주는 패킷, 다른 플래그와 같이 출력되는 경우도 있습니다.
    받는 사람이 보낸 사람 시퀀스 번호에 TCP 계층에서 길이 또는 데이터 양을 더한 것과 같은 ACK를 보냅니다.(일반적으로 +1 하여 보냄) 
    ACK 응답을 통해 보낸 패킷에 대한 성공, 실패를 판단하여 재전송 하거나 다음 패킷을 전송한다.
- RST(Reset) - R : 제 연결 종료
    재설정(Reset)을 하는 과정이며 양방향에서 동시에 일어나는 중단 작업이다. 비 정상적인 세션 연결 끊기에 해당한다. 
    이 패킷을 보내는 곳이 현재 접속하고 있는 곳과 즉시 연결을 끊고자 할 때 사용한다.
- PSH(Push) - P : 밀어넣기
    TELNET 과 같은 상호작용이 중요한 프로토콜의 경우 빠른 응답이 중요한데, 이 때 받은 데이터를 즉시 목적지인 OSI 7 Layer 의 Application 계층으로 전송하도록 하는 FLAG. 
    대화형 트랙픽에 사용되는 것으로 버퍼가 채워지기를 기다리지 않고 데이터를 전달한다. 데이터는 버퍼링 없이 바로 위 계층이 아닌 7 계층의 응용프로그램으로 바로 전달한다.
- URG(Urgent) - U : 긴급 데이터
    Urgent pointer 유효한 것인지를 나타낸다. Urgent pointer란 전송하는 데이터 중에서 긴급히 전당해야 할 내용이 있을 경우에 사용한다. 
    긴급한 데이터는 다른 데이터에 비해 우선순위가 높아야 한다. 
    EX) ping 명령어 실행 도중 Ctrl+c 입력
- FIN(Finish) - F : 연결 종료 요청
    세션 연결을 종료시킬 때 사용되며 더이상 전송할 데이터가 없음을 나타낸다.
- Placeholder             
    패킷의 플래그에 SYN, FINISH, RESET, PUSH등의 플래그가 설정 되어 있지 않은 경우 이 플래그가 세팅된다. 이 플래그는 ACK플래그와 함께 사용되는 경우도 있다.


### TCP Round Trip Time, Timeout
- TCP는 신뢰적인 데이터 전송(RDT)처럼 세그먼트의 손실을 발견하기 위해 타임아웃/재전송 메커니즘을 사용한다. 이 때, 송신측에서 데이터를 전송한 후, 
  ACK받기까지 걸린 시간을 RTT(Rount Trip Time)이라 표현한다.
- 타임아웃(timeout) 주기는 RTT보다 길어야 한다. 너무 짧으면 타임아웃이 자주 발생하여 세그먼트의 불필요한 재전송이 발생하므로 링크를 비효율적으로 사용하게 된다. 
  반대로 너무 길게 되면, 세그먼트의 손실에 대한 느린 대응으로 회복이 비효율적이게 된다.
- SampleRTT : 세그먼트가 송신된 시간부터 ACK 받기까지 측정된 시간으로, 재전송한 세그먼트는 무시한다. 값은 네트워크 부하에 따라 가변적이다.
- 현재 SampleRTT 값이 아닌 최근의 값들의 평균값으로 추정한다.
- 추정RTT : EstimatedRTT = (1-a) * EstimatedRTT + a * SampleRTT
    일반적으로 a=0.125이고, 지수적 가중 이동 평균(EWMA)방식으로 과거 샘플들의 영향이 지수적으로 감소한다. a가 높을수록 SampleRTT에 더 영향을 준다.
- DevRTT : RTT 변화율로, SampleRTT와 EstimatedRTT의 편차값이다. 즉, 현재 RTT값이 추정 RTT값으로부터 얼마나 벗어났는가에 대한 정보이다.
- 재전송 타임아웃 주기 (Timeout Interval) = EstimatedRTT + 4 * DevRTT
- 타임아웃나서 세그먼트를 재전송하게 되면, 재전송한 세그먼트에 대한 타이머가 시작된다.

### TCP의 RDT
- TCP는 비신뢰적인 인터넷 네트워크 계층(IP서비스)의 상위 계층에서 신뢰적인 데이터 전달(Reliable Data Transfer, RDT) 서비스를 제공한다.
- 파이프라인되는 세그먼트 (ACK의 응답이 없어도 다수의 세그먼트를 전송가능)
- 누적된 ACKs
- 단 하나의 재전송 타이머
- 재전송은 타임아웃이 되거나, 중복 ACKs를 수신했을 경우 발생한다.
- “간소화된 TCP 송신자”는 중복 ACKs를 무시하고 흐름제어 및 혼잡제어를 무시한다.

### TCP 송신자의 3가지 상황(Sender Events)
- 상위 계층으로부터 수신된 데이터 : 데이터를 받았으므로 상위 계층으로부터 데이터 전송 요청이 오게 된다.
    순서번호(Seq#)를 가진 세그먼트 생성 (Seq# : 세그먼트의 첫 번째 바이트의 바이트 스트림 번호)
    타이머가 실행되지 않고 있으면 타이머를 시작한다.
    타이머의 만료주기는 Timout Interval 로 계산한다.

- 타임아웃 : 타임아웃에 의해 세그먼트가 재전송되고, 전송했으나 ACK 받지 않은 가장 오래된 세그먼트에 대해 타이머가 다시 시작한다.
- ACK를 수신한 경우 : 이전에 ACK받지 않은 세그먼트의 ACK 이면, 해당 세그먼트를 ACK 응답된 세그먼트로 표시한다. 
    즉, 윈도우 크기를 조정한다. 아직 ACK받지 못한 세그먼트들이 존재한다면 타이머를 시작한다.