---
tags:
  - 네트워크/프로토콜/웹소켓
  - 스프링
---

# STOMP란?
Simple Text Oriented Message Protocol의 약자이다. **메세지 브로커를 활용**하여 [[pub-sub 아키텍처]]기반으로 **웹소켓 통신**해주는 서브 **프로토콜**이다.

### 장점
- 프레임을 제공한다. 개발자가 웹소켓 프로토콜의 프레임을 규정하지 않아도 된다.
- **WebSockerHandler**를 구현할 필요 없이 엔드포인트를 별도로 관리할 수 있다.(@MessageMapping 활용)
- **외부 메세지 브로커**를 사용할 수 있다.

> **WebSockerHandler란**?
> 웹소켓 연결을 관리하고 클라이언트와 서버 간의 메세지를 처리한다.

### 단점
- SimpleBroker를 사용하면 비정상적인 서버 종료 시 브로커에 저장된 정보들이 모두 삭제될 수 있다.


### 구조
**Command, Header, Body**
Command : 메세지 동작 type을 정의한다.
Header :  메세지의 추가 정보를 제공한다.
Body : 메세지의 내용

### 동작원리
- 구독자가 topic 구독 요청을 한다.
- 메세지 브로커가 topic을 관리하고 구독자 정보를 저장한다.
- 발행자가 topic에 메세지를 발행한다.
- 메세지 핸들러가 전달 받은 메세지를 처리하고 **메세지 브로커에게 메세지**가 전달된다.
- 메세지 브로커는 토픽과 관련된 구독자들에게 메세지를 브로드 캐스팅한다.

![[스크린샷 2024-11-01 오후 5.01.28.png]]
### 핵심 구성 요소
- publisher(발행자) : 토픽에 메세지를 발행한다.
- subscriber(구독자) : 토픽을 구독하고 메세지를 소비한다.
- topic : 그룹, 메세지 발행 단위
- messageHandler : 메세지를 처리하는 핸들러
- channel : 메세지가 전달되는 통로
- messageBroker : SimpleBroker는 스프링이 메모리에 구성한 브로커다.

#### channel와 메세지
![[스크린샷 2024-11-01 오후 5.03.08.png]]

- InboundChannel : 클라이언트로부터 전달받은 메세지를 전달한다.
- OutboundChannel : 웹소켓 클라이언트에 서버로부터 전달받은 메세지를 전달한다.
- BrokerChannel : 서버 측 애플리케이션 코드로 브로커채널로 메세지를 전달하면 브로커로 전달된다.

### flow 이해하기
1) /app prefix 로 전달된 메세지
![[스크린샷 2024-11-01 오후 5.08.40.png]]
- /app prefix로 메세지를 발행하면 @MessageMapping 어노테이션이 붙은 컨트롤러로 /app prefix가 제거되어 라우팅됩니다. 
- 처리가 된 메세지는 broker channel을 통해 메세지 브로커에게 전달됩니다.
- 메세지 브로커가 구독자들을 찾아 outboundChannel로 메세지를 전달하고 구독자들에게 브로드 캐스팅됩니다.



2) /app prefix 없이 전달된 메세지
![[스크린샷 2024-11-01 오후 5.16.26.png]]
- /app prefix 없이 메세지를 발행하면 InboundChannel을 거쳐 메세지 브로커로 바로 전달됩니다.
- 메세지 브로커는 해당 topic으로 구독하는 구독자를 찾아 outboundChannel로 메세지를 전달하고 구독자들에게 브로드 캐스팅됩니다.


![[스크린샷 2024-11-01 오후 5.19.58.png]]


### 레퍼런스
https://tecoble.techcourse.co.kr/post/2021-08-14-web-socket/
https://brunch.co.kr/@springboot/695
https://velog.io/@ohjinseo/WebSocket-Spring-Boot-stomp-Redis-PubSub-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%B1%84%ED%8C%85-%EA%B5%AC%ED%98%84
https://docs.spring.io/spring-framework/reference/web/websocket/stomp/message-flow.html