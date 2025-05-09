
## 혼잡 제어(Congestion Control)

^a2889c
- 많은 노드의 요청이 라우터에 몰릴 경우, 라우터는 자신에게 온 데이터를 처리할 수 없게 된다. 이런 경우 호스트들은 또 재전송을 하고 결국 **혼잡만 가중 시켜서 오버플로우가 패킷 손실**이 발생한다. 네트워크의 혼잡을 피하기 위해 송신 측에서 전송속도를 강제로 제어한다. 이를 혼잡 제어라고 한다.
- 네트워크의 **혼잡**을 방지하는 기술이다.
- 송신 측의 데이터 전달과 **네트워크의 데이터 처리 속도 차이를 해결**하기 위한 기법이다.

> 네트워크가 혼파망 되기 전에 다른 노드들이 깜짝 놀라서 속도 조절하는 느낌. 뉴비 본 고인물 느낌..

> 혼잡(Congestion)이란?
> - 네트워크를 통해 **너무 많은 데이터가 전송되고 네트워크가 과부하**되어 패킷이 손실되고 네트워크 성능이 저하되는 현상을 의미한다.

목표
- 데이터 흐름 속도를 제어해서 네트워크의 과부하를 줄이고 패킷 손실과 성능 저하를 방지한다.
단점
- 데이터 흐름을 조절하기 때문에 데이터 전송이 지연된다.

#### 혼잡 제어 알고리즘, 혼잡 회피 방식
##### AIMD(Additive Increase/ Multicative Decrease) 합-증가, 곱-감소

> Additive
> : 부가의, 첨가물, 덧셈의
> Multicative
> : 곱셈의

처음에 **패킷을 하나씩, 하나씩** 보내고 문제 없으면 **window 크기를 1씩 증가(합 증가)시키는 방법**이다. 만약 패킷 전송에 **실패하면 윈도우 크기를 절반**(곱 감소)으로 줄인다.

**단점**
- 초기에 높은 대역폭을 사용하지 못하기 때문에 **느리다**. (하나씩, 하나씩 보내니..)
- **네트워크가 혼잡해지기 전에 미리 감지하지 못하고 발생하면(실패하거나, 느려지면) 문제를 해결하는 방법**이다.

##### Slow Start (느린 시작)
- 패킷을 하나씩 보내다가 각각의 **ACK 패킷마다 window 크기를 1씩 증가**시킨다. 즉, 한 주기마다 2배가 된다. 혼잡이 발생하면 **Window size를 1로 떨어트린다**.
- 한 번 혼잡이 발생하면 네트워크 수용량을 예상할 수 있기 때문에 이후부터 1씩 증가시키는 방법이다.


tahoe
- 타임아웃, 3way duplicate 발생하면 네트워크 혼잡하다고 판단 
reno
- 타임아웃, 3way duplicate 발생하면 네트워크 혼잡하다고 판단, timeout과 3way duplicated를 따로 관리


## 흐름 제어 (flow control)
- 여러 **노드 간 데이터 흐름을 제어**하는 기술입니다. **송신자가 전송하는 패킷 속도가 빠를 때 발생하는 문제입니다.**

**목표**
- 패킷 손실과 네트워크 성능 저하를 방지하기 위해 **버퍼 오버플로우를 방지**합니다.
**단점**
- **데이터 흐름 속도를 제어**하기 때문에 데이터 전송이 지연된다.
- 혼잡한 네트워크 환경에서는 적합하지 않다. 

**메커니즘**
- 슬라이딩 윈도우 프로토콜
- 스탑앤 웨이트

#### 알고리즘

##### Stop and wait
- 매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방법이다.

##### 슬라이딩 윈도우 프로토콜 (더 찾아보기)
- 송신자와 수신자가 연결을 수립할 때 수신자가 **window(수신자의 버퍼에서 사용 가능한 크기)를 전달**한다. 만약 TCP에서 수신자의 윈도우 크기가 0이면 0이 아닐 때까지 데이터 전송을 중단한다.
- 즉, 송신 측이 **수신이 처리할 수 있는 데이터의 양**을 알고 있다.

https://www.techtarget.com/searchnetworking/definition/sliding-windows


### 흐름 제어 vs 혼잡 제어
- **흐름 제어**는 트래픽 속도 제어를 **수신자가 제어**하지만 **혼잡 제어는 송신자가 트래픽 속도를 제어**한다.

| 흐름 제어                            | 혼잡 제어                                   |
| -------------------------------- | --------------------------------------- |
| **느린 수신자**에게 과부하가 걸리는 것을 방지하기 위함 | **네트워크를** 통해 너무 많은 데이터가 전송되는 것을 방지하기 위함 |
| 데이터 링크 계층에서 사용                   | 네트워크 및 전송 계층에 적용                        |

## 오류 제어
- 패킷이 유실되거나 잘못된 데이터가 수신되었을 때 해결하는 방법이다.


**window 크기란?**
- 수신자가 처리할 수 있는 버퍼의 크기, 응답 헤더에 담아서 전달한다.


https://jwprogramming.tistory.com/36
https://www.baeldung.com/cs/tcp-flow-control-vs-congestion-control
[혼잡 제어](https://evan-moon.github.io/2019/11/26/tcp-congestion-control/)
[흐름제어](https://evan-moon.github.io/2019/11/22/tcp-flow-control-error-control/)