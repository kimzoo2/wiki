---
tags:
  - 네트워크/프로토콜/TCP
  - 네트워크/프로토콜/UDP
---

## TCP
- **3-way-handshake와 4-way-handshake** 으로 **신뢰성을 보장**하고 **연결 지향 방식**(=패킷을 전송할 때 논리적 경로가 존재함)으로 **패킷 교환 방식을 사용**합니다.  그렇기 때문에 HTTP 프로토콜로 전송 시 TCP를 기반으로 통신하는 편입니다. 다만, **신뢰성** 있는 대신 UDP에 비하여 **데이터 전송 속도가 느리다**는 단점이 있습니다.


## UDP
- UDP는 **비연결형(Connectionless) 프로토콜**로 **패킷마다 순서를 부여하여 재조립한다는 특징**이 있습니다. 데이터 전송 속도가 TCP에 비하여 빠르지만 **신뢰성을 보장하지 않기 때문에 연속성이 중요한 서비스** 즉, **스트리밍 서비스에 자주 사용되는 프로토콜**입니다.


[TCP](https://evan-moon.github.io/2019/11/17/tcp-handshake/)