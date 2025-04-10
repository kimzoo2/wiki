## blocking vs non-blocking
- 블로킹과 논블로킹은 **제어권**을 기준으로 합니다. 주체는 호출된 대상입니다.

https://musma.github.io/2019/04/17/blocking-and-synchronous.html

### blocking
- blocking은 자신이 수행하고 있던 작업이 다른 작업을 호출했을 때, 그 작업이 끝날 때까지 대기 했다가 수행하는 것을 의미합니다. 즉, 다른 작업이 제어권을 넘기지 않습니다.
**장점**
- 코드 구조가 단순하고 예측하기 쉽다.
- 안정성, race condition 문제를 줄이기 쉽다.
**단점**
- 성능 저하 : 자원을 기다리는 동안 다른 작업을 할 수 없기 때문에 응답성이 떨어질 수 있다.

### non-blocking
- non-blocking은 자신이 수행하고 있던 작업이 다른 작업을 호출해도 수행을 지속합니다. 즉, 다른 작업이 원래 작업을 수행할 수 있도록 제어권을 넘겨줍니다.
**장점**
- cpu 입장에서 **대기 시간을 감소**할 수 있다.
**단점**
- 예측하기 어렵다.


### 그럼 언제 blocking? non-blocking
blocking 
- i/o 작업이 적고 cpu 작업이 많은 프로그램
- 데이터 일관성이 중요한 프로그램
non-blocking
- i/o 작업이 많을 때
- 응답 시간이 중요할 때