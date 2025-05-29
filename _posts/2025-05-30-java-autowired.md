---
title: "[Java] @Autowired, @RequiredArgsConstructor 차이"
excerpt: "@RequiredArgsConstructor를 권장하는 이유"

categories:
  - Java
tags:
  - [java, autowired, requiredargsconstructor]

permalink: /java/autowired/

toc: true
toc_sticky: true

date: 2025-05-29
last_modified_at: 2025-05-29
---

Spring에는 Bean DI를 지원하는 대표적인 어노테이션인 @Autowired와 @RequiredArgsConstructor가 있다.

그러나 많은 사람들이 @Autowired대신 @RequiredArgsConstructor를 권장하는데 그 이유는 무엇일까?

먼저, 둘의 차이부터 알아보자.

<hr>

## 1. @Autowired

>@Autowired를 활용한 DI를 필드 주입이라고 한다.

```java
public class MyService {

    @Autowired
    private UserRepository userRepository;
}
```

이 Annotation을 특정 필드에 부여하면 Spring IoC Container 안에 존재하는 해당 Type의 Bean을 찾아 자동 주입해준다.

위 코드를 해석하자면 Spring IoC Container가 관리하고 있는 UserRepository 타입의 Bean이 userRepository 매개변수에 주입되는 것이다.

<hr>

## 2. @RequiredArgsConstructor

>@RequiredArgsConstructor를 활용한 DI를 생성자 주입이라고 한다.

```java
@RequiredArgsConstructor
public class MyService {

    private final UserRepository userRepository;
}
```

해당 Class의 생성자를 자동으로 만들어주는 lombok Annotation이다.

private final 접근제어자로 선언된 멤버변수를 생성자 파라미터로 넣어준다.

<hr>

## 3. @RequiredArgsConstructor를 권장하는 이유

둘 다 똑같이 스프링 컨테이너가 관리하는 **빈(bean)**을 주입받기 위해 사용된다.

즉, 외부에서 객체를 생성하지 않고 스프링이 알아서 생성해 주입하도록 하는 방식이라는 건 같은데...

어째서 @RequiredArgsConstructor를 권장할까?

1. 순환참조 에러 방지
2. 안정적 (final로 선언된 필드는 객체 생성 후 변경 불가)
3. 코드작성 용이 (매번 @Autowired를 쓸 필요 없음)

* 순환참조란 A가 B를 필요로 하고, 동시에 B도 A를 필요로 하는 상황을 말한다.

```java
public class A {

  @Autowired
  private B b;
}

public class B {

  @Autowired
  private A a;
}
```
