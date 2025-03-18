- 동적 프록시를 만들지 않아도 되고 **프록시 객체를 동적으로 런타임에 생성**해준다.

> JDK는 리플렉션과 **인터페이스를 기반**으로 프록시를 동적으로 만들어준다. 따라서 인터페이스가 필수다.

### InvocationHandler를 구현한 동적 프록시

- `InvocationHandler`는 reflection이 제공하는 인터페이스다. 클라이언트의 모든 요청을 `invocationHandler`에 위임하여 타겟 오브젝트를 호출할 수 있다.
- 모든 요청이 **하나의 핸들러에 집중**되기 때문에 중복을 제거할 수 있다.
![[invocationHandler.jpeg]]

즉, 프록시 객체를 각각 생성하지만 중복되는 부가 기능은 invocationHandler를 통해 구현할 수 있다.


#### 시간 기록 기능 구현
```java
@Slf4j  
public class TimeInvocationHandler implements InvocationHandler {  
    private final Object target; // 구현체가 아니라 오브젝트로 추상화되었다.
  
    public TimeInvocationHandler(Object target) {  
       this.target = target;  
    }  
  
    @Override  
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
       log.info("TimeProxy 실행");  
       long startTime = System.currentTimeMillis();  
  
       Object result = method.invoke(target, args); // 타겟 오브젝트를 호출
  
       long endTime  = System.currentTimeMillis();  
       long resultTime = endTime - startTime;  
       log.info("TimeProxy 종료 resultTime = {}", resultTime);  
       return result;  
    }  
}
```

#### 시간 기록 테스트
```java
@Test  
void dynamicB() {  
    BInterface target = new BImpl();  
    TimeInvocationHandler handler = new TimeInvocationHandler(target);  
  
    // 인수의 의미 -> 프록시 어디에 생성될지? 어떤 인터페이스를 기반으로 만들지? 프록시가 사용할 로직은?  
    BInterface proxyInstance  
       = (BInterface) Proxy.newProxyInstance  
       (  
          BInterface.class.getClassLoader(), // 프록시 어디에 생성될지?  
          new Class[] {BInterface.class}, // 어떤 인터페이스를 기반으로 만들지?  
          handler // 프록시가 사용할 로직은?  
       );  
  
    proxyInstance.call(); // call을 호출하라고 넘겨준 듯  
    log.info("targetClass = {}", target.getClass());  
    log.info("proxyClass = {}", proxyInstance.getClass());  
}
```
- Proxy 객체를 `Proxy.newProxyInstance` 를 통해 생성한다. 이때 인수로 프록시 생성 정보와 handler를 넘겨서 프록시로 호출하게 만든다.

### 로그 추적기에 jdk 적용하기
- jdk는 **인터페이스**에만 프록시 적용이 가능함을 잊지 말자.

#### 부가기능을 적용하는 LogTraceBasicHandler
```java
public class LogTraceBasicHandler implements InvocationHandler {  
  
    private final Object target;  
    private final LogTrace logTrace;  
  
    public LogTraceBasicHandler(Object target, LogTrace logTrace) {  
       this.target = target;  
       this.logTrace = logTrace;  
    }  
  
    @Override  
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
       TraceStatus status = null;  
       try {  
          String message = method.getDeclaringClass().getSimpleName() + "." + method.getName() + "()";  
          status = logTrace.begin(message);  
          // target 호출  
          Object result = method.invoke(target, args);  
          logTrace.end(status);  
          return result;  
       }catch (Exception e){  
          logTrace.exception(status, e);  
          throw e;  
       }  
    }  
}
```

#### 빈 등록 Config
- 빈을 등록할 때 프록시를 생성해서 실제 기능을 수행할 `invocationHandler`를 전달한다.
```java
@Configuration  
public class DynamicProxyBasicConfig {  
  
    @Bean  
    public OrderControllerV1 orderControllerV1(LogTrace logTrace){  
       OrderControllerV1Impl target = new OrderControllerV1Impl(orderServiceV1(logTrace));  
       return (OrderControllerV1)Proxy.newProxyInstance(OrderControllerV1.class.getClassLoader(),  
          new Class[] {OrderControllerV1.class},  
          new LogTraceBasicHandler(target, logTrace)); // 담고 있는 타겟이 다르기 때문에 매번 생성해줘야 함  
    }
```


### 하지만
invocationHandler를 적용하면 모든 메서드에 로그 수집 기능이 추가된다.
no-log인 경우도 그렇다. 이런 no-log인 경우에는 어떻게 제외할 수 있을까?


#### 필터링 기능을 추가한 handler를 등록하자.
```java
public class LogTraceFilterHandler implements InvocationHandler {  
  
    private final Object target;  
    private final LogTrace logTrace;  
    private final String[] parttens;  
  
    public LogTraceFilterHandler(Object target, LogTrace logTrace, String[] parttens) {  
       this.target = target;  
       this.logTrace = logTrace;  
       this.parttens = parttens;  
    }  
  
    @Override  
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
  
       // 메서드 이름 필더  
       String methodName = method.getName();  
       // save, request, reque*, *est 모두 가능  
       if(!PatternMatchUtils.simpleMatch(parttens, methodName)){  
          return method.invoke(target, args);  
       }  
  
       TraceStatus status = null;  
       try {  
          String message = method.getDeclaringClass().getSimpleName() + "." + method.getName() + "()";  
          status = logTrace.begin(message);  
          // target 호출  
          Object result = method.invoke(target, args);  
          logTrace.end(status);  
          return result;  
       }catch (Exception e){  
          logTrace.exception(status, e);  
          throw e;  
       }  
    }  
}
```

리플렉션을 활용(method.getName())하여 **패턴에 매치되지 않은 경우에는 부가기능을 적용하지 않는다**.