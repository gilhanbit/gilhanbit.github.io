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

<P>
자바(Java)에서 조건문은 특정 조건에 따라 코드의 흐름을 제어할 때 사용된다.
주로 if, ifelse, switch 세 가지가 많이 사용되며, 각각의 특징은 아래와 같다.
</p>
<h2>1. if문</h2>
if문은 가장 기본적인 조건문 중 하나로 조건이 'true'일 때만 해당 블록의 코드가 실행된다.
<pre>
  <code>
    ```java
    int num = 10;
    if (num > 5) {
    System.out.println("num은 5보다 큽니다.");
    }
  </code>
</pre>