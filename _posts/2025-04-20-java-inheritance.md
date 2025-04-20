---
title: "[Java] 상속(Inheritance)"
excerpt: "상속의 장단점"

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

>**기존 클래스(부모 클래스)의 <font color="#990000">필드(변수)와 메서드(기능)</font>**를 **새로운 클래스(자식 클래스)**가 <u>물려받아 사용하는 것</u>을 말한다.

이를 **is-a** 관계라고 표현하기도 하며, 자식 클래스는 부모 클래스의 기능을 **재사용, 확장, 수정**할 수 있다.

<hr>

## 2. 상속을 사용하는 이유

1. 이미 있던 클래스를 재사용하기 때문에 효율적이다.
2. 부모 클래스를 수정하면 자식 클래스 전부가 영향을 받기에 유지보수가 용이하다.
3. 기존 코드를 재활용하고 기능을 추가하면 되므로 확장성이 좋다.
4. 부모 클래스로 자식 객체를 다룰 수 있으므로 유연하다. (2번과 비슷한 맥락)

<hr>

## 3. 상속 사용 방법

>자식 클래스 옆에 `extends` 부모 클래스로 상속 관계를 선언한다.

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

### 부모 생성자의 호출 `super()`

이는 객체지향 프로그래밍에서 클래스와 객체 생성의 원리에 관련되어있다.

부모 클래스의 메서드가 호출되는 이유는 자식 클래스가 부모 클래스를 상속 받았기 때문이다.

자바에서는 생성자를 호출할 때마다 자식 클래스가 생성되기 전에 부모 클래스의 생성자가 먼저 호출된다.

이는 자바의 기본 원칙 중 하나로, 객체 생성 과정에서 부모 클래스의 모든 초기화 작업이 수행되도록 되어 있기 때문이다.

<hr>

## 4. 상속의 단점

>상속의 장점으로 인한 편리함도 있지만, 많은 단점도 공존한다.

### 의존도가 높아진다.

객체지향 프로그래밍에서는 의존성(결합도, coupling)이 낮을수록, 응집도(해당 클래스가 갖는 책임)는 높을수록 좋다.

그러나 상속을 하게 될 경우 결합도가 당연히 높아질 수 밖에 없다.

부모, 자식 간의 결합도가 높다면 부모 클래스의 변경으로 인해 자식 클래스가 취약해질 수 있으며, 이를 취약한 기반 클래스 문제라고 부른다.

### 캡슐화를 깨트린다.

>캡슐화: 객체의 필드, 메서를 하나로 묶고 구현 내용을 은닉해 접근 제어자를 통해 접근하는 것을 말한다.

이를 다른 말로 풀어 쓴다면, 부모 클래스가 어떻게 구현되느냐에 따라 자식 클래스에 이상이 생길 수도 있다는 것이다.

이를 해결하려면 부모 클래스의 구현 내용을 알아야 하며, 불필요한 구현 내용의 노출은 캡슐화를 깨트리는 행위다.

### 상속은 결함 또한 상속한다.

부모 클래스에 결함이 있을 경우, 자식 클래스에도 이 결함이 상속된다.

따라서 상속을 사용하려면 결함이 있는지 확인해야 하는 검증이 반드시 필요하다.

<hr>

## 5. 상속 대신 OO을 사용해라?

자바 상속의 단점을 막기위해, **<font color="#000099">합성(Composition)</font>**을 사용하라고 말한다.

상속은 부모 클래스의 구현에 결합되어 변경에 취약하고 캡슐화를 약화시킨다는 단점이 있지만, 합성은 다른 클래스의 인스턴스를 포함하는 방식으로 코드를 재사용하기 때문에 상속의 단점을 보완하고 코드 재사용을 더 유연하게 할 수 있기 때문이다.

1. `is-a` 관계 (자식이 부모의 일종일 경우)
2. 여러 클래스가 공통 기능(공통된 필드 / 메서드)을 가질 때

그래서 위의 두 가지 경우가 아니라면 합성이 더 적절하다. 그리고 합성이 더 적절한 경우는 아래와 같다.

1. `has-a` 관계 (일종이 아니라 갖고 있는 경우)
2. 여러 기능을 조합할 때

<hr>

## 6. 상속 사용 체크리스트

|  | 항목 | 체크 |
|------|------|------|
| 1 | **“자식은 부모의 일종(is-a)인가?”**<br>→ 예: Dog is an Animal | ✔️ / ❌ |
| 2 | **“부모 클래스의 기능이 자식에게 의미가 있는가?”** | ✔️ / ❌ |
| 3 | **“공통 기능이 여러 클래스에 중복되어 작성되고 있는가?”** | ✔️ / ❌ |
| 4 | **“단일 상속만으로 충분한가?”**<br>→ 여러 상위 클래스를 동시에 쓰고 싶다면 인터페이스나 합성 고려 | ✔️ / ❌ |

**하나라도 ❌면?**
→ 상속보다 "합성(Composition)"을 고려해 보는 것이 좋다.

<hr>

**참고**

- [참고 1](https://chanhuiseok.github.io/posts/java-1/)
- [참고 2](https://backenddeveloper.tistory.com/7)
- [참고 3](https://hoons-dev.tistory.com/105)
- [참고 4](https://hoons-dev.tistory.com/106)
- [참고 5](https://www.inflearn.com/community/questions/1558064/9%EB%B6%84-%EC%A7%88%EB%AC%B8?focusComment=407077&srsltid=AfmBOoq5q0tmkW_SoS8A8xUwjli913L1kscYrQoQ20vG-4-qWKFl-bdC)
- [참고 6](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5%EC%9D%98-%EC%83%81%EC%86%8D-%EB%AC%B8%EC%A0%9C%EC%A0%90%EA%B3%BC-%ED%95%A9%EC%84%B1Composition-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0#1._%EA%B2%B0%ED%95%A9%EB%8F%84%EA%B0%80_%EB%86%92%EC%95%84%EC%A7%90)