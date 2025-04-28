---
title: "[Baekjoon] 입출력과 사칙연산"
excerpt: "백준 - 단계별로 풀어보기"

categories:
  - Baekjoon
tags:
  - [백준, 코딩테스트]

permalink: /baekjoon/step1/

toc: true
toc_sticky: true

date: 2025-04-28
last_modified_at: 2025-04-28
---

```java
import java.util.Scanner;

public class step01 {

  public static void main(String[] args) {
    
    Scanner scan = new Scanner(System.in);
    
    // https://www.acmicpc.net/problem/2588
    System.out.print("세 자리 자연수 A, B를 입력하세요");
    int a = scan.nextInt();
    String b = scan.next();
    
    for (int i = b.length() - 1; i >= 0; i--) {
      System.out.println(a * (b.charAt(i) - '0'));
    }
    
    System.out.println(a * Integer.parseInt(b));
    
    
    // https://www.acmicpc.net/problem/10171
    System.out.println("\\    /\\");
    System.out.println(" )  ( ')");
    System.out.println("(  /  )");
    System.out.println(" \\(__)|");
        
        
    // https://www.acmicpc.net/problem/10172
    System.out.println("|\\_/|");
    System.out.println("|q p|   /}");
    System.out.println("( 0 )\"\"\"\\ ");
    System.out.println("|\"^\"`    |");
    System.out.println("||_/=\\\\__|");
    
  }
}
```
