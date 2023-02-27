---
categories: [SWACADEMY]
---

# ComputerNetwork02

## 컴퓨터 네트워크 성능

### 인터넷 성능 지표 : 속도

![속도](/assets/images/2023/02/27/img_4.png)

### 인터넷 성능 지표 : 지연시간

![지연시간](/assets/images/2023/02/27/img_5.png)

- 전송시간: 1비트가 컴퓨터에서 나가는 시간 - 네트워크 카드에 따라 좌우됨.
- 전파시간: 물리적인 거리에 따른 시간(고정값)
- 큐잉시간: 가변시간, 제일 중요한 변수, 캐싱을 통해서 응답 속도를 빠르게 할 수 있다.

### 인터넷 성능 지표 : 손실률

- 100개의 패킷을 보냈을 때 99개의 패킷이 전송 성공 -> 1%의 손실률

## 비동기 프로그래밍(Async)

### 동기 VS 비동기

#### 동기(Synchronous)

- 특정 작업이 끝나면 다음 작업을 처리하는 순차처리 방식

#### 비동기(Asynchronous)

- 여러 작업을 처리하도록 예약한 뒤 끝나면 결과를 받는 방식
- cpu연산이 DB/API 연산보다 훨씬 빠르다.

## 전송계층

### 전송계층

#### TCP

- 신뢰성 : 오류 탐지 및 복구, 순서전송, 중복제거
- 흐름제어 : 수신자의 상태에 따른 전송량 조절
- 혼잡제어 : 네트워크 + 수신자의 혼잡상태에 따른 전송량 조절
- 연결관리 : 라우터, 네트워크 카드

#### UDP

- 아무것도 없음

### 프로세스

- 전송계층의 양 끝은 프로세스
- 포트번호가 프로세스를 식별
- netstat -tn

### 멀티플렉싱 & 디멀티플렉싱

- 멀티플렉싱 : 여러 소켓에서 데이터를 수집
- 디멀티플렉싱 : 수신 세그먼트를 적절한 소켓에 전달

### UDP

- User Datagram Protocol(RFC768)
- port number를 이용한 멀티플렉싱 이외의 기능없음
- 연결을 만들지 않음(connectionless)
- 빠르다, 단순하다

![UDP](/assets/images/2023/02/27/img_6.png)

### UDP 체크섭(checksum)

- goal: 전송된 세그먼트의 비트에러 탐지

### 신뢰성 있는 데이터전송 원리

- 패킷손실 발생 탐지
- 재전송을 통한 패킷복구
- 예)
  - Stop-and-Wait ARQ
  - Go-back-N ARQ
  - Selective-Repeat ARQ
  - TCP

### TCP

- full duplex data
  - 동일 연결에서 양방향 데이터 전송
  - MSS : Maximum segment size
- Connection-oriented
  - handshaking 과정을 통한 연결 설정
- flow control
  - sender will not overwhelm receiver
- point-to-point
  - one sender, one receiver
- reliable, in-order byte steam
- pipelining
  - TCP congestion and flow control set window size
- 3-way handshake

### TCP 혼잡제어

- 서버가 나한테 얼마나 보낼 것인지 내가 서버한테 얼마나 보낼 것인지를 결정하는 것!

#### AIMD

- Additive Increase Multiplicative Decrease
- 점진적으로 더하면서 보낸다. 감소할 땐 급격하게 줄여서 보낸다.

#### slow start

- tcp 연결이 끝난 후 내가 1초에 몇 바이트를 보낼지를 결정하는 과정
- 1 segment = 1460byte
- 1 MSS(Maximum segment size) : 1460byte
- cwnd의 크기를 늘리고 줄인다.
- 초기엔 천천히 보내다가 갈수록 기하급수적으로 빠르게 보낸다.



