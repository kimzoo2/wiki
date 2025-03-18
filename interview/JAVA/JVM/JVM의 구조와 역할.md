## JVM이란?
- 자바 버츄얼 머신으로 **여러 운영체제에서 java application을 런타임으로 실행**해주는 도구다.
![[jvm.png]]
개발자가 작성한 java 파일을 컴파일러가 변환하면 **바이트 파일**이 생성된다. 바이트 파일을 여러 운영체제에 맞게 실행하는 것이 **jvm**이다.


https://www.turing.com/kb/java-virtual-machine
https://www.geeksforgeeks.org/jvm-works-jvm-architecture/
https://www.freecodecamp.org/news/jvm-tutorial-java-virtual-machine-architecture-explained-for-beginners/

![jvm](https://www.freecodecamp.org/news/content/images/2021/01/image-39.png)


# JVM의 3요소
- **클래스로더, 런타임 데이터 영역, 실행 엔진**

## Class Loader
- **바이트 코드를 읽어서** 클래스 정보를 **등록, 검증**하고 **핵심 API를 로드**한다.
![](https://www.freecodecamp.org/news/content/images/2021/01/image-40.png)

### ClassLoader 종류
- **부트스트랩 클래스 로더** : 루트 클래스 로더로 `java.lang`, `java.util`, `java.io` 등 핵심 API를 로드한다.
- **확장 클래스로더** : **표준 자바 라이브러리**를 로드한다.
- **애플리케이션 클래스 로더** : 지정한 루트 패스부터 등록된 클래스들을 로드한다.

**만약 클래스로더 찾을 수 없는 클래스라면 어떻게 되나요?**
- `ClassNotFoundException` 또는 `NoClassDefFoundError` 오류가 발생한다.

### 역할
- **바이트 코드 로딩**
- **링킹 (검증)** : 메모리에 **로드된 클래스를 연결하고 검증**하고 정적 필드에 대한 **메모리를 기본값으로 초기화**한다.
- **초기화** : 링킹 단계에서 초기값으로 선언되면 이 단계에서 사용자가 실제 선언한 변수 값이 들어간다.

```
ex)
private static final boolean enabled = true;

-- linking
-> 초기값인 false로 값이 세팅된다.

-- initial
-> 사용자 설정 값인 true로 값이 세팅된다.
```

## Runtime Data area
- 총 다섯가지 영역이 존재한다.

### 종류
- method area : 클래스 정보 및 **정적 데이터**가 저장되는 공간, **쓰레드 간에 공유**된다.
- stack : 모든 로컬 변수가 저장되는 공간, 쓰레드 당 하나씩 생성된다.
- heap : 레퍼런스 변수가 저장되는 공간, 쓰레드 간에 공유된다.
- native method area :  Java가 아닌 다른 프로그래밍 언어로 작성된 메서드를 네이티브 메서드라고 한다. 이러한 메서드는 바이트 코드로 변환되지 않기 때문에 별도의 공간이 필요하다.
- pc register : 스레드의 **현재 실행 명령 주소**를 저장한다. 

## Execution Engine
- 바이트 코드를 실행한다.
- **interpreter, jit compiler, GC**가 존재한다.
### 종류
#### interpreter
- 라인 별로 해석한 뒤 실행하는 도구, 라인 별로 해석하기 때문에 수행 속도가 느리다.
#### jit(just in time) compiler
- interpreter의 단점을 보완하는 도구, 반복으로 해석되는 코드에 대해 미리 컴파일하여 로딩 속도를 줄이는 역할을 한다.
#### GC
- 더이상 사용되지 않는 객체를 해제하는 도구다. stop the world, mark and sweep 등으로 메모리를 관리한다.