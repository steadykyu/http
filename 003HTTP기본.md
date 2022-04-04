# HTTP 요약

- HTTP 메시지에 모든 것을 전송가능하다.

- HTTP 역사 : HTTP/1.1을 기준으로 학습

> 특징

- 클라이언트 서버 구조

- 무상태 프로토콜(스테이스리스)

- 비연결성

- 단순함, 확장 가능한 기술

- 지금은 HTTP 시대

# 1. 모든 것이 HTTP

## 1.1 HyperText Transfer Protocol

HTTP 메시지에 모든 것을 전송

- 처음은 HTML, TEXT 문서를 전송
- IMAGE, 음성, 영상, 파일
- JSON, XML (API)
- 현재 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용

즉 지금은 HTTP 시대!

## 1.2 HTTP 역사

- HTTP/0.9 1991년: GET 메서드만 지원, HTTP 헤더X
- HTTP/1.0 1996년: 메서드, 헤더 추가
- **HTTP/1.1** 1997년: 가장 많이 사용, 우리에게 가장 중요한 버전
  - RFC2068 (1997) -> RFC2616 (1999) -> RFC7230~7235 (2014)
- HTTP/2 2015년: 성능 개선
- HTTP/3 진행중: TCP 대신에 UDP 사용, 성능 개선

  1.1 버전이 가장 중요

아무래도 발전속도가 빠르지는 않아서 RFC 2616 자료의 문서와 책이 되게 많다. 그래도 신규 스펙이 RFC7230 이므로 이 자료를 보는게 좋다.

## 1.3 기반 프로토콜

- TCP: HTTP/1.1, HTTP/2

- UDP: HTTP/3

```
TCP가 안정적이지만, 속도 때문에 APP위 UDP를 올려 최적화 하는 식으로 바뀌고 있다.
```

- 현재 HTTP/1.1 주로 사용

- HTTP/2, HTTP/3 도 점점 증가

> 참고 (현재 Web의 http)확인법

실제 웹에서 f12 개발자 모드 - 네트워크 - 우클릭(프로토콜) http 버전을 볼 수 있다.

1.1을 잘 이해하고 추가된 기능의 2 , 3 을 공부하면 된다.

> 특징

- 클라이언트 서버 구조

- 무상태 프로토콜(스테이스리스), 비연결성

- HTTP 메시지

- 단순함, 확장 가능

아래에서 하나씩 알아가 보자.

# 2. 클라이언트 서버 구조

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_1.png" width="70%" height="70%">
</p>

이러한 서버구조를 이용하여 복잡한 비즈니스 로직, 데이터 처리, 서버 아키텍처 등은 서버에 맡긴다. 또한 UI, UX등을 클라이언트에 집중시킨다. 그결과 양쪽이 독립적으로 진화를 할 수 있다.

# 3. Stateful, Stateless

## 3.1 무상태 프로토콜

스테이스리스(Stateless)

> 요약

- 서버가 클라이언트의 상태를 보존X

- 장점: 서버 확장성 높음(스케일 아웃)

- 단점: 클라이언트가 추가(더 많은) 데이터 전송

## 3.2 예제 : 상태 유지 - stateful

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_2.png" >
</p>

> stateful 점원이 중간에 바꼈을때

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_3.png" >
</p>

서버에 비유하면 데이터를 못 찾고 서비스 장애가 나는 상황이다.

## 3.3 예제 무상태 - Stateless

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_4.png" >
</p>

점원은 고객의 마지막 말만 듣고도, 적절한 결론을 이해할 수 있다.

> 정리

- 상태 유지: 중간에 다른 점원으로 바뀌면 안된다.

```
(중간에 다른 점원으로 바뀔 때 상태 정보를 다른 점원에게 미리 알려줘야 한다.)
```

- 무상태: 중간에 다른 점원으로 바뀌어도 된다.
  - 갑자기 고객이 증가해도 점원을 대거 투입할 수 있다.
  - 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있다.
- 무상태는 응답 서버를 쉽게 바꿀 수 있다. -> 무한한 서버 증설 가능(서버아키텍처 확장)

## 3.4 그림으로 보는 상태 유지 -stateful

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_5.png" >
</p>

클라 A는 서버1과만 통신해야한다.

서버1이 망가지면 다른 서버에서 **처음부터** 다시 시작해야한다.

## 3.4 그림으로 보는 무상태 - stateless

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_6.png" >
</p>

서버 1이 망가져도 서버2번이 이어서 로직을 진행 할 수 있다.

> scale out (스케일 아웃)

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_7.png" >
</p>

## stateless 한계

- 1.상태유지에 비해 클라이언트가 전송해야 하는 데이터가 더 많다.

- 2.모든 것을 무상태로 설계 할 수 있는 경우도 있고 없는 경우도 있다.

- 무상태

  - 예) 로그인이 필요 없는 단순한 서비스 소개 화면(이벤트)

- 상태 유지
  - 예) 로그인
- 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지해야 한다.

- 일반적으로 브라우저 쿠키와 서버 세션등을 사용해서 상태 유지할 수 있다.(추후배움)

