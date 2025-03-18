---
tags:
  - 네트워크/프로토콜/HTTP
---

HTTP 특징
- **stateless**
- **Connectionless**
- server - client 구조
- 비연결성 - 요청 - 응답이 끝나면 즉시 커넥션 종료된다.
- 단순함, 확장 가능
- cacheable?

### HTTP 버전별 특징
#### HTTP 1.0
- GET 이외의 HTTP 메소드 도입 **(POST, HEAD)**
- 공식적으로 **헤더 지원**
- 지속 연결(persistent connection, keep-alive)이 지원되지 않았기 때문에 **연결 주고 받을 때마다 매번 종료**
	- 지속 연결되지 않으면 서비스를 요청했을 때 html, js, css 등 여러 리소스가 필요한데 매번 연결 수립하고 데이터 보내고 종료되고 연결 수립하고 데이터 보내고 종료됨

#### HTTP 1.1
- **지속 연결 도입**
	- 여러 HTTP 요청과 응답을 한 번의 연결로 처리할 수 있다. 지정한 timeout동안 커넥션을 닫지 않는다.
- 응답이 수신되지 않으면 재요청하는 **파이프라이닝 기능 도입**
- **평문 기반의 메세지** 송수신
- **Host 헤더 추가**
	- 하나의 서버에서 여러 도메인 처리 가능.

**문제점**
- **HOL blocking 문제**
- 헤더 중복, 헤더가 데이터보다 큰 문제 발생

##### HeadOfLineBlocking이 뭔가요?
- 첫번째 요청 데이터가 응답되지 않으면 다른 요청 데이터도 대기하는 문제입니다.
- 큐에 데이터가 적재되어 순차적으로 처리할 때, 첫번째 패킷의 지연이 발생하면 다른 패킷들도 지연하는 문제다.

> **지속 연결과 웹소켓, SSE와의 차이**
> - 지속 연결은 연결을 유지한 상태로 데이터를 주고 받을 수 있다. **연결을 유지한 상태로 데이터를 주고 받는다**는 점에서 지속 연결이 웹소켓, SSE와 공통점이 있으나 **클라이언트의 요청이 있을 때만 데이터를 주고 받을 수 있다**는 차이점이 존재한다. 웹소켓, SSE는 요청이 없어도 서버에서 데이터를 전송할 수 있다.
> [웹소켓과 지속 연결의 차이](https://stackoverflow.com/questions/7620620/whats-the-behavioral-difference-between-http-keep-alive-and-websockets)

#### HTTP 2.0
- HTTP 1.1을 개선하기 위한 버전
- 송수신 효율을 높이기 위해서 **헤더를 압축**하여 전송
- **바이너리 프레임으로** 송수신 -> 바이너리 데이터로 패킷을 전송하여 효율 증가.
- 클라이언트가 요청하지 않아도 필요할 것으로 예상되는 **리소스를 미리 전송하는 서버 푸시**
- HOL 블로킹 문제 완화를 위해 **멀티플렉싱** 도입
	- Application Layer 에서의 HOL Blocking 은 해결했지만 Transport Layer 에서의 HOL Blocking 은 여전히 문제

#### HTTP 1.1과 HTTP 2.0
- 크게 **성능과 전문** 차이가 있습니다. 1.1 버전은 전문이 **평문 형태로 전달**되고 2버전은 **전문이 바이너리로 변경**되어 전달됩니다. 성능적으로는 `Multiplexing`과 `Server push` 등이 추가되었습니다. 1.1 버전은 파이프라이닝을 통해 통신을 하기 때문에 **HeadOfBlocking**이라는 이슈가 존재합니다.
- 하지만 2버전은 **Multiplexing**를 통해 요청을 다중화하였기 때문에 1.1 버전에서 발생하는 **HeadOfLineBlocking** 이슈는 발생하지 않고 속도도 개선되었습니다.
- **server push는** 예를 들어 클라이언트가 html 파일을 요청했을 때 추가로 요청될 것 같은 js나 css 파일도 서버에서 전달하는 기능입니다. 통신의 효율성을 개선시킨 기능입니다.
- 이뿐만 아니라, 기존 1.1 버전은 HTTP 헤더가 중복되었는데 2버전 부터는 stream 형태로 통신을 하기 때문에 중복되는 헤더는 전송하지 않는 **header compression**이 추가되었습니다.


### HTTP3
**QUIC 프로토콜**을 기반으로 성능을 향상 시킨 HTTP 버전이다.

### QUIC(Quick UDP Internet Connections)
- 구글에서 만든 TCP를 대체하기 위해 나온 프로토콜이다.
- TCP의 `3way handshake` 대신에 보다 효율적인 `handshake`를 사용하기 때문에 보다 더 빠르다.
- UDP를 사용하기 때문에 **다방향 브로드캐스팅을 지원해서 HOL 문제를 해결**한다.
- 페이로드 뿐만 아니라, 메타데이터도 (ex - 헤더) 암호화하기 때문에 보다 더 안전하다.

#### 어떻게 HOL 문제를 해결하나요?
![HTTP/2.0과 QUIC의 데이터 전송 방식 차이](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzrXUI%2FbtsJdoCfr16%2FkkB5ErW61tGO4JU5qA9zu0%2Fimg.png)

- **TCP는 단일 지점을 연결**해서 통신한다. 하지만 UDP는 **노드 간의 엔드포인트를 여러 지점으로 둘 수 있다**. 그래서 하나의 엔드포인트가 응답이 지연되거나 중단된다면 **다른 엔드포인트로 스트림이 계속 동작**할 수 있게 된다.


#### 보다 효율적인 handshake란?
![QUIC shake](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*yrWmmqAzDDkCq6rP)

- QUIC에는 TLS가 내장되어 있다.
- 연결 수립([[3way handshake, 4way handshake|tcp handshake]])과 보안([[HTTPS#^310949|tls handshake]])이 단일 단계로 결합되어 있다.

-> **HTTP 2.0의 handshke와 비교**해보자 [[TCP + TLS 프로토콜 연결 수립 과정]]

[quic과 http3가 뭔가요?](https://www.geeksforgeeks.org/what-is-quic-and-http-3/)
[http는 왜 udp를 쓰나요?](https://www.reddit.com/r/programming/comments/11l6v62/why_http3_uses_udp_protocol_under_quic_instead_of/)

https://dkswhdgur246.tistory.com/53