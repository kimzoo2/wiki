thread-safe 하다는 것은 **멀티 쓰레드** 환경에서 임계영역에 접근하여 **공유자원을 사용**했을 때, **데이터나 자원의 일관성을 보장**하는 상태를 의미합니다.

### 설계 방법
- immutable 데이터 사용 : 불변 객체를 사용하면 데이터가 변경되었을 때 새로운 객체가 생성되기 때문에 데이터 경합을 방지할 수 있습니다.
- 쓰레드 로컬 사용 : 쓰레드 로컬은 쓰레드마다 고유한 저장공간을 보장해준다.
- 동기화 컬렉션 사용 : ConcurrentHashMap과 같은 콜렉션 프레임워크를 사용하여 동시성을 제어한다.
- Atomic 객체 사용
- synchronized 키워드 사용
- volatile 필드 사용
- reentrant lock 사용


https://www.baeldung.com/java-thread-safety