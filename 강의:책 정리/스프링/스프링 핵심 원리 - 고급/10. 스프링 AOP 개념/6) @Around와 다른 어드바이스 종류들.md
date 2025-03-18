지금까지 `@Around`를 포인트컷을 지정하고 어드바이스를 생성했다. 다른 방식은 없을까?

### 어드바이스 종류
`@Around`: 아래 모든 기능을 포함한다.
`@Before` : joinpoint 직전에 로직을 실행한다.
`@After Returing` : 조인 포인트 **정상 완료** 후 실행
`@After throwing` : 예외가 던져질 때
`@After` : 조인포인트 정상, 예외 상관 없이. finally라고 생각하자.

### 어드바이스 실행 시점

```java
@Slf4j  
@Aspect  
public class AspectV6Advice {  
  
    @Around("hello.aop.order.aop.Pointcuts.orderAndService()") // 조합이 가능하다  
    public Object doTransaction(ProceedingJoinPoint joinPoint) throws Throwable {  
  
       try {  
          // @Before  
          log.info("[트랜잭션 시작] {}", joinPoint.getSignature());  
          Object result = joinPoint.proceed();  
          // @AfterReturning  
          log.info("[트랜잭션 커밋] {}", joinPoint.getSignature());  
          return result;  
       } catch (Exception e){  
          // @AfterThrowing  
          log.info("[트랜잭션 롤백 {}]", joinPoint.getSignature());  
          throw e;  
       } finally {  
          // @After  
          log.info("[리소스 릴리즈]");  
       }  
    }  
  
    @Before("hello.aop.order.aop.Pointcuts.orderAndService()")  
    public void doBefore(JoinPoint joinPoint) { // joinpoint 실행 전에 간단하게 실행하는 방법  
       log.info("[before] {}", joinPoint.getSignature());  
       // joint.proceed() 안 해줘도 됨. 내부적으로 실행해 줌.  
    }  
  
    @AfterReturning(value = "hello.aop.order.aop.Pointcuts.orderAndService()", returning = "result")  
    public void doReturn(JoinPoint joinPoint, Object result){  
       // result을 쓸 수는 있지만 result를 변경할 수는 없음  
       log.info("[return] {} return = {}", joinPoint.getSignature(), result);  
    }  
  
    @AfterThrowing(value = "hello.aop.order.aop.Pointcuts.orderAndService()", throwing = "ex")  
    public void doThrowing(JoinPoint joinPoint, Exception ex){  
       log.info("[ex] {} message = {}", ex, ex.getMessage());  
    }  
  
    @After(value = "hello.aop.order.aop.Pointcuts.orderAndService()")  
    public void doAfter(JoinPoint joinPoint){  
       log.info("[after] {}", joinPoint.getSignature());  
    }  
  
  
}
```

사실 `@Around`만 있어도 모든 위치에 어드바이스를 적용할 수 있다.
하지만 **@Around**는 `ProceedingJoinPoint`를 사용해야만 한다.


`@Before`
- Object result = joinPoint.proceed(); 를 호출하지 않아도 된다. 프레임워크에서 실행해주기 때문이다.

`@After Returing`
- return의 매개변수로 **특정 타입을(ex)String) 지정하면 다른 타입의 응답이 발생했을 때 호출되지 않을 수 있다**.
```java
@AfterReturning(value = "hello.aop.order.aop.Pointcuts.orderAndService()", returning = "result")  
public void doReturn(JoinPoint joinPoint, **String** result){  
    // result을 쓸 수는 있지만 result를 변경할 수는 없음  
    log.info("[return] {} return = {}", joinPoint.getSignature(), result);  
}
```
-> String으로 result를 받지만 실제 타겟 객체의 return 타입이 다르면 호출되지 않는다. (부모 타입으로 지정하면 모든 자식 타입은 인정된다.)

`@After`
- 리소스를 해제하는 데 사용한다.

`@Around`
- 메서드 실행 전후에 작업을 수행한다.
- 어디든 어드바이스를 등록할 수 있기 때문에 아주 강력하다.
- 전달 값, 반환 값, 예외 변환 가능하다.
- try - catch - finally 구문 처리가 가능하다.
- proceed()를 여러번 실행할 수 있다.


#### 실행 순서
- Around, Before, After, AfterReturing, AfterThrowing

##### @Around 외에 다른 어드바이스가 존재하는 이유
```java
@Around("hello.aop.order.aop.Pointcuts.orderAndService()")
public Object doTransaction(ProceedingJoinPoint joinPoint) throws Throwable {  
    log.info("[트랜잭션 시작] {}", joinPoint.getSignature());  
}
```
- `@Around`는 proceed를 호출하지 않으면 **타겟 객체가 호출되지 않는 치명적인 오류가 발생**한다.
- `@Before`나 다른 어드바이스는 **proceed를 호출하지 않기 때문에 실수할 가능성이 낮다**. 또한 이 코드를 작성한 의도가 명확하게 드러난다. -> 제약 덕분에 역할이 명확해진다.