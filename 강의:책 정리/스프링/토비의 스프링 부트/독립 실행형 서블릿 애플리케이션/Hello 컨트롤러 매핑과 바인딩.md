### 목표
1. 프론트 컨트롤러를 활용해서 **매핑과 바인딩이 무엇인지 이해**한다.
2. 현재는 서블릿에서 /hello에 대한 요청과 응답을 처리하고 있다. 만들어둔 HelloController를 활용하여 요청과 응답을 처리하도록 관심사를 분리하자.

#### HelloController 스프링 어노테이션 제거
HelloController 객체에 존재했던 스프링 어노테이션을 제거한다.
```java
public class HelloController {  
    public String hello(String name){  
       return "hello " + name;  
    }  
}
```

### 프론트 컨트롤러에서 HelloController 매핑 및 바인딩
프론트 컨트롤러에서 요청에 대한 로직을 수행하지 않고 HelloController에서 사용하도록 변경한다.
1. req에 담겨진 요청 파라미터를 변수화한다.
2. 파라미터를 HelloController에게 전달하여 로직을 수행한다.
3. HelloController로부터 받은 응답을 HTTP 응답으로 활용한다.

```java
public static void main(String[] args) {  
    ServletWebServerFactory serverFactory = new TomcatServletWebServerFactory();  
    // WebServer jetty 등의 유명한 서블릿 컨테이너를 추상화한 객체  
    WebServer webServer = serverFactory.getWebServer(servletContext ->{  
       HelloController helloController = new HelloController();  
       servletContext.addServlet("frontcontroller", new HttpServlet() {  
          @Override  
          protected void service(HttpServletRequest req, HttpServletResponse resp) throws IOException {  
             // 인증, 보안, 다국어, 공통 기능  
             if(req.getRequestURI().equals("/hello") && req.getMethod().equals(HttpMethod.GET.name())){  
                String name = req.getParameter("name");  
  
                // 컨트롤러 호출  
                String ret = helloController.hello(name);  
  
                resp.setStatus(HttpStatus.OK.value());  
                resp.setHeader(HttpHeaders.CONTENT_TYPE, MediaType.TEXT_PLAIN_VALUE);  
                // 컨트롤러 호출한 값을 웹 응답값으로 활용한다.  
                resp.getWriter().println(ret);   
             }  
             else if(req.getRequestURI().equals("/user")){  
                //  
             }else {  
                resp.setStatus(HttpStatus.NOT_FOUND.value());  
             }  
  
          }  
       }).addMapping("/*");  
    });  
    webServer.start();  
}
```


### 매핑이란?
웹 요청 정보를 활용해서 **적절한 오브젝트를 호출하는 작업**이다.

### 바인딩이란?
웹 요청 정보를 받아서 **인자 값으로 넘겨주는 작업**이다.