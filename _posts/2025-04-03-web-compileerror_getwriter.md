---
title: "[Web] response.getWriter();의 컴파일 에러"
excerpt: "컴파일 에러 이유와 throws IOException을 선언해야 하는 이유"

categories:
  - Web
tags:
  - [web, ioexception]

permalink: /web/compileerror_getwriter/

toc: true
toc_sticky: true

date: 2025-04-03
last_modified_at: 2025-04-03
---

## 1. IOException란?

>`IOException`은 **Input/Output Exception**, 즉 **입출력 오류 예외**처리를 말한다.

- 파일을 읽으려 했는데 파일이 없음
- 파일이 잠겨 있어서 읽을 수 없음
- 네트워크 연결이 끊겨서 데이터 전송 불가
- 웹 브라우저에 응답을 보내는 중 연결이 끊김
- 출력 스트림이 이미 닫혀있는데 또 쓰려고 함

위와 같은 경우에 `IOException`가 발생할 수 있으며, 즉 **"읽거나 쓰는 도중 무언가 잘못되면 생기는 문제"를 `IOException`이라고 한다.**

<hr>

## 2. response.getWriter();의 컴파일 에러

>서블릿 클래스가 `HttpServlet`을 상속 받고, `HttpServletRequest`, `HttpServletResponse`, `PrintWriter`을 제대로 임포트 했음에도 `response.getWriter();`에서 컴파일 오류가 발생하는 주된 이유는 보통 `IOException`을 처리하지 않았기 때문이다.

```java
import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloServlet extends HttpServlet {
	
	@Override
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
		PrintWriter out = response.getWriter();
		out.println("Hello World!");
	}
}
```

`getWriter()`는 `IOException`을 던질 수 있으므로, `doGet()` 메서드에 `throws IOException`이 꼭 선언되어야 한다.

<hr>

## 3. 왜 doGet()에 던져야 할까?

>정확히 말하면, 예외는 **메서드를 호출한 쪽으로 던져지는 것.**

즉, 예외가 발생하면 이 메서드를 호출한 쪽이 그걸 처리해야 한다.

**doGet()은 누가 호출할까?**

`doGet()`은 서블릿 컨테이너(예: 톰캣, 제티, 웹로직 등)가 **HTTP 요청이 들어오면 자동으로 호출**해주는 메서드다.

누군가 웹 브라우저에서 `/내 서블릿`에 GET 요청을 보내면,

1. 톰캣이 요청을 받음
2. 작성한 서블릿 인스턴스를 찾아서
3. `doGet(request, response)` 메서드를 자동 호출

`doGet()`을 호출하는 주체는 **톰캣 같은 컨테이너다.**

결국 **예외는 "톰캣에게 던지는 것"**

**왜 직접 처리하지 않고 톰캣에게 던질까?**

보통 웹 애플리케이션에서는 요청 처리 도중 발생한 에러를 직접 처리하기보다는 **컨테이너(톰캣)**에 맡기고, 그쪽에서 공통 에러 페이지를 보여주는 방식이 일반적이다.

이는 모든 예외를 일일이 `try-catch` 하기보다 톰캣이 처리하는 게 유지보수에 유리하기 때문이다.