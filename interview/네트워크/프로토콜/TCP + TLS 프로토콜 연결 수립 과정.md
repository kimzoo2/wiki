
![연결 수립 과정](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*NzdidW6gOv1LygE5)
TLS 사용해서 HTTP 연결하려면 총 6개의 패킷 필요하다.
TCP 연결 3개 [[3way handshake, 4way handshake]] + TLS 연결 4개(TCP 세번째 단계랑 clienthello랑 결합) = 총 6개
https://medium.com/@gtamilarasan/a-closer-look-at-http3-quic-1b2b97a5cd50