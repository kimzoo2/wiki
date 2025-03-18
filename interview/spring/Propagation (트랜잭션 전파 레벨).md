---
tags:
  - 스프링
---
### 트랜잭션 전파 : Propagation

- **트랜잭션을 시작하거나 기존 트랜잭션에 참여하는 방법을 결정**하는 속성
- **REQUIRED** : 있으면 참여하고 없으면 시작한다.
- SUPPORTS : 시작된 트랜잭션이 있으면 참여하고 그렇지 않으면 트랜잭션 없이 진행한다.
- MANDATORY : 시작된 게 있으면 참여하고 없으면 예외를 발생시킨다. 혼자 발생하면 안 될 때 씀
- **REQUIRED_NEW** : 항상 독립적인 트랜잭션을 생성한다
- **NOT_SUPPORTED** : 트랜잭션 사용하지 않는다. 이미 진행 중인 트랜잭션 있으면 보류한다.
- NEVER : 트랜잭션 사용하지 않도록 강제한다. 이미 진행 중인 트랜잭션도 있으면 안 된다.
- NESTED : 중첩 트랜잭션을 시작한다.

https://wonyong-jang.github.io/spring/2020/03/21/Spring-Transaction-Propagation.html?utm_source=chatgpt.com