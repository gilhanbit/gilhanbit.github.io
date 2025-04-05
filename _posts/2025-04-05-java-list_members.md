---
title: "[Java] 기존 List에 새로운 List 중복검사 후 추가하기"
excerpt: "기존 회원에 신규 회원 이름(동명이인) 중복검사 후 추가하는 방법"

categories:
  - Java
tags:
  - [java, list]

permalink: /java/list_members/

toc: true
toc_sticky: true

date: 2025-04-05
last_modified_at: 2025-04-05
---

## 1. 회원 추가하기

>새로 입력할 이름을 기존 리스트에 추가한다. (만약 동명이인이 있을 경우 회원명 뒤에 숫자를 붙인다)

- 기존 회원: 우솝, 루피, 상디, 나미, 로빈
- 신규 회원: 프랑키, 루피, 쵸파, 로빈, 루피

>정답: 우솝, 루피, 상디, 나미, 로빈, 보람, 루피1, 쵸파, 로빈1, 루피2

정답은 동명이인일 경우 뒤에 붙은 수에 `+1`을 해야만 파악이 가능하므로, **뒤에 붙은 숫자만 보고 직관적으로 수를 파악할 수 있도록** 2부터 시작하도록 변경하기로 함.

1. 기존 회원과의 비교를 위해 기존 회원과 신규 회원 모두 List에 저장.

```java
public static void main(String[] args) {
  List<String> members = new ArrayList<>(Arrays.asList("우솝", "루피", "상디", "나미", "로빈"));
  List<String> newMembers = new ArrayList<>(Arrays.asList("프랑키", "루피", "쵸파", "로빈", "루피"));
}
```

<hr>

## 2. 중복 검사 후 기존 회원에 추가

1. `for문`으로 순회.
2. 조건이 true일 경우 들어갈 수 있도록 `while문`으로 동명이인 검사.
3. 동명이인일 경우 숫자를 붙여 추가.

```java
public static void main(String[] args) {
  List<String> members = new ArrayList<>(Arrays.asList("우솝", "루피", "상디", "나미", "로빈"));
  List<String> newMembers = new ArrayList<>(Arrays.asList("프랑키", "루피", "쵸파", "로빈", "루피"));
  for(int i = 0; i < newMembers.size(); i++) {
        String newName = newMembers.get(i);
        Integer count = 1;
        while(members.contains(newName)) {
          count++;
          newName = newMembers.get(i) + count;
        }
        members.add(newName);
  }
	System.out.println(members);
}
```

<hr>

## 3. 신규 회원을 기존 회원에 직접 추가해가며 동명이인 검사까지할 수 있을까?

```java
public static void main(String[] args) {
  List<String> members = new ArrayList<>(Arrays.asList("우솝", "루피", "상디", "나미", "로빈"));
  List<String> newMembers = new ArrayList<>(Arrays.asList("프랑키", "루피", "쵸파", "로빈", "루피"));
  for(int i = 0; i < newMembers.size(); i++) {
        Integer count = 1;
        while(members.contains(newMembers.get(i))) {
          count++;
          members.add(newMembers.get(i) + count);
          newMembers.set(i, newMembers.get(i) + count);
        }
        members.add(newMembers.get(i));
  }
	System.out.println(members);
}
```

**결과**

```java
// 루피2, 루피23, 루피234, 루피2345... 무한루프 발생
```

**이유**

```java
for(int i = 0; i < newMembers.size(); i++) {
      Integer count = 1;
      while(members.contains(newMembers.get(i))) { // 루피 진입시작
        count++;
        members.add(newMembers.get(i) + count); // 기존 회원에 루피2 추가
        newMembers.set(i, newMembers.get(i) + count); // 신규 회원 루피 = 루피2 대입 -> 다시 while문 true -> 무한루프 발생
      }
      members.add(newMembers.get(i));
}
System.out.println(members);
```

**break;로 멈춘다면?**

```java
for(int i = 0; i < newMembers.size(); i++) {
      Integer count = 1;
      while(members.contains(newMembers.get(i))) { // 루피 진입시작
        count++;
        members.add(newMembers.get(i) + count); // 기존 회원에 루피2 추가
        newMembers.set(i, newMembers.get(i) + count); // 신규 회원 루피 = 루피2 대입 -> 다시 while문 true -> 무한루프 발생
        break;
      }
      members.add(newMembers.get(i)); // 루피2 한번 더 추가
}
System.out.println(members);
```

**결과**
```java
// 우솝, 루피, 상디, 나미, 로빈, 프랑키, 루피2, 루피2, 쵸파, 로빈2, 로빈2, 루피2, 루피2
```

루피2는 신규 회원 목록에 없으니 while에 들어가지 않고, 뒤의 루피가 다시 한번 while에 들어감.

<hr>

## 4. 핵심 Point!

즉, **이 문제를 풀기 위한 조건**은 아래와 같다고 생각한다.

1. 신규, 기존 회원 비교
2. 동명이인일 경우 뒤에 + count 후 while문을 빠져나올 것.
3. 반드시 while문을 빠져나와서 동명이인 + count를 기존 회원에 추가
4. 기존 회원(동명이인 + count까지 이루어진)과 다시 신규 회원 비교

`while문` 안에서 `add`를 할 경우 무한루프 발생. 무한루프 방지를 위해서는 **2, 3번이 Point!** 간단한 코드지만 깊게 고민해볼 수 있는 코드.