## String이란?
- 문자열을 저장하는 **불변 객체**

### 장점
- 불변성

### 단점
- 메모리 사용량 증가
### 스트링 생성자 차이
```java
// 리터럴
String a = "String";
// new 연산자를 통한 인스턴스 생성 
String b = new String("String");
```
- 리터럴 String은 **String constant Pool**에 들어감
- new 연산자를 통해 생성된 String은 **heap 객체**에 들어감

### 스트링의 위험한 메소드
- `intern()` : String pool에 해당 문자열이 있는지 검색하고 있으면 반환하고 없으면 String pool에 문자열을 넣는 메소드. 

### 스트링의 단점을 보완한 가변 객체
- `StringBuffer` : 문자열 가변 **thread-safe한** 객체
- `StringBuilder` : 문자열 thread-safe 하지 **않은** 객체

## String constant pool이란?
- **재사용성을 향상 시키기 위한 pool**이다.
- 모든 객체는 `heap` 영역에 저장되는데 String만 heap 영역에 있는 `String constant pool`에 저장된다. 새 문자열 객체를 생성할 때마다 해당 문자열이 존재하는지 확인할 수 있게 합니다. 만약 존재하면 Pool에 있는 참조가 공유되고 그렇지 않다면 pool에 새로 생성됩니다.
[[Java의 메모리 구조 (Stack, Heap)에 대해 설명해주세요.]]