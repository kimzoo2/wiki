## @Controller와 @RestController의 차이점
- `@ResponseBody`차이이다. @RestController 어노테이션을 확인하면 @Controller 어노테이션이 내장되어 있기 때문에 같은 스테레오 타입 어노테이션이다. 하지만 `@ResponseBody`가 붙어있기 때문에 응답된 자바 객체가 HTTP 응답 body에 작성된다.
- **객체**를 **json 형태로 변경**하여 **HTTP 응답 body**에 작성해주는 어노테이션이다.