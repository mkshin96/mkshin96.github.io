---
layout: post
title:  "@Entity와 @Table의 Name 속성"
date:   2021-03-24 14:58:08 +0900
categories: JPA
comments: true 
---

`@Entity` 의 `name ` 의 경우 `HQL` 또는 `JPQL` 에서 사용하는 Entity의 이름을 변경하지만, `@Table` 의 경우 데이터베이스의 테이블 이름을 변경한다. 예를 들어 다음과 같이 `Order` 클래스에 설정하였을 경우

~~~java
@Entity(name = "ORDERS_ENTITY") @Table(name = "ORDERS_TABLE")
public class Order {
		...
}
~~~

다음과 같이 `JPQL` 을 사용하는 경우 `ORDERS_ENTITY` 로 매핑되지만

```
public interface OrderRepository extends JpaRepository<Order, Long> {

    @Query("select o from ORDERS_ENTITY o where o.id = :id")
    List<Order> findAllById(Long id);
}
```

테이블은 `ORDERS_TABLE` 로 매핑된다.

![name in table](https://github.com/mkshin96/mkshin96.github.io/blob/master/images/name_in_table.png?raw=true)

