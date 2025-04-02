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
- 흔히 **도구 상자**에 비유.
- 필요할 때 꺼내서 사용하는 도구들을 모아 놓음.
- Math, random 등.

`Hello, World!`를 출력하기 위해 직접 서버를 구성해야 하며 구조와 흐름 또한 **개발자가 직접 작성해야 한다.**

```java
import com.sun.net.httpserver.HttpServer;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpExchange;

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;

public class HelloWorldServer {
    public static void main(String[] args) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(8000), 0);
        server.createContext("/", new MyHandler());
        server.setExecutor(null); // 기본 executor
        server.start();
        System.out.println("서버 시작됨: http://localhost:8000");
    }

    static class MyHandler implements HttpHandler {
        @Override
        public void handle(HttpExchange t) throws IOException {
            String response = "Hello, World!";
            t.sendResponseHeaders(200, response.getBytes().length);
            OutputStream os = t.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}
```


### 프레임워크(Framework)
- 도구를 사용해 직접 집을 지을 필요는 없고 **집**에 필요한 가구 등만 배치하면 됨.
- 흐름은 프레임워크가 잡고 있으며, 개발자는 주어진 흐름 위에 코드를 작성.

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
public class HelloWorldApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloWorldApplication.class, args);
    }
}

@RestController
class HelloController {
    @GetMapping("/")
    public String hello() {
        return "Hello, World!";
    }
}
```

- 위 코드처럼 Spring이 라우팅과 실행 흐름을 제어.
- 개발자는 정해진 규칙에 맞춰 `@GetMapping`, `@RestController` 등을 사용.

<hr>

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
