쓰레드 로컬을 사용하면 **특정 쓰레드에서만 저장된 데이터를 접근**할 수 있다.

### 사용방법
``` java
@Slf4j  
public class ThreadLocalService {  
  
    private ThreadLocal<String> nameStore = new ThreadLocal<>();  
  
    public String logic(String name){  
       log.info("저장 name={} -> nameStore={}", name, nameStore.get());  
       nameStore.set(name);  
       sleep(1000);  
       log.info("조회 nameStore = {}", nameStore.get());  
       return nameStore.get();  
    }  
  
    private void sleep(int mills){  
       try {  
          Thread.sleep(mills);  
       } catch (InterruptedException e) {  
          throw new RuntimeException(e);  
       }  
    }  
}
```

threadLocal 공간에 데이터를 저장한다.
- get() : 쓰레드로컬의 데이터를 조회한다.
- set() : 쓰레드로컬에 데이터를 저장한다.
- remove() : 해당 쓰레드가 쓰레드로컬에 저장한 데이터를 제거한다.

### 주의사항
- 쓰레드 풀과 사용할 때는 주의해야 한다. 쓰레드 로컬의 데이터를 제거하지 않으면 반납했던 쓰레드가 쓰레드 로컬의 데이터를 재사용할 수 있기 때문이다.
- 또한 제거되지 않으면 heap에 남아서 메모리 누수가 발생한다.




https://www.baeldung.com/java-threadlocal