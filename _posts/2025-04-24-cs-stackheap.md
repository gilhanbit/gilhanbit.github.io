---
title: "[CS] 스택(Stack)과 힙(Heap)의 차이"
excerpt: "데이터 / 스택 / 힙"

categories:
  - CS
tags:
  - [cs, stack, heap]

permalink: /cs/stackheap/

toc: true
toc_sticky: true

date: 2024-04-24
last_modified_at: 2024-04-24
---

기술면접에서 자주 나온다고 하는 만큼, 개발자에게 `stack`과 `heap`은 굉장히 중요하다.

프로그램이 실행되면 메모리가 자동으로 할당되고 해제되며 동작하는데, 이때 할당되는 메모리의 공간이 크게 `데이터`, `스택`, `힙` 영역으로 나누어진다.

![memory](/assets/images/posts_img/stackheap/memory.png)

메모리 구조를 간단하게 나타내면 위 사진과 같다.

<hr>

## 1. 코드 영역

>프로그램 안에 개발자가 적은 코드가 저장되는 영역이다.

덱스트 영역이라고도 부르며 CPU가 코드 영역에 저장된 명령어를 하나씩 가져가 처리하게 된다.

`main()`함수부터 모든 정의가 여기에 해당하며, 실행 파일을 메모리에 올릴 때 함께 올라간다.

<hr>

## 2. 데이터 영역

>전역변수, 정적(static)변수(자바에서는 전역 == static)들이 저장되는 곳이다.

우리가 만들지 않고(new또한 하지 않음) 호출해서 사용하는 메서드들이 static에 해당, `프로그램 실행과 동시에 메모리에 올려진다.`

즉, 중괄호로 묶여있는 범위의 밖에 선언되는 변수가 위치한 곳을 말한다.

```java
public class Class {

  public static int a = 0;

  public static void main() {
    System.in.print()
  }
}
```

이름에서 알 수 있듯 블록 안이 아니라 `모든 곳에서 불러 올 수 있다.`

프로그램이 시작되는 시점부터 끝날 때까지 메모리를 사용하며 사라지지 않는다.

- 프로그램이 종료될 때까지 지워지지 않을 데이터를 저장
- 대표적으로 전역변수, static변수
- 상수

<hr>

## 3. 스택 영역

>지역변수와 매개변수(parameter)가 저장되는 영역이다.

**함수의 호출과 함께 할당**되며, **함수의 호출이 완료되면 소멸**한다.

이처럼 스택 영역에 저장되는 함수의 호출 정보를 스택 프레임(stack frame)이라고 하며, 스택 영역은 `push`로 데이터를 저장, `pop` 동작으로 데이터를 인출한다.

**<font color="#990000">후입선출(LIFO, Last-In First-Out)</font>**에 따라 동작하므로, 가장 늦게 저장된 데이터가 가장 먼저 인출된다. (씻은 접시를 쌓아두고 가장 위의 접시 먼저 꺼내는 것에 비유할 수 있다)

- 잠깐 사용하고 삭제하는 데이터 (지역변수, 매개변수)
- 해당 객체가 정의된 블록(스코프)를 벗어날 때 소멸
- 함수를 호출하는 위치도 저장
- 힙보다 빠름

<hr>

## 4. 힙 영역

>객체, 동적 메모리를 저장하는 곳이다.

`new`를 통해 객체를 생성하면 힙에 저장되며, 힙에 할당된 메모리는 개발자가 직접 없애지 않는 이상 계속 남아있는다. (자바는 GC가 자동으로 삭제)

- 가비지 컬렉터가 없으면 개발자가 직접 관리 해줘야 함
- 스택보다 큰 메모리 할당
- 동적 메모리 할당 (new / 포인터)
- 스택보다 느림

<hr>

## 5. 예시 코드

```java
public class Memory {

  class Dog {
      String name; // heap 내부 변수

      Dog(String name) {
          this.name = name; // heap에 저장
      }

      void bark() {
          System.out.println(name + "가 짖습니다!"); // heap 데이터 사용
      }
  }

  public static void main(String[] args) {
      int localVal = 100; // stack
      Dog myDog = new Dog("바둑이"); // stack + heap
      myDog.bark(); // stack 호출 + heap 객체 접근
  }
}
```

<hr>

**참고**

[참고 1](https://helloworld-japan.tistory.com/33)
[참고 2](https://junghyun100.github.io/%ED%9E%99-%EC%8A%A4%ED%83%9D%EC%B0%A8%EC%9D%B4%EC%A0%90/)
