### 적용하기
`ThreadLocal`을 활용하여 동시성 문제를 해결해보자.

##### `ThreadLocalLogTrace` 개발하기

```java
@Slf4j  
public class ThreadLocalLogTrace implements LogTrace {  
  
    private static final String START_PREFIX = "-->";  
    private static final String COMPLETE_PREFIX = "<--";  
    private static final String EX_PREFIX = "<X-";  
  
    // traceId 동기화, 동시성 이슈 발생  
    private ThreadLocal<TraceId> traceIdHolder = new ThreadLocal<>();  
    @Override  
    public TraceStatus begin(String message) {  
       syncTraceId(); // begin + beginSync  
       TraceId traceId = traceIdHolder.get();  
       Long startTimeMs = System.currentTimeMillis();  
       log.info("[{}] {}{}", traceId.getId(), addSpace(START_PREFIX, traceId.getLevel()), message);  
       // 로그 출력  
       return new TraceStatus(traceId, startTimeMs, message);  
    }  
  
    private void syncTraceId() {  
       TraceId traceId = traceIdHolder.get();  
       if(traceId == null) {  
          traceIdHolder.set(new TraceId()); // 새로 만든다.  
       } else {  
          traceIdHolder.set(traceId.createNextId()); // 동기화하고 현재 정보에 level만 증가한다.  
       }  
    }  
  
    @Override  
    public void end(TraceStatus status) {  
       complete(status, null);  
    }  
  
    @Override  
    public void exception(TraceStatus status, Exception e) {  
       complete(status, e);  
    }  
  
    private void complete(TraceStatus status, Exception e) {  
       Long stopTimeMs = System.currentTimeMillis();  
       Long resultTimeMs = stopTimeMs - status.getStartTimeMs();  
       TraceId traceId = status.getTraceId();  
       if (e == null){  
          log.info("[{}] {}{} time = {}ms", traceId.getId(), addSpace(COMPLETE_PREFIX, traceId.getLevel()), status.getMessage(), resultTimeMs);  
       } else {  
          log.info("[{}] {}{} time = {}ms ex={}", traceId.getId(), addSpace(EX_PREFIX, traceId.getLevel()), status.getMessage(), resultTimeMs, e.toString());  
       }  
       releaseTraceId();  
    }  
  
    private void releaseTraceId() {  
       TraceId traceId = traceIdHolder.get();  
       if(traceId.isFirstLevel()){  
          traceIdHolder.remove(); <- remove 필수
       } else {  
          traceIdHolder.set(traceId.createPreviousId());  
       }  
    }  
  
    private String addSpace(String prefix, int level) {  
       StringBuilder sb = new StringBuilder();  
       for (int i = 0; i < level; i++) {  
          sb.append((i == level -1) ? "|" + prefix : "|   ");  
       }  
       return sb.toString();  
    }  
}
```


##### `ThreadLocal` 적용하기
이미 만들어둔 Configuration을 통해 쉽게(**비즈니스 로직 변경 없이**) 로그 수집기를 변경하자.
```java
@Configuration  
public class LogTraceConfig {  
  
    @Bean  
    public LogTrace logTrace() {  
       return new ThreadLocalLogTrace();  
       // return new FieldLogTrace();  
    }  
}
```

#### 결과
![[스크린샷 2025-01-02 오후 10.44.21.png]]


#### 주의사항
`ThreadLocal` 소개 시 말했던 것처럼 쓰레드풀을 사용하는 환경에서 `ThreadLocal`을 사용하려면 꼭 `remove` 해주어야만 한다.