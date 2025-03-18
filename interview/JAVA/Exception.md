![[1exception.png]]
- 자바의 Exception은 크게 **Error**와 **Exception**으로 분류된다.

## Error
- **자바 프로그램 밖에서 발생한 예외**를 의미한다. 
- 예를 들어, OutOfMemoryError, StackOverFlowError 등이 존재한다.

## Exception
- Exception은 **checked exception**과 **unchecked exception**으로 나뉜다.

### unchecked Exception (=runtime exception)
- 컴파일러가 예외처리를 강제하지 않는 예외이다. try-catch 없이도 컴파일이 가능하지만 실행 중에 발생하면 해당 쓰레드가 종료될 수 있다. 주로 개발자의 실수에 의해 발생.
- 예외가 발생할 것을 **미리 감지하지 못했을 때 발생**한다. **RuntimeException을 확장한 예외**들이다.
- **nullPointerException** 등이 존재한다.

### 왜 예외처리를 강제 했을까?
- 개발자가 핸들링할 수 없기 때문 아닐까? IOException이나 SQLException은 외부 시스템에서 발생하기 때문에 예외처리를 강제해서 프로그램이 중단되어도 개발자가 조치하게 만듦.

### checked Exception(=compiletime exception)
- 컴파일러가 예외처리를 강제한다. 
- **IOException, SQLException** 등이 존재한다.


### Throwable과 Error, Exception 간의 관계
- Exception과 Error는 **공통 부모**를 갖고 있습니다. Object와 `Throwable` 클래스입니다. Throwable은 클래스로 예외 메세지를 담을 수 있는 생성자를 갖고 있습니다. 그렇기 때문에 Exception이나 Error를 처리할 때 Throwable로 처리해도 무관합니다. 

### 왜 그런 상속 관계?
- 상속 관계가 이렇게 되어 있는 이유는 Exception과 Error가 다루는 오류에 대해 성격을 다르지만, **모두 동일한 이름의 메소드를 사용해서 처리할 수 있도록 하기 위함**입니다.

### Java에서 Error와 Exception 중에서 프로그래머가 직접 처리해야 하는 것과 처리해서는 안 되는 것은 무엇인가요?
- 직접 처리해야 하는 것은 Exception이며 처리해선 안 될 오류는 Error입니다. 그 이유는 각 클래스의 성격에 따릅니다. Exception은 **자바 프로그램 내에서 발생하는 오류**이기 때문에 **핸들링이 가능**하고 Error는 자바 프로그램 밖에서 예를 들어, 서버의 갑작스러운 종료나 메모리 초과 같은 오류가 발생한 것이기 때문에 프로그래머가 직접 처리할 수 없는 오류입니다.


### 예외 처리 전략?