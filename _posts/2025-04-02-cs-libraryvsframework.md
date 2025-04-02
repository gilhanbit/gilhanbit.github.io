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

## 1. 차이점 한눈에 보기

>**라이브러리(Library)**와 **프레임워크(Framework)**는 비슷하면서도 다른 개발 도구로 **제어의 흐름(Control Flow)**에서 차이가 있다.

**한눈에 보는 차이점**

| 항목             | 라이브러리 (Library)             | 프레임워크 (Framework)             |
|------------------|----------------------------------|-----------------------------------|
| **제어 흐름**     | 개발자가 호출           | 프레임워크가 호출함 |
| **유연성**        | 비교적 자유로움                      | 정해진 틀을 따름                     |
| **규칙 강도**     | 낮음                                | 높음                                |
| **의존 방식**     | 개발자가 필요할 때 가져다 씀              | 개발자가 프레임워크 안에서 움직임         |
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

## 3. 라이브러리와 프레임워크 그리고 IDE

>라이브러리와 프레임워크가 프로그래밍에 필요한 도구 모음이라면 IDE와 비슷한 건가?

**프레임워크와 IDE의 차이**

| 항목         | 프레임워크               | 개발 도구 (IDE, Editor)             |
|--------------|--------------------------|-------------------------------------|
| **정의**      | 개발 규칙과 구조를 제공함     | 코드를 작성, 편집, 실행하는 환경 제공 |
| **예시**      | Spring, Vue.js, Django  | IntelliJ, Eclipse, VSCode           |
| **역할**      | 애플리케이션 구조/흐름 담당   | 코딩, 디버깅, 자동완성 도와주는 툴    |

<hr>

## 4. 요약

### Library

>`Math`, `random` 클래스처럼 필요할 때 꺼내 쓸 수 있도록 만들어 놓은 도구 모음

### Framework

>빈칸만 입력하면 작동되도록 구현해 놓은 모음집