---
title: "[Java] 상속(Inheritance)"
excerpt: "상속의 정의와 상속을 사용하는 이유"

categories:
  - Java
tags:
  - [java, inheritance]

permalink: /java/inheritance/

toc: true
toc_sticky: true

date: 2025-04-20
last_modified_at: 2025-04-20
---

## 1. 상속이란?

**기존 클래스(부모 클래스)**의 <font color="#990000">필드(변수)와 메서드(기능)</font>를 **새로운 클래스(자식 클래스)**가 <u>물려받아 사용하는 것</u>을 말한다.

자식 클래스는 부모 클래스의 기능을 **재사용, 확장, 수정**할 수 있다.

<hr>

## 2. 상속을 사용하는 이유

1. 이미 있던 클래스를 재사용하기 때문에 효율적이다.
2. 부모 클래스를 수정하면 자식 클래스 전부가 영향을 받기에 유지보수가 용이하다.
3. 기존 코드를 재활용하고 기능을 추가하면 되므로 확장성이 좋다.
4. 부모 클래스로 자식 객체를 다룰 수 있으므로 유연하다. (2번과 비슷한 맥락)

<hr>

## 3. 상속 사용 방법

자식 클래스 옆에 `extends` 부모 클래스로 상속 관계를 선언한다.

자바는 다중 상속이 불가능 하므로, extends 뒤에는 하나의 부모 클래스만 와야한다.

```java
/*[접근 제어자]*/ class child extends parent


// 접근 제어자 종류
// - default: 지정하지 않았을 경우 기본으로 default. (같은 패키지 내에서만 접근 가능)
// - public: 접근 제한이 없다. (모두 접근 가능)
// - protected: 같은 패키지 내에서 접근 가능하며, 다른 패키지의 자손 클래스에서 접근 가능하다.
// - private: 같은 클래스 내에서만 접근이 가능하다. (외부에서 접근 불가)
```

**부모 클래스**
```java
public class HealthInfo {
	protected String name;
	protected int height;
	protected int weight;

  // 생성자(Constructor): 객체가 생성될 때 한번만 불려지는 특수한 메소드
	// return type이 아예 존재하지 않음(void 금지)
	// 클래스명과 같다.
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

**자식 클래스**
```java
public class HealthRate extends HealthInfo {

	public HealthRate(String name, int height, int weight) {
		// 생성자를 만들지 않으면 파라미터가 없는 기본 생성자가 만들어진다.
		// 파라미터 있는 생성자를 만들면 기본 생성자가 만들어지지 않는다.
		
		// 상속을 받는 자식 클래스의 객체를 생성할 때 부모 생성자가 먼저 호출된 뒤, 자식 생성자가 그 다음에 호출된다.
		
		// 임의로 부모 생성자를 부르지 않으면 부모 기본 생성자를 호출하려고 시도한다.
		// 하지만 부모 클래스에 기본 생성자가 없으므로 3개의 파라미터가 있는 부모 생성자를 호출.
		super(name, height, weight);

    // 부모 클래스에 파라미터가 없는 기본 생성자가 있거나, 생성자 자체가 없을 경우?
    // 자식 클래스에서 super()를 호출하지 않아도 컴파일러가 자동으로 생성하여 호출함.
	}
	
	public double standardWeight() {
		return (this.height - 100) * 0.9;
	}
	
	public double getRate() {
		return (this.weight - standardWeight()) / standardWeight() * 100;
	}
	
	public String status() {
		double rate = getRate();
		if (rate < 10) {
			return "정상";
		} else if (rate < 20) {
			return "과체중"; 
		} else {
			return "비만";
		}
	}
	
	@Override
	public void showInfo() {
		// 출력 (ex: 홍길동님의 신장 160, 몸무게 45kg => 정상입니다.)
		super.showInfo();
		System.out.println(" => " + status() + "입니다.");
	}
}
```

## 4. 부모 생성자의 호출 `super()`

이는 객체지향 프로그래밍에서 클래스와 객체 생성의 원리에 관련되어있다.

부모 클래스의 메서드가 호출되는 이유는 자식 클래스가 부모 클래스를 상속 받았기 때문이다.

자바에서는 생성자를 호출할 때마다 자식 클래스가 생성되기 전에 부모 클래스의 생성자가 먼저 호출된다.

이는 자바의 기본 원칙 중 하나로, 객체 생성 과정에서 부모 클래스의 모든 초기화 작업이 수행되도록 되어 있기 때문이다.

<hr>

## 참고

[참고 1](https://chanhuiseok.github.io/posts/java-1/)
[참고 2](https://backenddeveloper.tistory.com/7)
[참고 3](https://hoons-dev.tistory.com/106)
[참고 4](https://www.inflearn.com/community/questions/1558064/9%EB%B6%84-%EC%A7%88%EB%AC%B8?focusComment=407077&srsltid=AfmBOoq5q0tmkW_SoS8A8xUwjli913L1kscYrQoQ20vG-4-qWKFl-bdC)