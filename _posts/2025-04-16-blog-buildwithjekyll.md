---
title: "[Blog] pages build and deployment 에러"
excerpt: ".md 파일은 repository에 정상적으로 업로드 되었으나 블로그에 노출이 되지 않을 때"

categories:
  - Blog
tags:
  - [blog, buildwithjekyll]

permalink: /blog/buildwithjekyll/

toc: true
toc_sticky: true

date: 2025-04-16
last_modified_at: 2025-04-16
---

## 1. pages build and deployment 에러

어느 때처럼 vscode에 글을 작성 후 깃헙 리포지토리에 push를 했다.

그러나 시간이 지나도 내 블로그에 글이 보이지 않는 것.

[에러 포스팅 : JSP 연습 문제](https://gilhanbit.github.io/web/checkbox)

처음에는 캐시문제인 줄 알고 캐시를 삭제 후 기다려 보는데...

시간이 지나도 보이지 않았다.

혹시 제대로 push가 되지 않았나 싶어 리포지토리를 확인해 봤지만 .md 파일은 제대로 올라가 있었다.

그럼 왜지? 하고 리포지토리의 Actions 탭을 확인해 봤다.

그리고 아래처럼 계속된 빌드 에러가 나고 있었다.

![에러](/assets/images/posts_img/blog/buildwithjekyll/fail_pagesbuildanddeployment.png)

## 2. Build with Jekyll

![에러2](/assets/images/posts_img/blog/buildwithjekyll/buildwithjekyll.png)

에러 원인 파악을 위해 우측 상단의 view logs를 클릭하고 로그인.

![에러3](/assets/images/posts_img/blog/buildwithjekyll/error3.png)

Build with Jekyll 좌측 X 옆의 화살표 버튼을 누르면 에러의 원인을 파악할 수 있다.

처음에는 jsp 지원을 하지 않아서 발생한 에러인가 싶어, 있는 그대로 출력하도록 지시하는 태그를 집어넣어 수습했는데 진짜 에러 원인은 따로 있었다.

![에러3](/assets/images/posts_img/blog/buildwithjekyll/error4.png)

바로 **Jekyll이 Liquid 템플릿 문법을 잘못 해석해 에러를 발생시킨 것**이다.

코드 블록의 내용을 {% raw %}{{{% endraw %} 로 시작하는 Liquid 변수로 오해하면서 발생한 문제였던 것.

<font color="#990000">코드 블록 밖에서 {% raw %}{{{% endraw %} 을 연달아서 사용해도 에러 발생 확인!</font>

그럼 동일한 코드를 jsp로 감싸주고 에러의 원인만 수정해보자.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.*" %>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>배탈의 민족</title>
  </head>
  <body>
    <%
    List<Map<String, Object>> list = new ArrayList<>();

    // 정석대로 입력
    Map<String, Object> map = new HashMap<>();
    map.put("name", "버거킹");
    map.put("menu", "햄버거");
    map.put("point", 4.3);
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
                continue;
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

만약 이 글이 보인다면 제대로 수정한 것!

{% raw %}{%{% endraw %} raw %}
다른 한가지 방법은 이렇게 감싸는 방법이있다.
{% raw %}{%{% endraw %} endraw %}
