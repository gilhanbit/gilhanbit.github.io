---
title: "[Baekjoon] 반복문"
excerpt: "for, while 등의 반복문을 사용해 봅시다."

categories:
  - Baekjoon
tags:
  - [baekjoon]

permalink: /baekjoon/step3/

toc: true
toc_sticky: true

date: 2025-05-04
last_modified_at: 2025-05-04
---

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.Scanner;
import java.util.StringTokenizer;

public class Step03 {

  private static String String;

  public static void main(String[] args) throws NumberFormatException, IOException {
    
    Scanner scan = new Scanner(System.in);
    StringTokenizer st;
    BufferedReader br = new BufferedReader (new InputStreamReader(System.in));
    BufferedWriter bw = new BufferedWriter (new OutputStreamWriter(System.out));
    
    // https://www.acmicpc.net/problem/2739
    System.out.println("구구단");
    int a = scan.nextInt();
    
    for (int i = 1; i < 10; i++) {
      System.out.println(a + " X " + i + " = " + (a * i));
    }
    
    // https://www.acmicpc.net/problem/8393
    System.out.println("입력 값까지의 합 구하기");
    int b = scan.nextInt();
    
    int sum = 0;
    for (int i = 1; i <= b; i++) {
      sum += i;
    }
    System.out.println(sum);
    
    // https://www.acmicpc.net/problem/25304
    int totalPrice = Integer.parseInt(br.readLine());
    int totalCount = Integer.parseInt(br.readLine());
    int price, count, sum = 0;
    
    for (int i=0; i<totalCount; i++){
        st = new StringTokenizer(br.readLine());
        price = Integer.parseInt();
        count = Integer.parseInt(st.nextToken());
        sum += (price*count);
    }
    
    if (sum == totalPrice){
        System.out.println("Yes");
    } else {
        System.out.println("No");
    }
    
    // https://www.acmicpc.net/problem/15552
    int t = Integer.parseInt(br.readLine());
    
    for (int i = 0; i < t; i++) {
      st = new StringTokenizer(br.readLine());
      int a = Integer.parseInt(st.nextToken());
      int b = Integer.parseInt(st.nextToken());
      bw.write((a+b) + "\n");
    }
    
    bw.flush();
    
    br.close();
    bw.close();
    
    // https://www.acmicpc.net/problem/25314
    System.out.println("byte");
    int size = scan.nextInt();
    
    if (size % 4 == 0) {
      for (int i = 0; i < size / 4; i++) {
        System.out.print("long ");
      }
      System.out.print("int");
    } else {
      System.out.println("false");
    }
    
    // https://www.acmicpc.net/problem/11021
    int t = Integer.parseInt(br.readLine());
    
    for (int i = 1; i <= t; i++) {
      st = new StringTokenizer(br.readLine());
      int a = Integer.parseInt(st.nextToken());
      int b = Integer.parseInt(st.nextToken());
      bw.write("Case #" + i + ": " + (a+b) + "\n");
    }
    
    bw.flush();
    
    br.close();
    bw.close();
    
    // https://www.acmicpc.net/problem/11022
    int t = Integer.parseInt(br.readLine());

    for (int i = 1; i <= t; i++) {
      st = new StringTokenizer(br.readLine());
      int a = Integer.parseInt(st.nextToken());
      int b = Integer.parseInt(st.nextToken());
      bw.write("Case #" + i + ": " + a + " + " + b + " = " + (a + b) + "\n");
    }

    bw.flush();

    br.close();
    bw.close();
    
    // https://www.acmicpc.net/problem/2438
    int t = scan.nextInt();

    for (int i = 1; i <= t; i++) {
      for (int j = 0; j < i; j++) {
        System.out.print("*");
      }
      System.out.println();
    }
    
    // https://www.acmicpc.net/problem/2439
    int t = scan.nextInt();

    for (int i = 1; i <= t; i++) {
      for (int j = t - 1; j >= i; j--) {
        System.out.print(" ");
      }
      for (int j = 0; j < i; j++) {
        System.out.print("*");
      }
      System.out.println();
    }
    
    // https://www.acmicpc.net/problem/10952
    for (int i = 0; i < 1;) {
      st = new StringTokenizer(br.readLine());
      int a = Integer.parseInt(st.nextToken());
      int b = Integer.parseInt(st.nextToken());
      bw.write((a+b) + "\n");
      if (a == 0 && b == 0) {
        break;
      }
      i--;
    }
    
    bw.flush();
    
    br.close();
    bw.close();
    
    // https://www.acmicpc.net/problem/10951
    while (scan.hasNext()) {
      int a = scan.nextInt();
      int b = scan.nextInt();
      System.out.print(a+b);
    }
    // EOF - mac: control + d
    
  }
}
```
