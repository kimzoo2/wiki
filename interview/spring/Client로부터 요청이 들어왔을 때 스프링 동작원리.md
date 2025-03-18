1. 클라이언트가 HTTP 요청을 보냅니다.
2. 서블릿 컨테이너가 **Dispatcher Servlet**으로 요청을 전달합니다.
3. **Dispatcher Servlet**이 **HandlerMapping을** 통해 핸들러를 결정하고 핸들러를 실행하기 위한 **HandlerAdapter를** 선택합니다.
4. **HTTP 요청 메세지**를 분석하여 **ArgumentResolver가** **Handler**에 바인딩될 인자로 변환합니다.
5. 핸들러를 실행합니다.
6. viewName이 있는 경우엔 **ViewResolver를** 통해 View를 찾거나
7. **HttpMessageConverter**를 통해 json으로 응답을 변환합니다.
8. 최종 결과를 클라이언트로 전달합니다.