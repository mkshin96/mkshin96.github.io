---
layout: post
title:  "Spring @Transactional propagation 옵션"
date:   2021-04-28 14:58:08 +0900
categories: Spring
comments: true 
---

`Spring`에서 선언적 트랜잭션이라 불리는 ` @Transactional` 의 여러 가지 옵션 중 `propagation`에 대해 간단하게 정리하려고 한다.

1. `PROPAGATION_REQUIRED` (**Default**)
   - 트랜잭션이 실행중이라면 해당 트랜잭션에 참여한다. 없다면 트랜잭션을 생성한다.
2. `PROPAGATION_MANDATORY`
   - 트랜잭션이 실행중이라면 해당 트랜잭션에 참여한다. 없다면 exception을 발생시킨다.
3. `PROPAGATION_NEVER`
   - 트랜잭션이 실행중이어도 참여하지 않고, 만약 트랜잭션이 있다면 exception을 발생시킨다.
4. `PROPAGATION_NOT_SUPPORTED`
   - 트랜잭션이 실행중이어도 참여하지 않고, 언제나 `non-transactionally`하게 실행된다.
5. `PROPAGATION_REQUIRES_NEW`
   - 트랜잭션을 생성한다. 만약 기존에 트랜잭션이 있다면 중지시킨다.
6. `PROPAGATION_SUPPORTS`
   - 트랜잭션이 실행중이라면 해당 트랙잭션에 참여한다. 만약 실행중인 트랜잭션이 없다면 `non-transactionally` 하게 실행된다.
7. `PROPAGATION_NETSED`
   - 트랜잭션이 있다면 `nested transaction`을 생성한다. 만약 실행중인 트랜잭션이 없다면 `PROPAGATION_REQUIRED`처럼 동작한다.

