## @Autowired와 @Qualifier의 차이점은?
- `@Autowired`는 등록된 빈을 다른 빈에서 주입 받을 때 사용하는 어노테이션이다.
- `@Qualifier`는 동일한 type의 여러 빈이 등록되어 있을 때, **명시적으로 주입받을 빈을 지정**해주는 어노테이션이다.


`@Primary`와 `@Qualifier` 차이점은?
- @Primary 빈은 동일한 타입의 빈이 여러개 있을 때 **기본값으로 설정하는 빈**이다. 주입 시점에 다른 빈이 필요하다면 @Qualifier를 사용하면 된다.
- @Qualifier가 우선순위가 더 높다.
