---
tags:
  - 자바/변수
---
## StringBuilder와 StringBuffer
`StringBuilder`와 `StringBuffer`는 가변 문자열 객체입니다. 문자열 변경이 많은 작업일 때  `String` 보다 유리합니다. 플러스(+) 연산 시에 객체가 생성되는 `String`의 단점을 보완한 객체입니다.

### 차이점
멀티 쓰레드 환경에서 동시성 제어 지원 여부입니다.

### StringBuilder
- `StringBuilder`는 single-thread 환경에 적합한 객체입니다.
- 동기화 비용이 없기 때문에 `StringBuffer`보다 빠른 성능을 보입니다.

### StringBuffer
- `StringBuffer`는 multi-thread 환경에 적합한 객체입니다. 내부 메소드마다 `synchronized` 키워드가 존재하여 thread-safe 합니다.
