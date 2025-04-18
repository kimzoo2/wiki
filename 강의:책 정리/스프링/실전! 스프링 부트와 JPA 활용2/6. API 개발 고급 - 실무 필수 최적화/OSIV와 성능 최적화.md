### OSIV(Open Session In View)
언제 JDBC가 데이터베이스 커넥션을 가져올까?
- 데이터베이스 트랜잭션이 시작할 때 영속성 컨텍스트가 커넥션을 가져온다.
그럼 언제 데이터베이스 커넥션을 반납할까?
- osiv가 켜져있으면 Controller까지 커넥션을 반납하지 않는다.
- 즉, API면 응답을 줄 때까지 뷰면 렌더링을 끝낼 때까지 커넥션을 반납하지 않는다.

왜? -> 컨트롤러나 View에서 `lazyLoading` 해야 하니까.

OSIV는 너무 오랜시간 동안 커넥션 리소스를 사용하기 때문에 **커넥션이 모자라고 결국 장애로 이어질 수 있다**.

#### OSIV를 끄면?
- 모든 지연로딩을 트랜잭션 안에서 처리해야 한다. 즉, 외부에서 지연로딩하고 있던 경우 트랜잭션 안으로 포함하거나 fetchJoin을 통해 모든 데이터를 응답하면 된다.

영한님은 **실시간 트래픽이 많이 몰리는 실시간 API는 osiv를 끄고**, ADMIN처럼 **커넥션을 많이 사용하지 않는 곳은 osiv를 켠다**고 한다.