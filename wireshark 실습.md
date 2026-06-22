# wireshark 패킷 분석 실습

<img width="1466" height="1000" alt="스크린샷 2026-06-14 231606" src="https://github.com/user-attachments/assets/7e58abeb-9711-4a22-80d7-b6a7f94f6529" />
*실습 화면

No. 순서
Time 보낸 시간
Source 보낸 사람 IP
Destination 받는 사람 IP
Protocol 어떤 프로토콜인지
Length 패킷 크기

저는 wireshark를 통해 TCP 및 ICMP 프로토콜의 동작 과정을 분석하였습니다.

<img width="1459" height="1000" alt="스크린샷 2026-06-15 003219" src="https://github.com/user-attachments/assets/0421ef2e-70f7-4530-a333-3495880e99ef" />
*실습 화면

TCP 패킷을 분석하던 중 No. 222에서 [TCP Retransmission]이 발생한 것을 확인했습니다.

해당 패킷은 172.16.1.22가 172.30.1.30에게 SYN 패킷을 전송하는 과정에서 발생했습니다.
TCP는 전송한 패킷에 대한 응답을 일정 시간 동안 받지 못하면 동일한 패킷을 다시 전송합니다. 이를 Retransmission(재전송)이라고 합니다.

현재 실습 화면에서 SYN 패킷이 정상적으로 처리되지 않아 재전송된 것으로 분석되었습니다. 
이를 통해 TCP가 신뢰성 있는 통신을 위해 재전송 기능을 사용한다는 것을 확인할 수 있었습니다.

<img width="1449" height="992" alt="스크린샷 2026-06-15 005039" src="https://github.com/user-attachments/assets/839b7bd1-6c20-41e1-95a8-1749bfd11af0" />
*실습 화면

실습 중 No. 349와 No.350에서 무슨 일이 생겨 무엇일까? 라는 생각이 들어 찾아보았습니다.

[TCP Spurious Retransmission]은 불필요한 재전송을 의미합니다.
일반 Retransmission은 진짜 패킷이 사라졌을 때 발생하지만, Spurious ReTransmission은 이미 받은 데이터이지만 송신 측이 잃어버렸다고 착각을 하여 다시 보낸 경우 발생합니다.

왜 발생할까?

- 네트워크 지연이 심할 경우
- ACK가 늦게 도착할 경우
- 패킷 순서가 뒤바뀔 경우

ex)

```smalltalk
A → B
```

데이터 전송을 했지만

```smalltalk
B → A
```

ACK가 늦게 도착을 하여 A가 패킷이 손상되었나? 라고 판단을 하여 다시 보냄

그러나 이미 패킷은 정상적으로 도착한 상태라서 wireshark가 [TCP Spurious Retransmission]라고 표시합니다.

다음으로 [TCP Dup ACK]는 같은 ACK를 중복으로 보내졌다는 것을 의미합니다.

ex)

```smalltalk
1 받음
2 받음
3 손실
4 받음
```

이러면 수신자는

```smalltalk
3 보내줘
3 보내줘
3 보내줘
```

와 같은 ACK를 여러번 보냅니다.

이를 wireshark가 [TCP Dup ACK]라고 표시합니다.

<img width="1458" height="993" alt="스크린샷 2026-06-15 010601" src="https://github.com/user-attachments/assets/df61cd7d-529b-4359-ac0c-62daee0e8400" />
*실습 화면

다시 wireshark를 확인하던 중 No. 2767와 No. 2768에서도 무슨 일이 발생한 것을 확인했습니다.

우선 [TCP Previous segment not captured]는 패킷 앞에 있어야 할 TCP 패킷이 캡쳐되지 않았다는 것을 알려줍니다.

wireshark에게는 No. 100, No. 101, No. 103으로 보여서 No. 102가 없어? 라고 판단한 것입니다.

발생 원인

- wireshark가 너무 많은 패킷을 받아서 일부를 놓칠 경우
- 무선랜 캡쳐 중 수신을 실패할 경우
- 실제 네트워크에서 패킷 손실이 발생할 경우

보통은 캡쳐 과정에서 놓친 경우가 많다고 합니다.

다음으로 [TCP Out-Of-Order]은 패킷 순서가 예상과 다르게 도착할 경우 나타납니다.

ex)

정상일 경우

```smalltalk
1 → 2 → 3 → 4 → 5
```

순서이지만 

실제로

```smalltalk
1 → 2 → 4 → 3 → 5
```

순서로 도착한 경우입니다.

왜 발생할까요? 

1. 네트워크 지연

패킷 3이 느린 경로를 타고 패킷 4가 빠른 경로를 타면

```smalltalk
1 → 2 → 4 → 3
```

순서가 될 수도 있습니다.

1. Wi-Fi 환경

무선 환경은

- 간섭
- 재전송
- 신호 약화

때문에 [Out-Of-Order]가 꽤 자주 보입니다.

<img width="1452" height="1000" alt="스크린샷 2026-06-15 012657" src="https://github.com/user-attachments/assets/5c3782a5-f070-40e7-81b1-e29f5cacb1d4" />
그 이후 계속 wireshark를 살펴보던 중 No. 14892에서 초록색 글씨로 쓰인 줄을 확인했습니다.

ICMP Time-to-Live exceeded (Time to live exceeded in transit)은 IP 패킷에는 TTL (Time To Live) 값이 있는데,

ex)

```smalltalk
TTL = 64
```

이면 라우터를 하나 지날 때마다

```smalltalk
64 → 63 → 62 → 61 ...
```

이렇게 감소합니다.

TTL 값이 0이 되면 라우터가 “패킷이 너무 오래 다녔으므로 버릴게” 라고 하며 ICMP 메시지를 보냅니다.

이러한 메시지는 네트워크 경로 추적 (traceroute) 과정에서 자주 발생합니다.
