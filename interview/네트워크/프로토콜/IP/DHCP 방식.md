---
tags:
  - 네트워크/프로토콜/IP
---
### DHCP(Dynamic Host Configuration Protocol)란? 
네트워크 장치에 **IP 할당 및 설정을 하는 프로토콜**을 의미한다. 클라이언트 - 서버 아키텍처에 기반하며, 네트워크 장치가 IP 주소를 할당 받기 위해 요청하는 순간 연결이 설정된다.


![[DCHP-Request.png]]

### 작동원리
**DHCP Discover** 
- 브로드캐스트 (다른 서버들은 이 메세지를 보고 삭제함)
**DHCP Offer**
- DHCP 서버가 호스트에게 IP를 제공한다.
**DHCO Request**
- 호스트가 임대 요청을 보낸다.
**DHCP ACK**
- 승인 후, 호스트에게 IP를 전송한다.


https://www.techtarget.com/searchnetworking/definition/DHCP
https://youtu.be/e6-TaH5bkjo?feature=shared