---
title: network-tcp-congestion-control
date: 2020-04-07 09:11:32
tags: 
- TCP

categories:
- Network
- TCP
---

## Congestion Control (혼잡 제어)

### TCP Fast Retransmit
- 송신측에서 Duplicate ACK를 받게 되면 패킷 손실로 간주하고 즉시 재전송
- Fast Retransmit 미사용 시 손실 발생 시 재전송 Timeout 만료 후 재전송
- 재전송 타이머 값이 종종 상대적으로 길어지므로, 손실된 패킷의 재전송 전에 지연시간이 커진다.
- 위의 상항을 해결하고자 중복 ACKs를 통해 손실된 세그먼트를 검출한다.
  송신측에서 바로바로 여러 개의 세그먼트를 전송할 경우, 세그먼트가 손실되면 수신측에서는 중복 ACK를 보내게 되는데, 타임아웃 전에 송신측에서 중복 ACK를 3번받게 되면 세그먼트를 즉시 전송한다. 
  즉, 수신측이 기다리는 순서번호의 세그먼트보다 큰 순서번호의 세그먼트가 3개 도착할 경우를 의미한다.
  
![](/images/network/tcp/retransmit/retransmit_1.jpeg)
![](/images/network/tcp/retransmit/retransmit_2.gif)