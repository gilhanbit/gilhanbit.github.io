---
title: "[CS] 비전공자도 쉽게 이해하는 Library, Framework 차이점"
excerpt: "많이 헷갈리는 Library, Framework에 대해서 알아보자"

categories:
  - CS
tags:
  - [cs, library, framework]

permalink: /cs/libraryvsframework/

toc: true
toc_sticky: true

date: 2025-04-02
last_modified_at: 2025-04-02
---

## 1. 요약

>**라이브러리(Library)**와 **프레임워크(Framework)**는 비슷하면서도 다른 개발 도구로 **제어의 흐름(Control Flow)**에서 차이가 있다.

**한눈에 보는 차이점**

| 항목             | 라이브러리 (Library)             | 프레임워크 (Framework)             |
|------------------|----------------------------------|-----------------------------------|
| **제어 흐름**     | 내가 호출함 (개발자 주도)           | 프레임워크가 호출함 (프레임워크 주도) |
| **유연성**        | 비교적 자유로움                      | 정해진 틀을 따름                     |
| **규칙 강도**     | 낮음                                | 높음                                |
| **의존 방식**     | 내가 필요할 때 가져다 씀              | 내가 프레임워크 안에서 움직임         |
| **예시**         | React, jQuery, Lodash              | Angular, Vue.js, Spring            |

<hr>

## 2. 개념 설명

### 라이브러리(Library)
- **도구 상자** 같다고 보면 돼요.
- 필요할 때 꺼내서 내가 사용하는 구조입니다.
- 예: `React`는 UI 컴포넌트를 만들 수 있는 라이브러리입니다.
  - 내가 직접 컴포넌트를 만들고, 필요한 라이브러리를 가져와 사용하죠.

**예시 코드 (React):**
```js
import { useState } from 'react'; // 내가 필요한 것만 가져옴

function App() {
  const [count, setCount] = useState(0); // 내가 직접 로직 제어
  return <button onClick={() => setCount(count + 1)}>Click</button>;
}
```

---

### 2. ✅ 프레임워크(Framework)
- **틀이 잡힌 집**에 내가 들어가서 살아야 해요.
- 흐름을 프레임워크가 잡고 있고, 나는 그 안에서 코드를 작성합니다.
- 예: `Vue.js`, `Angular`, `Spring`은 모두 프레임워크입니다.

**예시 코드 (Spring):**
```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello, world!";
    }
}
```
- 위 코드처럼 Spring이 라우팅과 실행 흐름을 제어해줍니다.
- 나는 정해진 규칙에 맞춰 `@GetMapping`, `@RestController` 등을 써야 하죠.

---

## 🎯 비유로 이해하기

| 비유 | 설명 |
|------|------|
| **라이브러리**는 요리 재료 | 요리사가 내가 직접 꺼내 쓰는 재료들이에요 (선택권은 나에게 있음) |
| **프레임워크**는 요리법 키트 | 정해진 방식으로 조리해야 결과가 나와요 (규칙을 따라야 함) |

---

## 💡 함께 기억하면 좋은 포인트

- 라이브러리는 "선택의 자유"가 있지만, 구조는 개발자가 책임져야 해요.
- 프레임워크는 "강제된 틀"이 있지만, 일관성 있고 유지보수하기 쉬워요.
- 실제 개발에선 둘을 함께 쓰는 경우가 많아요.
  - 예: Vue (프레임워크) + Axios (라이브러리)
  - 예: Spring (프레임워크) + Jackson, Lombok 등 (라이브러리)

---

필요하다면 실무 기준으로 언제 어떤 걸 선택해야 하는지도 알려드릴 수 있어요.  
그거 궁금하신가요?
