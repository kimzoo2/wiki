
첫번째 포인트컷 지시자인 `execution`에 대해서 알아보자.
메소드 실행 조인포인트를 매칭된다. AOP에서 가장 많이 사용하고, 복잡하다.

#### execution 문법
```
execution(modifiers-pattern? ret-type-pattern declaring-type-pattern?name-pattern(param-pattern) throws-pattern?)

execution(접근제어자? 반환타입 선언타입?메서드이름(파라미터 타입) 예외?)
```
- ? 는 생략 가능한 패턴이다.
- `*` 같은 와일드카드를 지정할 수 있다.

execution 안에 많은 조건이 존재하듯, 매칭될 수 있는 포인트도 많다.

- 메서드이름 매칭
- 선언 타입 매칭
- 파라미터 매칭
- 반환 타입 매칭


### 포인트컷
#### 가장 정확한 포인트컷
```java
@Test  
void exactMatch() {  
    // 출력 : public java.lang.String hello.aop.member.MemberServiceImpl.hello(java.lang.String)
        pointcut.setExpression("execution(public String hello.aop.member.MemberServiceImpl.hello(String))");  
    assertThat(pointcut.matches(helloMethod, MemberServiceImpl.class)).isTrue();  
}
```

`helloMethod`는 public java.lang.String hello.aop.member.MemberServiceImpl.hello(java.lang.String) 를 담고 있다.
포인트컷 표현식도 execution(public String hello.aop.member.MemberServiceImpl.hello(String)
로 동일하기 때문에 테스트는 성공한다. (반환 타입과 파라미터 타입은 패키지 생략 가능한듯)

#### 가장 생략된 포인트컷
```java
@Test  
void exactMatch() {  
    // 출력 : public java.lang.String hello.aop.member.MemberServiceImpl.hello(java.lang.String)
        pointcut.setExpression("execution(* *(..))");  
    assertThat(pointcut.matches(helloMethod, MemberServiceImpl.class)).isTrue();  
}
```
- 접근 제어자, 선언 타입, 예외 - 생략
- 반환 타입, 메소드 이름 - `*`
- 파라미터타입 - ..

#### 메소드 이름 매칭 포인트컷
```java
pointcut.setExpression("execution( * hel*(..))"); // hello, heloo, hel22 가능
pointcut.setExpression("execution( * *el*(..))"); // xello, xel, xel1 가능
```

#### 패키지, 선언 타입 매칭 포인트컷
- 