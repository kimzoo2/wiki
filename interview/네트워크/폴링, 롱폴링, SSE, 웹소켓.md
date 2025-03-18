---
tags:
  - 네트워크/프로토콜/웹소켓
  - 네트워크/디자인/폴링
  - 네트워크/디자인/롱폴링
  - 네트워크/디자인/SSE
  - 네트워크/디자인/웹소켓
---


### 폴링(Polling)
- 폴링은 HTTP 프로토콜을 활용하여 주기적으로 서버에 요청을 보내 응답을 받는 것을 의미한다. HTTP 프로토콜을 사용하기 때문에 헤더가 반복되고 요청이 불필요할 수 있으며, **서버에 부담이 되는 단점**이 존재한다.

### 롱폴링(Long Polling)
- 롱폴링은 폴링의 단점을 보완한 방식이다. 주기적으로 요청을 보내는 폴링과는 다르게 롱폴링을 사용하면 클라이언트가 **오랜 시간 응답을 대기한다**는 특징이 있다. 서버에서 응답이 오거나 시간 초과가 발생할 때만 응답을 보낸다. 폴링에 비해 **불필요한 요청이 지속되지 않는다**는 특징이 있다.

## SSE (Server sent events)
- 롱폴링은 서버에서 응답이 오면 커넥션이 종료된다. 그래서 응답을 다시 받고자 하면 요청이 필수적이다. SSE는 그 문제를 보완한다. 
- 클라이언트가 요청을 하고 연결을 계속 유지된다. 서버가 응답을 보내도 커넥션이 종료되지 않는다는 특징이 있다. 대신 **단방향** 통신(서버->클라이언트)이기 때문에 양방향 통신을 위한 구조에는 알맞지 않다.
- IE는 지원하지 않는다.

## 웹소켓(Web socket)
- HTTP나 HTTPS 위에서 동작하는 서버와 클라이언트 간의 지속적인 연결을 의미한다. 
- 전이중 통신이 특징이며 HTTP 프로토콜 핸드쉐이크가 정상적으로 수행되면 ws 프로토콜로 업그레이드 되어 사용된다. HTTP 통신으로 인한 헤더 중복 문제를 해결할 수 있고 **실시간으로 양방향 통신**이 가능하다는 특징이 존재한다.



https://www.karanpratapsingh.com/courses/system-design/long-polling-websockets-server-sent-events
https://medium.com/@asharsaleem4/long-polling-vs-server-sent-events-vs-websockets-a-comprehensive-guide-fb27c8e610d0
https://medium.com/techieahead/http-short-vs-long-polling-vs-websockets-vs-sse-8d9e962b2ba8