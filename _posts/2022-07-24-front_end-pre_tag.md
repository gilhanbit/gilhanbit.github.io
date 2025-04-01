---
title: "[Front End] pre 태그"
excerpt: "쓴 그대로 보여주는 태그"

categories:
  - Front End
tags:
  - [frontend, pre]

permalink: /front_end/pre_tag/

toc: true
toc_sticky: true

date: 2025-04-01
last_modified_at: 2025-04-01
---

## 1. pre 태그

>`pre` 태그는 preformatted text의 줄임말로, HTML에서 사용자가 작성한 대로 공백, 줄바꿈 등을 유지하며 출력해주는 태그다.

**주로 코드 블럭이나 출력 포맷을 보여줄 때 사용된다.**

<hr>

### 기본 문법

```html
<pre>
  여기에 작성한
  내용은 그대로
  출력됩니다.
</pre>
```

### 출력 결과

```html
<pre>
여기에 작성한
내용은 그대로
 출력됩니다.
</pre>
```

**탭, 공백, 줄바꿈이 전부 보존된 채 출력.**

<hr>

```html
<pre>
function sayHello() {
 console.log("Hello, world!");
}
</pre>
```

입력한 코드 그대로 브라우저에서 출력됨.