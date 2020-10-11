---
layout: post
title:  "[Java] Effectively final"
date:   2020-10-09 14:58:08 +0900
comments: true
categories: java 
---

- `Java8` 에서 `final` 이 붙지 않은 변수의 값이 변경되지 않는다면 `effectively-final`이라고 한다.
- 다음의 코드에서 `value`는 `effectively-final`이다. 초기화와 동시에 `1`이 할당되고 그 값이 변경되지 않았기 때문이다.

~~~java
int value = 1;
Future<?> submit = Executors.newCachedThreadPool()
                .submit(() -> System.out.println(value));
~~~

- `effectively-final`와 관련된 `lambda expression`이나 `익명 클래스`의 외부 변수 값의 범위에 대해서는 다른 글에서 다뤄야겠다.
