---
title: "[Web] JSP 연습 문제"
excerpt: "checkbox로 조건 걸기"

categories:
  - Web
tags:
  - [web, checkbox, parameter, null]

permalink: /web/checkbox/

toc: true
toc_sticky: true

date: 2025-04-15
last_modified_at: 2025-04-15
---

## 1. 목적

>메뉴 데이터 중 별점이 4점 이상인 메뉴만 출력 (체크하지 않았을 경우 검색한 메뉴만 출력)

![출력](/assets/images/posts_img/checkbox/checkbox.png)

<hr>

## 2. Main page

1. 키워드 입력박스
2. 별점(점수) 필터 체크박스
3. submit 타입 버튼

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>배탈의 민족</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-Fy6S3B9q64WdZWQUiU+q4/2Lc9npb8tCaSX9FK7E8HnRr0Jz8D6OP9dO5Vg3Q9ct" crossorigin="anonymous"></script>
  </head>
  <body>
    <div class="container">
      <h1>메뉴 검색</h1>
      <form method="post" action="/lesson02/quiz07_1.jsp">
        <div class="d-flex">
          <input type="text" class="form-control col-3" name="keyword">
          <label class="ml-2 mt-2"><input type="checkbox" name="starPointFilter"> 4점 이하 제외</label>
        </div>
        <input type="submit" class="btn btn-success mt-3" value="검색">
      </form>
    </div>
  </body>
</html>
```

- 민감한 정보인가? -> no (get도 가능)
- 입력한 키워드만 추출하기 위해 `name`값을 줌
- `checkbox`에 value값을 주면 알아보기에 쉬울 수 있음 (`on`, `null`을 기억하면 굳이 주지 않아도...)

<hr>

## 3. Result page

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.*" %>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>배탈의 민족</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-Fy6S3B9q64WdZWQUiU+q4/2Lc9npb8tCaSX9FK7E8HnRr0Jz8D6OP9dO5Vg3Q9ct" crossorigin="anonymous"></script>
  </head>
  <body>
    <%
    List<Map<String, Object>> list = new ArrayList<>(); // list에 map 데이터 저장

    Map<String, Object> map = new HashMap<String, Object>() {{ put("name", "버거킹"); put("menu", "햄버거"); put("point", 4.3); } };
    list.add(map);
    map = new HashMap<String, Object>() {{ put("name", "BBQ"); put("menu", "치킨"); put("point", 3.8); } };
    list.add(map);
    map = new HashMap<String, Object>() {{ put("name", "교촌치킨"); put("menu", "치킨"); put("point", 4.1); } };
    list.add(map);
    map = new HashMap<String, Object>() {{ put("name", "도미노피자"); put("menu", "피자"); put("point", 4.5); } };
    list.add(map);
    map = new HashMap<String, Object>() {{ put("name", "맥도날드"); put("menu", "햄버거"); put("point", 3.8); } };
    list.add(map);
    map = new HashMap<String, Object>() {{ put("name", "BHC"); put("menu", "치킨"); put("point", 4.2); } };
    list.add(map);
    map = new HashMap<String, Object>() {{ put("name", "반올림피자"); put("menu", "피자"); put("point", 4.3); } };
    list.add(map);
    %>

    <div class="container">
      <h1 class="text-center">검색 결과</h1>
      <table class="table text-center">
        <thead>
          <tr>
            <th>메뉴</th>
            <th>상호</th>
            <th>별점</th>
          </tr>
        </thead>
        <tbody>
          <%
          String keyword = request.getParameter("keyword");
          String starPointFilter = request.getParameter("starPointFilter");

          for (Map<String, Object> item : list) {
            if (keyword.equals(item.get("menu"))) {
              if (starPointFilter != null && (double)item.get("point") <= 4.0) {
                continue; // 조건에 일치할 경우 skip 후 출력
              }
          %>
          <tr>
            <td><%= item.get("menu") %></td>
            <td><%= item.get("name") %></td>
            <td><%= item.get("point") %></td>
          </tr>
          <%
            }
          }
          %>
        </tbody>
      </table>
    </div>
  </body>
</html>
```

<hr>

## 4. checkbox를 null이 아닌 값으로 비교할 경우

```java
if (starPointFilter != null && (double)item.get("point") <= 4.0)
-> if (starPointFilter.equals("on") && (double)item.get("point") <= 4.0)
// checkbox에 체크 시 on으로 들어오니 on으로 비교해도 같은 결과가 출력되지 않을까?


/*
결과: NPE
이유: starPointFilter는 현재 null인데 비교를 위해 equals("on")를 호출하려했기 때문이다. (null이면 값 자체가 없지만 on일 경우 string으로 on저장)
*/
```

알고나면 당연한 결과지만, 에러를 분석하기 전까지 사람 머리로는 맞는 조건이 아니라고 하니 아주 흥미롭다.

또 한가지 흥미로운 점은 형변환에 관환 것인데, `Integer`와 `int`는 `auto-boxing`과 `auto-unboxing` 기능으로 자동 변환을 해주는데 왜 **object에 저장한 double 타입의 point는 자동으로 다운캐스팅을 해주지 않을까? 하는 점**과 **가장 상위에 있는 object라면 전부 수용하고 비교할 수 있어야 하는 게 맞는 것 같지만 아니라는 점**이다.

이유는 일단 오토박싱과 오토언박싱은 같은 타입에만 가능, 자바는 컴파일 시점에 타입을 확인하는 정적 타입 언어이기 때문이다.

자동으로 변환을 해주니 같은 타입이라면 기본형, 참조형 구분 없이 막 사용해도 되겠네? 편하다! 생각할 수 있지만, 오토박싱, 오토언박싱이 빈번하게 발생하는 코드는 CPU, 메모리 사용량을 증가시킬 수 있으므로 성능을 고려한다면 코드 작성 시 필요할 때만 래퍼 클래스를 사용해야한다.