- 최대한 무상태로 만들자. 상태 유지는 필요시에 최소한만 사용

# 4. 비 연결성(connectionless)

## 4.1 연결을 유지하는 모델

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_8.png" >
</p>

클라 3이 요청할때 클라 1과 2는 놀고 있어도, 계속 연결을 하고 있어야한다는 단점이 있다.

## 4.2 연결을 유지 하지 않는 모델

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_9.png" >
</p>

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_10.png" >
</p>

요청과 응답을 나눌때만 자원을 사용함.

## 4.3 HTTP - 비연결성

- HTTP는 기본이 연결을 유지하지 않는 모델

- 일반적으로 초 단위의 이하의 빠른 속도로 응답

- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음

  - 예) 웹 브라우저에서 계속 연속해서 검색 버튼을 누르지는 않는다.

- 서버 자원을 매우 효율적으로 사용할 수 있음

## 4.4 비 연결성의 한계와 극복

- 요청, 응답마다 TCP/IP 연결을 새로 맺어야 함 - 3 way handshake 시간 추가

- 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, css, 추가 이미지 등등 수 많은 자원이 함께 다운로드되는데 각각 요청과 응답 해주어야함.

> 극복

- 지금은 HTTP 지속 연결(Persistent Connections)로 문제 해결

- HTTP/2, HTTP/3에서는 더 많은 최적화가 이루어짐

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_11.png" >
</p>

## 4.5 (stateless)스테이트리스를 기억하자.

서버 개발자들이 어려워하는 업무

- 정말 같은 시간에 딱 맞추어 발생하는 대용량 트래픽

  - 예) 선착순 이벤트, 명절 KTX 예약, 학과 수업 등록
  - 예) 저녁 6:00 선착순 1000명 치킨 할인 이벤트 -> 수만명 동시 요청

- stateless 할수 없어보이더라도, 최대한 stateless하게 짜야한다.

  - 예) 실무에선 처음에는 html 정적페이지를 보게하다가 이벤트를 누르도록 설계한다.

# 5. HTTP 메시지

HTTP 메시지에 모든 것을 전송할 수 있다.

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_12.png" >
</p>

그림은 요청메세지에 내용이 없지만, 문자나 body 본문이 들어와도 된다.

요청메세지, 응답 메시지가 약간 다르다.

**공백라인은 무조건 들어가야한다.**

## 5.1 시작 라인 - 요청 메세지

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_13.png" >
</p>

- start-line = **request-line(O)** / status-line(x)
  > 구조

```
 request-line = method SP(공백) request-target SP HTTP-version CRLF(엔터)
```

- HTTP 메서드 (GET: 조회)
- 요청 대상 (/search?q=hello&hl=ko)
- HTTP Version

### 5.1.1 시작 라인 - HTTP 메서드

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_14.png" >
</p>

- 종류: GET, POST, PUT, DELETE...

- 서버가 수행해야 할 동작 지정
  - GET: 리소스 조회
  - POST: 요청 내역 처리

### 5.1.2 시작 라인 - 요청 대상

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_15.png" >
</p>

- absolute-path[?query] (절대경로[?쿼리])
- 절대경로= "/" 로 시작하는 경로
- 참고: \*, http://...?x=y 와 같이 다른 유형의 경로지정 방법도 있다.

### 5.1.3 시작 라인 - HTTP 버전

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_16.png" >
</p>

- HTTP Version

## 5.2 시작 라인 - 응답 메세지

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_17.png" >
</p>

- start-line = request-line(x) / **status-line(O)**
- status-line = HTTP-version SP status-code SP reason-phrase CRLF

- HTTP 버전
- HTTP 상태 코드: 요청 성공, 실패를 나타냄
  - 200: 성공
  - 400: 클라이언트 요청 오류
  - 500: 서버 내부 오류
- 이유 문구: 사람이 이해할 수 있는 짧은 상태 코드 설명 글

## 5.3 HTTP 헤더

> 형식

```
• header-field = field-name ":" OWS field-value OWS (OWS:띄어쓰기 허용)
• field-name은 대소문자 구문 없음
```

":" 왼쪽에는 공백이 절대 들어가면 안된다.

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_18.png" >
</p>

> 헤더의 용도

- HTTP 전송에 필요한 모든 부가정보(메타데이터 정보)

  - 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보...

- 표준 헤더가 너무 많음(추후 배움)
  - https://en.wikipedia.org/wiki/List_of_HTTP_header_fields
- 필요시 임의의 헤더 추가 가능
  - helloworld: hihi

## 5.4 HTTP 메시지 바디

<p align="center">
<img src ="https://github.com/steadykyu/http/blob/master/img/http3_19.png" >
</p>

- 실제 전송할 데이터
- HTML 문서, 이미지, 영상, JSON 등등 byte로 표현할 수 있는 모든 데이터 전송 가능

## 5.5 단순하면서 확장이 가능

- HTTP는 단순하다. 스펙도 읽어볼만...

- HTTP 메시지도 매우 단순

- 크게 성공하는 표준 기술은 단순하지만 확장 가능한 기술이다.
