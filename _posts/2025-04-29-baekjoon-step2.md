---
title: "[Baekjoon] 조건문"
excerpt: "if 등의 조건문을 사용해 봅시다."

categories:
  - Baekjoon
tags:
  - [백준, 코딩테스트]

permalink: /baekjoon/step2/

toc: true
toc_sticky: true

date: 2025-04-29
last_modified_at: 2025-04-29
---

```java
import java.util.Random;
import java.util.Scanner;

public class step02 {

  public static void main(String[] args) {
    
    Scanner scan = new Scanner(System.in);
    
    
    // https://www.acmicpc.net/problem/1330
    System.out.println("비교할 두 수를 입력하세요.");
    int a = scan.nextInt();
    int b = scan.nextInt();

    if (a > b) {
        System.out.println(">");
    } else if (a == b) {
        System.out.println("==");
    } else {
        System.out.println("<");
    }
    
    
    // https://www.acmicpc.net/problem/2753
    System.out.println("윤년 체크");
    int year = scan.nextInt();
    
    System.out.println(year % 4 == 0 && year % 100 != 0 || year % 400 ==0 ? "윤년" : "윤년이 아니다.");
    
    
    // https://www.acmicpc.net/problem/14681
    System.out.println("x");
    int x = scan.nextInt();
    System.out.println("y");
    int y = scan.nextInt();
    
    if (x != 0 && y != 0) {
      if (x > 0 && y > 0) {
        System.out.println("quadrant1");
      } else if (x < 0 && y > 0) {
        System.out.println("quadrant2");
      } else if (x < 0 && y < 0) {
        System.out.println("quadrant3");
      } else if (x > 0 && y < 0) {
        System.out.println("quadrant4");
      } 
    }
    
    
    // https://www.acmicpc.net/problem/2884
    System.out.println("H");
    int h = scan.nextInt();
    System.out.println("M");
    int m = scan.nextInt();
    
    if (0 < h && h < 24 && 0 <= m && m < 60) {
      m -= 45;
      if (m < 0) {
        h--;
        m += 60;
      }
    } else {
      System.out.println("잘못 입력하셨습니다.");
    }
    
    System.out.println(h + ":" + m);
    
    
    // https://www.acmicpc.net/problem/2525
    System.out.println("현재 시각을 입력하세요.");
    int h = scan.nextInt();
    int m = scan.nextInt();
    System.out.println("쿠킹 타임을 입력하세요.");
    int cookingTime = scan.nextInt();
    
    if (0 <= h && h < 24 && 0 <= m && m < 60) {
      m = m - cookingTime;
      while (m < 0) {
        h++;
        m += 60;
      }
      if (h > 24) {
        h -= 24;
      }
    }
    
    System.out.println(h + ":" + m);
    
    
    // https://www.acmicpc.net/problem/2480
    // 값을 입력하는 대신, 랜덤으로 값을 받도록 수정
    System.out.println("주사위를 굴렸다!");
    Random rand = new Random();
    int[] dice = new int[3];
    for (int i = 0; i < 3; i++) {
      dice[i] = rand.nextInt(6) + 1;
      System.out.println(dice[i] + " ");
    }
    
    int result = 0;
    int same = 0;
    for (int i = 0; i < dice.length - 1; i++) {
      for (int j = i + 1; j < dice.length; j++) {
        if (dice[i] == dice[j]) {
          result++;
          same = dice[i];
        }
      }
    }
    
    int max = dice[0];
    for (int i = 0; i < dice.length; i++) {
      if (max < dice[i]) {
        max = dice[i];
      }
    }
    
    if (result == 3) {
      System.out.println(10000 + same * 1000);
    } else if (result == 1) {
      System.out.println(1000 + same * 100);
    } else {
      System.out.println(max * 100);
    }
  }
}
```
