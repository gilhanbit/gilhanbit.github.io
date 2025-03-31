---
title: "[Java] 조건문 정리"
excerpt: "if / if-else / switch 조건문"

categories:
  - Java
tags:
  - [java, condition]

permalink: /java/condition/

toc: true
toc_sticky: true

date: 2025-03-31
last_modified_at: 2025-03-31
---

자바(Java)에서 조건문은 특정 조건에 따라 코드의 흐름을 제어할 때 사용된다.
주로 `if`, `if-else`, `switch` 세 가지가 많이 사용되며, 각각의 특징은 아래와 같다.

<hr>

<h2>1. if문</h2>

<blockquote>
if문은 가장 기본적인 조건문 중 하나로 조건이 `true`일 때만 해당 블록의 코드가 실행된다.
</blockquote>

```java
int num = 10;
if (num > 5) {
System.out.println("num은 5보다 크다.");
}
```

조건이 참일 경우에만 중괄호 `{}` 안의 코드 실행 조건이 거짓이면 아무 일도 일어나지 않는다.

<hr>

<h2>2. if-else문</h2>

<blockquote>
`if` 조건이 `true`일 때와 `false`일 때 각각 다른 코드를 실행하고 싶을 때 사용한다.
</blockquote>

```java
int num = 3;

if (num > 5) {
 System.out.println("num은 5보다 크다.");
} else {
 System.out.println("num은 5보다 작거나 같다.");
}
```

조건이 참이면 `if` 블록 실행 
거짓이면 `else` 블록 실행

<hr>

<h2>3. switch문</h2>

<blockquote>
하나의 변수나 표현식의 값에 따라 여러 가지 경우(case) 중 하나를 선택해 실행할 때 사용된다.
주로 정수, 문자, 문자열 등 명확한 값 비교에 사용.
</blockquote>

```java
int day = 3;

switch (day) {
 case 1: System.out.println("월요일");
 break;
 case 2: System.out.println("화요일");
 break;
 case 3: System.out.println("수요일");
 break;
 default: System.out.println("요일을 알 수 없습니다.");
}
```

`switch` 안의 값과 `case`를 비교해서 일치하면 해당 블록이 출력되며, `break`를 써서 실행이 멈추도록 해야 한다. (안 쓰면 끝까지 다 봄) 
어떤 `case`도 해당되지 않으면 `default` 블록이 실행되며 종료된다.