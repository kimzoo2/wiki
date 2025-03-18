### 목표
- 프론트 컨트롤러 패턴으로 구현되어 있는 `DispatcherServlet`으로 전환해보자.

#### 기존 servlet 제거
``` java
WebServer webServer = serverFactory.getWebServer(servletContext ->{  
    servletContext.addServlet(""
    ).addMapping("/*");  
});  
webServer.start();
```
- webServer에 등록된 기존 서블릿을 제거한다. 이후 DispatcherServlet으로 전환한다.

#### DispatcherServlet으로 전환
```java
GenericWebApplicationContext applicationContext = new GenericWebApplicationContext();

WebServer webServer = serverFactory.getWebServer(servletContext ->{  
    servletContext.addServlet("dispatcherServlet",  
       new DispatcherServlet(applicationContext) // web에 특화된 applicationContext를 사용한다.
  
    ).addMapping("/*");  
});
```

- web에 특화된 `applicationContext`를 주입해준다.
- 위와 같이 등록한다면 알아서 bean을 등록하고 호출까지 해줄까? 
	- 아니다. 그 작업을 이해 어노테이션 매핑 등으로 해결해보자.