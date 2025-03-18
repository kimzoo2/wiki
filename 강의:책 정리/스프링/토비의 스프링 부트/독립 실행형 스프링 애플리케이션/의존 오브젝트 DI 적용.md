### 목표
- 객체에 선언해둔 구현체를 제거하고 외부에서 의존 관계를 주입하도록 변경한다.
- 구현체는 인터페이스로 변경하여 확장성을 고려한다.


#### 의존 오브젝트 DI를 적용한다.
```java
public class SimpleHelloService implements HelloService {  
    @Override  
    public String sayHello(String name){  
       return "Hello " + name;  
    }  
}
```

```java
// 스프링 컨테이너 생성  
GenericApplicationContext applicationContext = new GenericApplicationContext();  
// 빈 등록  
applicationContext.registerBean(HelloController.class);
applicationContext.registerBean(SimpleHelloService.class);
```

- **순서를 고려해야 하지는 않을까?**
	- 고려하지 않아도 된다. 스프링 컨테이너가 내부적으로 빈의 생성 순서를 결정한다.
- **빈만 등록하면 되나? 의존 관계 주입은 필요 없나?**
	- 의존 관계를 주입하지 않아도 된다. 스프링 컨테이너가 내부적으로 HelloService를 구현한 구현체를 찾아서 등록된 빈을 주입한다.