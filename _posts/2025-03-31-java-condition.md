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

**자바(Java)에서 조건문은 특정 조건에 따라 코드의 흐름을 제어할 때 사용된다.**
주로 `if`, `if-else`, `switch` 세 가지가 많이 사용되며, 각각의 특징은 아래와 같다.

<hr>

## 1. if문

>if문은 가장 기본적인 조건문 중 하나로 조건이 `true`일 때만 해당 블록의 코드가 실행된다.

```java
int num = 10;
if (num > 5) {
System.out.println("num은 5보다 크다.");
}
```

조건이 참일 경우에만 중괄호 `{}` 안의 코드 실행 조건이 거짓이면 아무 일도 일어나지 않는다.

<hr>

## 2. if-else문

>`if` 조건이 `true`일 때와 `false`일 때 각각 다른 코드를 실행하고 싶을 때 사용한다.

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

## 3. switch문

>하나의 변수나 표현식의 값에 따라 여러 가지 경우(case) 중 하나를 선택해 실행할 때 사용된다.
**주로 정수, 문자, 문자열 등 명확한 값 비교에 사용.**

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

<hr>

## 4. 연습문제

![문제](/assets/images/posts_img/condition/quiz.png)
  
위 조건을 보면 100으로 나누어 떨어지는 연도는 윤년이 아니지만 4로 나누어 떨어지는 연도는 윤년이라고 되어있다.

그러나 100으로 나누어 떨어지는 연도는 무조건 4로도 나누어 떨어지므로 둘을 구분해서 조건을 걸어주어야 한다.

그리고 400으로 나누어 떨어지는 수는 100으로 나누었을 때도 떨어지므로 이 또한 구분해 조건을 걸어야 한다.

```java
int year = input값;

if (year % 4 == 0) {
  System.out.println("윤년");
} else if (year % 100 == 0) {
  System.out.println("평년");
} else if (year % 400 == 0) {
  System.out.println("윤년");
} else {
  System.out.println("평년");
}
```
  
쉽게 위처럼 코드를 짤 수 있을텐데, 위에서 부터 검토해 본다면 결과가 `참`이 아니라는 걸 알 수 있다.

**왜일까?**

`year`값이 `400`이라고 가정한다면 첫 번째 if에서 참이므로 `윤년`이라는 결과가 나온다.

그러나 두 번째 else if가 첫 번째에 위치해 있었다면?

`year`값은 100으로도 나누어 떨어지므로, 이 역시 참으로 `평년`의 결과가 나온다.

여기서 중요한 건, **같은 값임에도 코드 순서에 따라 다른 결과가 나온다는 것**이다.

다시 한번 문제를 들여다보자.

{: .notice--info}
>1. 4로 나누어 떨어지는 연도는 윤년이다.
>2. 100으로 나누어 떨어지는 연도는 윤년이 아니다.
>3. 400으로 나누어 떨어지는 연도는 무조건 윤년이다.

조건에 부합하는 결과를 얻기 위해서는 좁은(특정) 조건을 위에 올려줘야 한다.

{: .notice--info}
>1. **무조건**이라는 말이 붙은 좁은 범위의 조건인 3번이 가장 상위에 있어야 한다.
>2. 100으로 나누어 떨어졌을 때는 윤년이 아닌 평년이여야 하므로 이 조건에 두 번째에 위치해야 한다.
>3. 가장 넓은 조건인 1번이 마지막에 위치해야 한다.

```java
int year = input값;

if (year % 400 == 0) {
  System.out.println("윤년");
} else if (year % 100 == 0) {
  System.out.println("평년");
} else if (year % 4 == 0) {
  System.out.println("윤년");
} else {
  System.out.println("평년");
}
```

위 순서로 코드를 짜야지만 정확한 결과를 얻을 수 있다.

하지만, 여기서 끝이 아니다.

원하는 결과가 나온다는 걸 확인했으면, 이제 **어떻게 하면 더 효율적으로 코드를 짤 수 있을까?** 하는 생각을 해야한다.

문제를 다시 한번 생각해보면 특정 조건으로 윤년과 평년으로 구분하는 문제인데, 걸어주는 조건을 모두 **윤년**인 조건으로 만들면 조금 더 효율적으로 코드를 짤 수 있을 것 같다.

즉, 2번 조건을 아래처럼 바꾸어주면 된다.

```java
year % 100 == 0 일 때 "평년"을 -> year % 100 != 0 으로 뒤집는다면 "윤년"이 참 조건이 된다는 것.
```

그리고 이를 **논리 연산자를 활용해 if-else-if 조건을 한줄로 완성할 수 있다.**

```java
int year = input값;

if ((year % 400 == 0) || (year % 100 != 0 && year % 4 == 0)){
  System.out.println("윤년");
} else {
  System.out.println("평년");
}
```

정말 간단한 코드지만 코드를 짤때 중요한 건, **결과가 참이 나오더라도 끊임없이 의심해 보고 검토하는 태도와 이를 어떻게 하면 효율적으로 짤 수 있을지 고민하는 태도**인 것 같다.