### Redis 설치하기
https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-on-mac-os/

```linux
brew --version
brew install redis
brew services start redis
brew services info redis
brew services stop redis
```

### Redis 연결 확인하기
```linux
redis-cli
```

**연결 정상 여부 테스트**
```linux
ping
```

**결과**
```linux
127.0.0.1:6379> ping
PONG
```


### Redis 기본 명령어 익히기
레디스는 **다양한 자료 구조**와 **명령어가 존재**한다.

### set - 저장하기
```shell
set [key] [value]

set kimzoo:name "juhee Kim"
set kimzoo:hobby game
```

### get - 조회하기
```shell
get [key]

get kimzoo:name
```

### del - 삭제하기
```shell
del [key]
del kimzoo:hobby
```

### TTL 지정하기
```shell
set [key] [value] ex(=expire) seconds

set kimzoo:name "juhee Kim" ex 30
- kimzoo의 이름은 juhee Kim이며 30초 뒤에 만료된다
```

### ttl - TTL 확인하기
```shell
ttl [key]
결과
양수 : 존재하는 경우
-1 : 저장되었으나 ttl이 지정되지 않은 키
-2 : 만료되거나 저장되어 있지 않은 키

ttl kimzoo:name 
ttl kimzoo:hobby
```

### keys - key 값 모두 조회하기
```shell
keys *
```

### flushall - key 값 모두 삭제하기
```shell
flushall
```
flush all이라는 뜻이다.


### Redis에서 Key 네이밍 컨벤션 익히기
- 콜론(:)을 활용해서 계층적으로 의미를 구분해서 사용한다.

ex)
- `users:100:profile` : 사용자들(users) 중에서 pk가 100인 프로필(profile)을 조회한다.
- `products:123:details` : 상품들(products) 중에서 pk가 123인 상세 정보들(details)을 조회한다.

