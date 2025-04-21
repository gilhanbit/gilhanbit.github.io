---
title: "[Java] 합성 vs 인터페이스"
excerpt: "코드의 재사용을 방지하고 유연한 설계가 가능한 합성과 인터페이스. 둘의 차이점은?"

categories:
  - Java
tags:
  - [java, composition, interface]

permalink: /java/composition/

toc: true
toc_sticky: true

date: 2025-04-21
last_modified_at: 2025-04-21
---

자바 상속에 대해 구글링을 해보면 상속을 지양하고 합성을 지향하는 글을 많이 볼 수 있다.

초기에는 코드의 재사용(중복 방지), 유지보수의 편리함 등으로 많이 사용했던 것 같은데, <u>점점 다양한 기능을 요구하게 되고... 코드는 복잡해지고...</u>

오히려 부모 클래스까지 찾고, 파악하고, 수정하는 일이 더 비효율적인 방식이 되어버린 것 같다.

그리고 여기에 점점 발전하는 PC의 성능으로 과거에 비해 메모리 사용에 대한 걱정이 덜하기 때문이 아닐까 싶다.

자바에서 **합성(composition)**과 **인터페이스(interface)**는 객체 간 관계를 설계할 때 사용하는 개념으로, 둘 다 `다형성`을 목적으로 쓰지만, **어떨 때 어떤 걸 써야 할지** 구분하는 게 중요하다.

<hr>

## 1. 합성이란?

>필드에 다른 클래스의 인스턴스를 참조하게 만드는 설계.

서로 관련없는 클래스 관계에서 다른 클래스의 기능을 사용해야 한다면? 합성 방식을 사용한다고 보면 된다.

### - 합성은 왜 사용할까?

1. 상속보다 유연하다. (부모 클래스가 바뀌어도 영향을 덜 받음)
2. 재사용이 용이하다. (객체만 변경하면 됨)
3. 캡슐화 (참조한 객체는 외부에서 직접 접근이 불가)

### - 코드 예시

```java
// 별도의 기능 클래스
class Engine {
    public void start() {
        System.out.println("Engine started.");
    }
}

// 자동차는 엔진을 '가지고 있음'
class Car {
    private Engine engine;

    public Car() {
        engine = new Engine(); // 합성
    }

    public void startCar() {
        engine.start();
        System.out.println("Car is moving.");
    }
}
```

```java
public class CompositionTest {
    public static void main(String[] args) {
        Car car = new Car();
        car.startCar();
        // 출력
        // Engine started.
        // Car is moving.
    }
}
```

위의 예시처럼 각각의 기능 (엔진, 페달, 핸들, 브레이크 등)을 가진 클래스를 참조해 자동차를 완성하는 걸 합성이라고 보면 이해가 조금 더 쉬울 듯 하다.

확실히 상속에 비해 의존성이 낮고, 에러가 발생해도 유지보수 측면에서 훨씬 유리하다고 생각한다.

인터페이스 또한 합성처럼 여러 기능을 받아 (implements) 사용할 수 있는데, 자식 클래스에서 반드시 기능을 구현해야한다는 점이 어느정도 의존도를 부여한다.

<hr>

## 2. 인터페이스란?

>동작을 자식 클래스가 반드시 구현해 사용하도록 하는 것.

인터페이스는 **</font color="#990000">무엇을 할 수 있는지</font>**만 정의하고 **</font color="#990000">어떻게 하는지</font>**는 자식 클래스에서 정한다.

### - 인터페이스는 왜 사용할까?

>클래스에게 다중상속이 필요할 때 사용한다. (반드시 오버라이딩해서 사용해야 함)

**수행하는 동작은 유사하나 다양한 성격의 클래스가 필요할 경우**, 인터페이스를 통해 각 클래스가 인터페이스의 추상 메서드를 구현하여 클래스들을 하나의 자료형으로 묶어준다.

### - 코드 예시

```java
// 인터페이스: 날 수 있는 능력
interface Flyable {
    void fly();
}

// 클래스가 날 수 있도록 기능 부여
class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Bird is flying.");
    }
}

class Airplane implements Flyable {
    @Override
    public void fly() {
        System.out.println("Airplane is flying.");
    }
}
```

```java
public class InterfaceTest {
    public static void main(String[] args) {
        Flyable f1 = new Bird();
        Flyable f2 = new Airplane();

        f1.fly(); // Bird is flying.
        f2.fly(); // Airplane is flying.
    }
}
```

<hr>

## 3. 합성 vs 인터페이스

| 예시 | 추천 |
|------|------|
| 객체가 다양한 부품을 조립해서 기능을 사용하게 하고 싶다 | 합성 |
| 상속 대신 더 유연한 구조가 필요할 때 | 합성 |
| 동일한 기능을 여러 클래스에 부여하고 싶다 | 인터페이스 |
| 행위(기능)의 통일성을 강조하고 싶을 때 | 인터페이스 |

<hr>

**참고**

- [참고 1](https://velog.io/@ung6860/JAVA%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4Interface%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-fc81k7rr)
- [참고 2](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5%EC%9D%98-%EC%83%81%EC%86%8D-%EB%AC%B8%EC%A0%9C%EC%A0%90%EA%B3%BC-%ED%95%A9%EC%84%B1Composition-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)