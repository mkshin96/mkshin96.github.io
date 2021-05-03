---
layout: post
title: "Spring @Transactional(readOnly=true)"
date: 2021-05-03 14:58:08 +0900
categories: Spring
comments: true 

---

Spring MVC를 사용할 때 조회만 발생하는 트랜잭션의 경우 `@Transactional` 의 `readOnly` 옵션을 `true` 로 주면 성능이 최적화된다는 사실을 익히 알고 있을 것이다. 어떻게 동작하는지 궁금해 조금 찾아보았다.

~~~text
public abstract boolean readOnly
A boolean flag that can be set to true if the transaction is effectively read-only, allowing for corresponding optimizations at runtime.
Defaults to false.

This just serves as a hint for the actual transaction subsystem; it will not necessarily cause failure of write access attempts. A transaction manager which cannot interpret the read-only hint will not throw an exception when asked for a read-only transaction but rather silently ignore the hint.

See Also:
TransactionDefinition.isReadOnly(), TransactionSynchronizationManager.isCurrentTransactionReadOnly()
Default:
false

~~~

Spring 문서에 따르면 해당 옵션은 실제 transaction을 발생시키는 서브시스템에게 힌트만 제공한다고 되어있다. 그렇다면 하이버네이트 세션(JPA의 `EntityManager`의 구현체)에서는 `@Transactional(readOnly=true)`일 경우 어떻게 조회를 최적화 할까. 자바 ORM 표준 JPA 프로그래밍에서는 다음과 같이 설명한다.

~~~
트랜잭션에 `readOnly=true` 옵션을 주면 스프링 프레임워크는 하이버네이트 세션의 플러시 모드를 `MANUAL`로 설정한다. 이렇게 하면 강제로 플러시를 호출하지 않는 한 플러시가 일어나지 않는다. 따라서 트랜잭션을 커밋해도 영속성 컨텍스트를 플러시하지 않는다. 영속성 컨텍스트를 플러시하지 않으니 엔티티의 등록, 수정, 삭제는 동작하지 않는다. 하지만 플러시할 때 일어나는 스냅샷 비교와 같은 무거운 로직들을 수행하지 않으므로 성능이 향상된다. 물론 트랜잭션을 시작했으므로 트랜잭션 시작, 로직수행, 트랜잭션 커밋의 과정은 이루어진다. 단지 영속성 컨텍스트를 플러시하지 않을 뿐이다.
~~~

다음에는 쿼리 힌트 중 하이버네이트 전용 힌트인 `org.hibernate.readOnly` 옵션에 대해 공부해봐야겠다. 아직은 해당 옵션은 '스냅샷을 생성하지 않아, 메모리 사용량을 최적화할 수 있다.' 정도로만 알고 있다. :(



### 참고

- https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Transactional.html#readOnly--

- [**자바 ORM 표준 JPA 프로그래밍**](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788960777330)

