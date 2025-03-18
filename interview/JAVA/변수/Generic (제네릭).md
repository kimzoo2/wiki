## 제네릭이란?
- 내부에서 타입을 지정하지 않고 **외부에서 타입을 지정하게 만드는 클래스**를 의미한다.
특성
- **타입안정성**
	- 제네릭은 컴파일 시 오류를 잡을 수 있다.
- **무변성**
	- [[변성]]은 대체할 수 있는 성질을 말하며 **무변성은 대체할 수 없는 상태를 의미**합니다.
장점
- **타입체크와 형변환**을 생략할 수 있어 **코드가 간결**해진다.

주의점
- **static 멤버 변수**에 T를 선언할 수 없다.
	- static 멤버는 인스턴스를 선언하기 전에 메모리에 올라간다.
	- **메모리에 올라가는 시점에 타입을 알 수 없어서 사용**할 수 없다.
- **제네릭 배열은 불가능**하다.
	- new 연산자를 사용하는 시점에 어떤 타입인지 알아야하기 때문이다.

>위와 같은 문제 때문에 **변성을 지원**하려고 나온 와일드카드


### 공변성과 반변성
- **와일드카드를 통해 공변성과 반변성을 지원**
- 공변성 : 하위 타입이 상위 타입을 대체할 수 있다는 성질
- 반공변성 : 상위 타입이 하위 타입을 대체할 수 있다는 성질
![[스크린샷 2024-12-23 오후 1.22.34.png]]
#### 메소드의 매개변수로서의 제네릭

##### 공변 예시
- 잘못된 예시
```java
public void covariance(List <? extends Number> list){  
    // 하위 타입에 대입하는 것은 불가능하다.  
    // 왜? -> 타입의 불공변성 때문이다.  
    List<Integer> subList = list;  
}
```

- 맞는 예시
```java
public static void main(String[] args) {  
    List<Integer> integers = new ArrayList<>();  
    integers.add(1);  
    covariance(integers);  
}  
  
// 공변 예시  
public static void covariance(List <? extends Number> list){  
    for (Number number : list) {  
       System.out.println(number);  
    }  
}
```
##### 반공변 예시

```java
public static void main(String[] args) {  
  
    List<Number> numbers = new ArrayList<>();  
    numbers.add(1);  
    concovariance(numbers);  
  
}  
  
// 반공변 예시  
public static void concovariance(List <? super Integer> list){  
    list.add(2);  
    list.add(3);  
}
```

### 타입 소거?
- **런타임 단계**에서 타입이 삭제되는 것이 **타입 소거** (**하위 호환성** 때문이다)
- 런타임 때 T는 **Object**로 치환된다.
	- Object로 치환이 되는데 왜 타입소거 때문에 제네릭 인스턴스 생성, 배열 생성이 불가능할까?
```java
// 제네릭 클래스 타입 소거 후
public class Box { 
	private Object item; 
	// T가 Object로 소거됨 
	public void setItem(Object item) { this.item = item; } 
	public Object getItem() { return item; } 
}

// 클래스 선언 타입 소거 시
AS-IS
Box<Integer> boxes = = new ArrayList<>();  

TO-BE
Box boxes = new ArrayList<>();
```
- 인스턴스를 생성하는 경우에는 타입이 소거 되기 때문에 `List<Integer>`는 List로 치환되고 내부 인스턴스 변수는 `Obejct`로 치환된다.

### PESC (Procedure-extends, Super-Consumer)
- 컬렉션으로부터 와일드 타입 객체를 생성하면 extends를 사용해라

## 제네릭 메소드
- **메소드의 선언부에 적은 제네릭**으로 **리턴 타입, 파라미터의 타입**이 정해지는 메소드다.

```java
// 인스턴스 변수
public class Student<T> {
	static T getName (T name) {return name;} // 안 됨
  // 지역 변수
	static <T> T getName (T name){return name;} // 됨
}
```

- 제네릭 메소드는 **호출 시 타입을 지정**하기 때문에 사용이 가능하다.
- 제네릭 메소드에 붙은 `T`는 **지역변수**, 제네릭 클래스에 붙은 `T`는 **인스턴스 변수**로 생각하자.
- 제네릭 메소드의 `T`와 클래스의 `T`는 같은 T여도 다르다.

#### 제네릭 메소드와 제네릭 클래스

```java
public class Student<T> { // 클래스 레벨의 T

	// 메소드 레벨의 T
	public <T> T genericMethod(T item) { 
        return item;
    }
    
	// 클래스 레벨의 T를 사용하는 메소드 
	public T genericMethod(T item) { return item; }
}
```

- 클래스 레벨의 T와 메소드 레벨의 T는 서로 다른 타입이다.
- 메소드 레벨의 T는 메소드를 호출하는 시점에 결정되기 때문이다.

```java
// T의 구체적인 타입이 결정  
public class GenericClass <T> {  
  
    // 클래스 타입의 T가 사용 된다.  
    public void print(T t){  
       System.out.println(t);  
    }  
  
    // 클래스 타입의 T가 사용 된다.  
    public T getT(T t){  
       return t;
    }  
  
    // 제네릭 메소드 : 클래스 타입과는 독립적이다.  
    public <T> T methodGeneric(T item){  
       return item;
    }  
  
}
```

- **클래스의 제네릭 타입은 변수로 저장하지 않아도 된다**. 타입안정성을 위해 타입을 제한하는 것이며 변수를 저장해야 하는 필요성은 없다.