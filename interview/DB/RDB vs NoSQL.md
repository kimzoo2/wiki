## RDB
RDB는 **테이블 형태로 구조화된 관계형 데이터베이스**입니다. 
장점
- 또한 **테이블 간의 관계를 통해 데이터를 관리**합니다. 
- **정규화**로 인해 데이터 중복 최소화
- 트랜잭션을 통해 **데이터의 일관성과 무결성을 보장**할 수 있습니다.
단점
- **scale-out**하기 어렵습니다.

**RDBMS의 한계**
- 스키마 때문에 여러 테이블을 한 번에 변경할 때 시간이 오래 걸린다. null 값 허용되지 않는 컬럼을 추가해야하기도 함.
- scale-out 하기 어렵다.

> 왜 scale-out 하기 어려울까?
> [[트랜잭션과 ACID|ACID]] 속성을 가지는 트랜잭션의 한계 때문이다. 분산 트랜잭션 환경에서 고려해보자.
> 
> A(Atomicity) : 문제가 발생하면 롤백이 되어야 하는데 모든 노드에서 롤백 보장하기 어렵다.
> C(Consitency) : 모든 노드가 일관성을 보장하려면 분산 트랜잭션이 필요하다. 
> I(Isolation) : 여러 노드에서 동시에 실행되면 **동시성 제어 문제**가 발생할 수도 있다.
> D(**Durability**) : 모든 노드에 데이터가 기록되어야 하는데 I/O 병목으로 이어진다.

## NoSQL(Not Only SQL)
NoSQL은 **SQL만 사용하지 않는다는 것**을 의미합니다. 다양한 데이터 모델을 통해 데이터를 관리한다.

**장점** 
- **schemaless**하기 때문에 유연하다. 
- **scale out**하기 쉽다.
**단점**
- **중복 저장 가능성**
	- 중복 저장 되기 때문에 **데이터 변경** 시 관련된 컬렉션들을 모두 수정
- **일관성, 정합성**이 부족할 수 있다.

유형
- **key-value**
	- 대표적으로는 redis가 존재한다.
	- map 형태로 저장하는 데이터를 생각하면 좋다. key-value 형태이기 때문에 데이터를 조회하는데 빠르다.
- **document**

`document`가 무엇인가요?
- key-value 스토어의 확장된 형태다. value에 document를 저장할 수 있는데 구조화된 문서 데이터를 의미한다. xml, json 형태 등을 저장할 수 있다.

### 언제 RDB? 언제 NoSQL?
- NoSQL 
	- 비정형 데이터(json)를 저장하거나 **스키마 변경이 잦은 경우**
	- 확장성이 중요한 소셜 SNS 같은 경우
- RDB 
	- 관계를 맺고 있는 **데이터 변경이 잦은 경우**, 구조화되어 있기 때문에 통계를 사용하는 경우.
	- **일관성이 중요한 은행**, 재고관리 등의 업무

**documented-oriented** NoSQL
- mongoDB가 예시이다.
차이점
- **데이터 모델이 직관적**이다. 즉, 저장한 데이터를 쿼리로 생성하지 않고 객체로 바로 매핑할 수 있다. join이나 정규화 작업이 필요없기 때문에 보다 직관적이다.
- **가독성**이 좋다. Json 형태로 저장할 수 있기 때문에 key, value 형태로 데이터를 제공할 수 있다. 개발자는 보다 쉽게 데이터를 파악할 수 있다.
- **스키마가 유연**하다. 스키마는 동적이기 때문에 먼저 DB에 미리 정의할 필요가 없다. 

https://www.mongodb.com/resources/basics/databases/document-databases

### CAP 이론
CAP Theorem은 **Database가** `Consistency`, `Availability`, `Partitioning를` 모두 만족할 수 없고, 둘만 만족할 수 있다는  것이다.

• Consistency(일관성): 모든 client가 언제가 같은 상태의 데이터를 바라볼 수 있다.
• Availability(가용성): 모든 요청에 항상 응답을 제공해야만 한다.
• Partition Tolerance(파티션 내성): 네트워크 분할이 발생해도 시스템이 동작한다.
클러스터링 노드 간에 **통신하는 네트워크가 장애**가 나더라도 정상적으로 서비스를 수행한다. 노드 간 물리적으로 전혀 다른 네트워크공간에 위치도 가능하다.

RDBMS는 `Consistency`를 강하게 보장하려고 하고 NoSQL은 `Availability과` `Partition Tolerance`를 보장하려고 한다.

### 왜 두가지만 될까?
- 분산 시스템의 근본적인 한계 때문이다. 일관성과 가용성 중 하나를 희생해야만 한다. 분산 시스템에선 네트워크 분할이 일어날 수 있기 때문에 일관성과 가용성을 동시에 보장하기 어렵다.

### 하지만 일관성을 보장하지 않으면 어떻게 scale-out?
- 일관성을 보장하지 않으면 scale-out해도 불완전한 데이터를 조회할 수 밖에 없다. 그렇기 때문에 `eventually consistency`(최종적 일관성)을 통해 일관성을 일시적으로 포기하면 결과적으로 일관성을 보장하는 형태로 나아간다.

https://sjh836.tistory.com/97

https://blog.naver.com/PostView.nhn?blogId=naverdev&logNo=120116323974&parentCategoryNo=8&viewDate=&currentPage=1&listtype=0