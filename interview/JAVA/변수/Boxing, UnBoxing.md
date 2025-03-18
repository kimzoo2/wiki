Boxing
-> primitive type을 reference type으로 변경하는 과정

UnBoxing
-> reference type을 primitive type으로 꺼내는 과정

Wrapper Class
- `primitive type`을 `reference type`으로 다룰 수 있게 만든 클래스를 `Wrapper Class`라고 한다. 예를 들어, **Integer, Character** 등이 존재한다.
- JDK 1.5부터 박싱, 언박싱이 필요한 상황에 오토 언박싱, 오토 박싱을 해준다.

### 언제 Wrapper Class 사용?
- 콜렉션 프레임워크가 wrapper class를 사용한다.


장점:  
1. Wrapper 클래스를 사용하면 **null 값을 제공**할 수 있습니다.  
2. [[Generic (제네릭)]] 프로그래밍에서 유용하게 활용할 수 있습니다.  
  
단점:  
1. 성능 저하와 **메모리 낭비가 발생**할 수 있습니다.  (기본 자료형은 스택, 레퍼런스 타입은 힙이기 때문에)
2. 값 비교 시 추가적인 비용이 소요될 수 있습니다.  
  
오토 박싱/언박싱은 Wrapper 클래스와 기본 자료형 간 변환 시 발생하는 것으로, 기본 자료형을 Wrapper 클래스로 바꾸거나 그 반대일 때 발생합니다. 주로 기본 자료형을 Wrapper 클래스로 변환하거나 Wrapper 클래스를 기본 자료형으로 사용할 때 발생합니다.