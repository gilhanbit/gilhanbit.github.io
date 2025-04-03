---
title: "[Java] 기초지만 실수할 수 있는 문제"
excerpt: "System.out.println() 메서드에 void를 넣으려 하는 경우"

categories:
  - Java
tags:
  - [java, inheritance]

permalink: /java/voidinprintln/

toc: true
toc_sticky: true

date: 2025-04-03
last_modified_at: 2025-04-03
---

## 1. Inheritance 구조로 신장과 몸무게를 받아 비만도를 계산하는 프로그램 구현

1. 부모 클래스를 상속받는 비만도 계산 클래스 설계
2. 표준체중을 계산하는 메서드 정의
3. 비만 여부를 구하는 메서드 정의

### 부모 클래스

```java
public class HealthInfo {
    protected String name;
    protected int height;
    protected int weight;

    public HealthInfo(String name, int height, int weight) {
        this.name = name;
        this.height = height;
        this.weight = weight;
    }

    public void showInfo() {
        System.out.println(name + "님의 신장 " + height + ", 몸무게 " + weight + "kg 입니다.");
    }

    public String getName() {
        return name;
    }

    public int getHeight() {
        return height;
    }

    public int getWeight() {
        return weight;
    }
}
```

### 자식 클래스

```java
public class HealthRate extends HealthInfo {
  public HealthRate(String name, double height, double weight) {
		super(name, height, weight);
	}
	
	public double sw() {
		return (this.height - 100)*0.9;
	}
	
	public double rate() {
		return (this.weight - sw()) / sw() * 100.0;
	}
	
	public String printRate() {
		if (rate() < 10) {
			return "정상";
		} else if (rate() < 20) {
			return "과체중";
		} else {
			return "비만";
		}
	}
	
	@Override
	public void showInfo() {
		super.showInfo();
		System.out.println(" -> " + printRate() + "입니다.");
	}
}
```

### 자식 클래스의 인스턴스

```java
public class HealthInfoTest {

	public static void main(String[] args) {
		
		HealthRate person1 = new HealthRate("Park", 155, 50);
		System.out.println(person1.sw());
		System.out.println(person1.rate());
		System.out.println(person1.printRate());
		System.out.println(person1.showInfo()); // 내가 한 실수
	}
}
```

그리고 얻은 결과

```text
Exception in thread "main" java.lang.Error: Unresolved compilation problem:
    The method println(boolean) in the type PrintStream is not applicable for the arguments (void)
```

아주 기초 중의 기초이지만 무의식적으로 void를 print에 집어넣은 것.

## 2. 에러 원인

위 오류 메시지는 Java 코드에서 `System.out.println()` 메서드에 `void` 타입 값을 넣으려고 해서 발생하는 문제다.

`System.out.println()`은 값을 출력하는 메서드인데, 내가 출력하려 한 건 **값이 아니라 "void 반환 메서드의 호출 결과"**였던 것.

### 수정 코드

```java
public class HealthInfoTest {

	public static void main(String[] args) {
		
		HealthRate person1 = new HealthRate("Park", 155, 50);
		System.out.println(person1.sw());
		System.out.println(person1.rate());
		System.out.println(person1.printRate());
		person1.showInfo(); // 호출만 하면 됨
	}
}
```

정말 기초 중의 기초에서 비롯된 실수...

조금 익숙해 졌다고 제대로 확인하지 않은 내 모습에 다시 한번 경각심을 갖게 되었다.

아직 갈 길이 먼 초보 개발자, 아니 개발자를 꿈꾸는 학생이라는 사실을 잊지말자.