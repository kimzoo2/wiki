
어노테이션을 활용하여 포인트컷을 지정하고 어드바이스를 적용해보자.
정리
- 어노테이션 만들기
- 어드바이저 만들기
- 어드바이저 빈 설정하기

#### LogTrace 만들기

``` java
@Target(ElementType.METHOD)  
@Retention(RetentionPolicy.RUNTIME)  
public @interface Trace {  
}

@Slf4j  
@Aspect  
public class TraceAspect {  
    @Before("@annotation(hello.aop.exam.annotation.Trace)")  
    public void doTrace(JoinPoint joinPoint) {  
       Object[] args = joinPoint.getArgs();  
       log.info ("[trace] {} args = {} ", joinPoint.getSignature(), joinPoint.getArgs());  
    }  
}
```

#### 재시도 로직 만들기
```java
@Target(ElementType.METHOD)  
@Retention(RetentionPolicy.RUNTIME)  
public @interface Retry {  
    int value() default 3;  
}

@Aspect
public class RetryAspect {  
    @Around("@annotation(retry)") // 파라미터에 애노테이션 선언하면 타입 정보가 대체가 된다  
    public Object doRetry(ProceedingJoinPoint joinPoint, Retry retry) throws Throwable {  
       log.info("[retry] {} args= {}", joinPoint.getSignature(), retry);  
       int maxRetry = retry.value();  
       Exception exceptionHolder = null;  
  
       for (int retryCount = 1; retryCount <= maxRetry; retryCount++) {  
          try {  
             log.info("[retry] try count={}/{}", retryCount, maxRetry);  
             return joinPoint.proceed();  
          } catch (Exception e) {  
             // 예외를 잡고  
             exceptionHolder = e;  
          }  
       }  
       // 예외 발생  
       throw exceptionHolder;  
    }  
}
```

#### 파일 설정
```java
@Slf4j  
@SpringBootTest  
@Import({TraceAspect.class, RetryAspect.class})  
class ExamTest {
....
}
```

