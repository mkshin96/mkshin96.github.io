---
layout: post
title:  "[Java] volatile"
date:   2020-10-09 14:58:08 +0900
categories: java 
---

- `volatile` keyword는 Java 변수를 Main Memory에 저장하겠다 라는 것을 명시하는 것이다.
- 매번 변수의 값을 Read할 때마다 CPU cache에 저장된 값이 아닌 Main Memory에서 읽는 것이다.
- 또한 변수의 값을 Write할 때마다 Main Memory에 까지 작성하는 것이다.



### 왜 필요할까?

- `volatile`변수를 사용하고 있지 않은 Multi Thread 애플리케이션에서는 Task를 수행하는 동안 **성능 향상** 을 위해 Main Memory에서 읽은 변수 값을 CPU cache에 저장하게 된다.
- 만약에 Multi Thread환경에서 Thread가 변수 값을 읽어올 때 각각의 CPU cache에 저장된 값이 다르면 변수 값 불일치 문제가 발생하게 된다.



### 예제

- `RaceCondition` 을 공유하고 있는 두 개의 Thread가 있다.
  - Thread-1은 `count`의 값을 더하고 읽는 연산을 한다. (Read & Write)
  - Thread-2는 `count`값을 읽기만 한다.(Only Read)

~~~java
public class RaceCondition {
		public int count = 0;
}
~~~

- Thread-1은 count값을 증가시키고 있지만 CPU cache에만 반영되어 있고, 실제로 Main Memory에는 반영이 되지 않았다. 
- 그렇기 때문에 Thread-2는 count값을 계속 읽어오지만 0을 가져오는 문제가 발생한다.



### 어떻게 해결할까?

- `volatile` 키워드를 사용하게 되면 CPU cache가 아닌 Main Memory에 저장하고 읽어오기 때문에 **변수 값 불일치 문제** 를 해결할 수 있다.

~~~java
public class RaceCondition {
    public volatile int count = 0;
}
~~~



### 언제 volatile이 적합할까?

- Multi Thread 환경에서 하나의 Thread만 read & write하고 나머지 Thread가 read하는 상황에서 가장 최신의 값을 보장한다.



### 항상 volatile을 사용하는 것이 옳을까?

- Thread-1이 값을 읽어 1을 추가하는 연산을 진행한다.
  - 추가하는 연산을 했지만 아직 Main Memory에 반영되기 전이다.
- 이 때, Thread-2가 값을 읽어 1을 추가하는 연산을 진행한다.
- 두 개의 Thread가 1을 추가하는 연산을 하여 결과가 2여야 하지만 각각 결과를 Main Memory에 반영하게 된다면 결과가 1인 상황이 발생한다.
- 이를 보아 하나의 Thread가 아닌 **여러 Thread**가 write하는 상황에서는 적합하지 않다.
- 여러 Thread가 write하는 상황이라면 `synchronized` 를 통해 변수 read & write의 원자성(atomic)을 보장해야 한다.



### 성능에는 어떤 영향이 있을까?

- volatile은 변수의 read나 write를 Main Memory에서 진행하게 된다.
- CPU cache보다 Main Memory가 비용이 더 크기 때문에 변수 값 일치를 보장해야 하는 경우에만 `volatile` 을 사용해야 한다.



### 정리

- `volatile`	
  - Main Memory에 read&write를 보장하는 키워드
- Best Practice
  - 하나의 Thread가 write하고 나머지 Thread가 읽는 상황인 경우
  - 변수의 값을 최신의 값으로 읽어와야 하는 경우
- 주의할 점
  - 성능에 영향을 끼칠 수 있음



### Reference

- http://tutorials.jenkov.com/java-concurrency/volatile.html
- https://nesoy.github.io/articles/2018-06/Java-volatile