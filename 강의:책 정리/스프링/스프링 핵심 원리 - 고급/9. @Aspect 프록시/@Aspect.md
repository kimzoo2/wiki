
## @Aspect
`Aspectj`에서 제공하는 애노테이션이다. 스프링은 `@Aspect` 애노테이션을 활용해서 어드바이저를 편리하게 구현할 수 있게 만든다.

### 어노테이션 기반 aop 적용해보기
#### LogTraceAspect 적용하기

```java
@Aspect // 애노테이션 기반 프록시  
public class LogTraceAspect {  
  
    private final LogTrace logTrace;  
  
    public LogTraceAspect(LogTrace logTrace) {  
       this.logTrace = logTrace;  
    }  
  
    @Around("execution(* hello.proxy.app..*(..))") // 포인트컷  
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {  
       // 어드바이스  
       TraceStatus status = null;  
       try {  
          String message = joinPoint.getSignature().toShortString();  
          status = logTrace.begin(message);  
          // target 호출  
          Object result = joinPoint.proceed(); // target 호출한다.
          logTrace.end(status);  
          return result;  
       }catch (Exception e){  
          logTrace.exception(status, e);  
          throw e;  
       }  
    }  
}
```

- `@Aspect`를 활용하면 **어드바이저를 생성**할 수 있다. 자동 프록시 생성기가 `@Aspect` 어노테이션이 선언된 클래스를 스캔하기 때문이다.
- `@Around`를 활용해서 **포인트컷을 적용**한다.
- execute 메소드와 `ProceedingJoinPoint`를 활용해서 **어드바이스를 구현**할 수 있다.
	- `ProceedingJoinPoint`는 [[5) 프록시 팩토리로 로그 수집기 적용#^3d61b1|MethodInvocation]]의 `invocation`과 유사한 기능이다.

### @Aspect 프록시란
- 앞서 말한 것처럼 어노테이션을 활용한 어드바이저 생성 방법이다.
- 자동 프록시 생성기(AnnotationAwareAspectJAutoProxyCreator)는 **Advisor를 자동으로 찾아서 프록시를 생성**한다고 했다. - [[2) 스프링이 사용하는 빈 후처리기#^3395c1|관련 페이지]]
- 자동 프록시 생성기는 하나의 역할을 더 한다. 총 두가지의 역할을 한다.
	- 1. `Advisor`를 오토 스캐닝하여 프록시 생성![[스크린샷 2025-01-27 오전 11.12.07.png]]
	- 2. `@Aspect`가 붙은 클래스 찾아서 `Advisor`로 생성![[스크린샷 2025-01-27 오전 11.12.32.png]]
즉, 자동 프록시 생성기는 2) 어드바이저 빌더를 통해 어드바이저를 생성하고 1) 어드바이저 빌더에 저장된 어드바이저를 스캔한다.
- 어드바이저 빌더는 개념만 알고 있으면 된다.


스프링은 `@Aspect`를 활용해서 **프록시를 적용**한다! 이제 스프링의 aop에 대해 본격적으로 알아보자.