JPA의 기본키 생성전략을 크게 두가지로 나뉜다.
- **직접 할당하는 방식** 
- **자동으로 생성하는 방식**

#### 직접 할당 방식
- Application에서 기본 키를 직접 할당하는 방식이다.
- 키 컬럼에 `@Id` 만 붙이면 된다.

```java
@Id
private long id;
```

**붙을 수 있는 타입**
- `Date`, `primitive` 타입, `wrapper` 타입, `String`
- `BigDecimal`, `BigInteger`

#### 자동 생성 방식
- DB를 활용하여 기본 키를 생성하는 방식이다.
- `@Id`와 `@GeneratedValue` 조합하여 기본 키가 할당된다.
- `IDENTITY`, `SEQUENCE`, `TABLE`, `AUTO` 방식이 존재한다.


#### IDENTITY
- `AutoIncrement`를 통해 DB에 기본 키 생성을 위임하는 방식이다.
- DB에 **저장해야만 식별자가 생성**된다는 특징이 있다.

장점
- 별도 테이블이나 시퀀스 설정할 필요가 없음
단점
- 영속성 컨텍스트에 저장하기 전에 id를 알 수 없음

#### SEQUENCE
- **DB Sequence**를 사용해 기본 키 생성을 위임하는 방식이다.
- oracle, h2 DB 등에서 자주 사용한다.

• **장점**:
	• 미리 ID를 생성하므로 **영속성 컨텍스트에 저장하기 전에 ID를 알 수 있음**.
	• 성능 튜닝 가능(예: 시퀀스 캐싱).

• **단점**:
	• 시퀀스를 지원하지 않는 데이터베이스에서는 사용할 수 없음.

#### TABLE
- 키를 생성하는 전용 테이블을 만들어 기본 키 생성을 위임하는 방식이다.

• **장점**:
- 시퀀스나 AUTO_INCREMENT를 지원하지 않는 데이터베이스에서도 사용 가능.

• **단점**:
- 별도의 테이블을 사용하므로 성능이 떨어질 수 있음.

#### AUTO
- **DB 방언에 따라 자동으로 `IDENTITY`, `SEQUENCE`, `TABLE` 전략을 자동으로 선택**한다.
### IDENTITY vs SEQUENCE
- identity는 데이터를 삽입할 때 pk가 생성되기 때문에 영속화 단계에서 데이터가 삽입된다.
- sequence는 영속화(persist) 단계에서 db에 저장된 sequence를 조회하여 쓰기 지연이 가능해진다.