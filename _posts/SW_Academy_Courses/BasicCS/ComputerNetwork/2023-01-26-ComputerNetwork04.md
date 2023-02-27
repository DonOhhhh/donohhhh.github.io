---
categories: [SWACADEMY]
---

# ComputerNetwork04

## TLS

### TLS란?

- Transport-Layer Security
- 웹 서버 <-> 클라이언트 사이의 암호화 통신을 가능하게 해준다.
- SSL에서 TLS로 발전함.
- HTTPS = HTTP + TLS(port 443)

### TLS의 핵심 기능

- 암호화 : 제 3자로부터 전송되는 데이터를 숨김
- 인증 : 정보 교환 당사자가 진짜임을 보장
- 무결성 : 데이터가 위조 또는 변조되지 않음을 보장

### TLS 작동 방식

- 웹 서버
  - TLS 인증서가 설치되어있어야 함.
  - 인증 기관이 도메인을 소유한 사람에게 TLS 인증서 발행
  - 인증서 : 서버의 공개 키와 함께 도메인 소유자 정보 포함
- TLS 연결
  - TLS 버전(1.0, 1.2, 1.3)등 지정
  - 사용할 암호 제품군 결정 : 암호화 알고리즘 집합
  - 서버 TLS 인증서를 이용하여 서버의 신원을 인증
  - Handshake가 완료된 후 메세지 암호화를 위한 세션 키 생성
  - 공개키 이용
  - 메세지 인증 코드(MAC) 서명 : 무결성 보장

### TLS handshake 프로토콜

![TLS handshake](/assets/images/2023/02/27/img_7.png)

- ClientHello
  - 버전, 사용가능한 암호화 종류
- ServerHello
  - 암호화 알고리즘 종류
- Certificate
  - 인증서
- ServerHelloDone
- ClientKeyExchange
  - Premaster secret 생성 후 서버의 공인키로 암호화 후 전송

## WebRTC

### WebRTC란?

- Plugin 없이 웹 브라우저에서 음성/영상 P2P 공유 가능하게 하는 Open Source Project이다.
- 웹으로 하는 실시간 통신(화상회의)를 할 때 쓰인다.

### WebRTC로 가능한 서비스들

![untact services](/assets/images/2023/02/27/img_8.png)

### WebRTC 구성 요소

- getUserMedia
  - 오디오와 비디오 미디어
- RTCPeerConnection
  - 피어 간 오디오, 비디오 통신 활성화: STUN
  - 신호처리, 코덱 관리, P2P 통신, 보안, 대역폭 관리 수행
- RTCDataChannel
  - 피어 간 양방향 임의 데이터 통신 허용: Tunnel API
  - 웹 소켓과 동일한 API를 사용
  - 낮은 레이턴시

### 피어 연결

- NAT 통과 기술 이용
  - STUN, TURN, ICE
- 신호
  - 다양한 방식 가능: REST/RPC




