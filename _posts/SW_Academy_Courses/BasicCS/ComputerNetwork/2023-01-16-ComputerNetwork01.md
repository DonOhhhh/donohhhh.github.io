---
categories: [SWACADEMY]
---

# ComputerNetwork01

## HTTP

### HTTP란?

- Hyper-text Transfer Protocl의 약자
- Socket을 이용한 웹 페이지 전송 프로토콜
- 웹 페이지 식별: URL

### HTTP 패킷

- http1.1에서는 연결 시작, 연결 종료가 있다.

### HTTP 요청 메세지

- 요청 내용
  - GET /images/logo.gif HTTP/1.1
- 헤더
  - Accept-Language: en
- 빈 줄
- 기타 메세지를 포함하여 표시된다.
- 요청 내용과 헤더 필드는 <CR><LF>로 끝나야 한다.
  - Carriage Return + Line Feed
  - 빈 줄<empty line>은 <CR><LF>로 구성

### HTTP 메소드

| HTTP 메소드 |Request Body| Response Body  | Safe  | 캐시 가능  |
|:---:|:---:|:--------------:|:-----:|:------:|
|GET|X|O|O|O|
|HEAD|X|X|O|O|
|POST|O|O|X|O|
|PUT|O|O|X|X|
|DELETE|X|O|X|X|
|CONNECT|O|O|X|X|
|OPTIONS|선택사항|O|O|X|
|PATCH|O|O|X|O|

### HTTP 응답 메세지

- 상태표시 행(status line)
  - 200 : OK
  - 301 : Moved Permanently
  - 400 : Bad Request
  - 404 : Not Found
  - 505 : HTTP Version Not Supported
- 응답 헤더필드(Content-Type: text/html)
- 빈 줄(empty line)
- 기타 메세지

### HTTP 응답과 요청이 이루어지는 과정

1. client와 server가 TCP Socket을 생성
2. client가 HTTP GET 메세지를 서버로 전송
3. 서버가 HTTP 응답 메세지를 클라이언트로 전송
4. 서버는 요청 객체를 클라이언트로 전송

### HTTP 서버/클라이언트 만들기

- 전통적인 방법
  - socket API, TCP socket을 이용한 HTTP 요청 파싱과 응답 모듈 개발
- Web framework를 이용하기
  - Python : Django, Flask, Fast API
  - Java : Netty, Spring
  - JavaScript : Node.js, Express.js
  - Ruby : Ruby on Rails
  - ASP.NET

### HTTP 프로토콜 발전

#### HTTP/1.0

- HTTP GET 메소드에 대해서 1개의 TCP 연결 사용
- 객체 1개 전송에 1개의 TCP 연결 -> 여러 개의 TCP 연결이 1개 웹 페이지에서 사용
- TCP 연결 만들 때 왕복지연시간 문제 -> non-persistent HTTP

#### HTTP/1.1

- HTTP, GET 메소드 처리에 이전 TCP 연결을 재사용
  - 왕복지연 시간 감소 -> persistent HTTP
- Pipelining으로 병렬 요청과 응답
  - 요청을 병렬 전송
  - 응답도 병렬 처리 가능
    - HTML + CSS response
  - HTTP/1.1 Pipelining 적용 사례
    - Apple iTunes에서 3배 성능

#### HTTP/2.0

- 웹 페이지 성능 향상 목표
  - HTTP/1.1 에서는 객체가 순차적으로 전송(1개의 TCP 연결대에서) -> Head-of-Line(HoL) 병목 현상 발생
  - 웹 페이지 로딩 시간을 50% 단축 목표
  - 멀티플렉싱으로 HoL 제거
- 바이너리 프레임
  - HTTP/1.1에서는 아스키코드로 전송됐었는데 binary로 전송한다.
  - 우선순위, 흐름제어
- 스트림 전송
  - 멀티플렉싱 지원
  - 우선순위 지원
- 헤더 압축
- Software
  - Apache web server
  - Nginx
  - MS IIS
  - Node.js
  - Jetty, Netty

#### HTTP/3.0

- TCP를 이용하는 HTTP/2.0의 HoL 병목현상을 해결하기 위해 UDP를 사용하였다.

## DNS

### DNS란?

- 인터넷에 연결된 컴퓨터의 이름과 IP 주소 분산 맵핑 시스템

### DNS 계층도

![DNS 계층도](/assets/images/2023/02/27/img_3.png)

