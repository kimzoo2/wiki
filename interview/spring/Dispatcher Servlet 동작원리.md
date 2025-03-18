---
tags:
  - 스프링/MVC
---
## Dispatcher Servlet이란?
Dispatcher Servlet은 **[[프론트 컨트롤러 패턴이란?|프론트컨트롤러]] 역할을 수행하는 클래스**로, 모든 클라이언트의 요청을 처리하고 핸들러에 매핑하는 핵심 컴포넌트입니다.

### 동작원리
1. Client로부터 요청이 **Dispatcher Servlet**으로 전달되면
2. **Handler Mapping**을 통해 요청을 처리할 수 있는 핸들러를 찾습니다. (요청 라우팅)
3. 핸들러를 찾아서 **HandlerAdapter를** 통해 요청을 처리합니다.
4. 핸들러가 반환한 논리적인 뷰 네임을 찾아서 **ViewResolver를** 통해 **View**로 변환됩니다.
5. 그래서 최종 응답을 클라이언트로 전달합니다.

### DispatcherServlet의 장점
- 중앙 집중화되어 공통된 로직을 처리하고 비즈니스 로직을 처리하는 다른 오브젝트에 위임할 수 있습니다.