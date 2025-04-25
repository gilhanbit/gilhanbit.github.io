---
title: "[Java] 자바 용어 정리"
excerpt: "꼭 알아야 하는 자바 용어"

categories:
  - Java
tags:
  - [java]

permalink: /java/terms/

toc: true
toc_sticky: true

date: 2025-04-25
last_modified_at: 2025-04-25
---

## 1. 클래스 (Class)
  
>클래스는 **객체를 만들기 위한 설계도**

```java
public class car {
    String name;
  
    void start() {
        System.out.println("부릉!");
   }
}
```

'car'라는 자동차에 대한 설계도(클래스)다.

<u>car 클래스로 'bmw', 'volvo', 'ford' 같은 객체를 만들 수 있다.</u>

<hr>

## 2. 객체 (Object)

>클래스로 구현한 것을 말한다.

클래스, 객체, 인스턴스에 대해 처음 들었을 때 '모두 같은 거 아니야?' 하고 생각할 수 있다.

```java
/* 여기서부터
public class car <- 객체 {
    String name;

   void start() {
      System.out.println("부릉!");
   }
}
여기까지가 클래스 */
```

클래스에서 본 코드로 다시 한번 설명하자면, 'car'라는 단어는 객체다.

"클래스 아니야?"

틀린 말은 아니지만, 굳이 구분하자면 **'클래스'라는 것은 아래 필드, 메서드를 포함한 전체**를 **'객체'는 car**를 말한다.

<hr>

## 3. 인스턴스 (Instance)

>클래스에서 생성된 '한 객체'를 인스턴스라고 부른다.

객체와 거의 같은 뜻인데, 단어 자체로는 '구현된 실체'라는 뜻.

```java
Car bmw = new Car();
```

자동차 설계도(클래스)로 찍어낸 'bmw'가 인스턴스.

그래서 **car의 인스턴스라고 부른다.**

'bmw'를 객체라 부를 수도 있지만, 클래스에서 나온 '실제 결과물'이 인스턴스다.

<u>객체와 인스턴스를 구분하는 것은 클래스로부터 생성되었다는 것에 의미를 부여하는 것일 뿐, 엄격하게 객체와 인스턴스를 나누는 것은 어렵다.</u>

![참고](/assets/images/posts_img/terms/ggoomter.png)

클래스, 객체, 인스턴스에 대해 정말 쉽게 설명한 댓글이 아닐까 싶다.

- [원글](https://www.codeit.kr/community/questions/UXVlc3Rpb246NWUzNDUyMjU4MGU1MTMzNzNkOTYyMzZj)

<hr>

## 4. 변수 (Variable)

>데이터를 저장하는 공간을 말한다.

즉, 값을 담을 수 있는 '그릇'

```java
int age = 25;
```

'age'라는 변수에 25를 담음.

<hr>

## 5. 메서드 (Method)

>클래스 안에서 정의되는 '기능'

**객체가 수행할 수 있는 수행 할 동작을 정의**한다.

비유하자면 리모컨의 버튼에 해당한다.

'전원 켜기' 버튼을 누르면 TV가 켜지는 것처럼, 메서드를 호출하면 할당한 기능이 수행된다.

```java
void start() {
    System.out.println("부릉!");
}
```

위는 start 메서드(시동 버튼)로, 누르면 부릉!이 출력된다.

<hr>

## 6. 생성자 (Constructor)

>객체가 생성될 때 딱 한 번 호출되는 메서드.

```java
public Dog(String name) {
    this.name = name;
}
```

클래스 이름과 같고, return 타입이 없다.

생성자를 정의하지 않을 경우, 파라미터가 없는 기본 생성자가 내부적으로 만들어져 호출된다.

<hr>

## 7. 상속 (Inheritance)

>부모 클래스의 기능을 자식 클래스가 물려받아 사용하는 것.

```java
class animal {
    void eat() {
      System.out.println("먹는다");
    }
}

class dog extends animal {
    void bark() {
      System.out.println("멍멍!");
    }
}
```

dog는 animal의 eat 메서드와 자신이 갖고있는 'bark'도 사용할 수 있다.

<hr>

## 8. 오버라이딩 (Overriding)

>부모 클래스에서 물려받은 메서드를 자식 클래스에서 고쳐 사용하는 것.

```java
class dog extends animal {
    void bark() {
        System.out.println("멍멍!");
    }
    
    @Override
    void eat() {
        // System.out.println("멍멍!");
        System.out.println("강아지는 사료를 먹어요.");
    }
}
```

<hr>

## 9. 오버로딩 (Overloading)

>클래스 내에 존재하는 메서드의 파라미터 `타입` or `개수`를 다르게 하여 사용하는 것.

오버로딩은 사전적으로 '과적하다'라는 뜻으로, C언어에서는 함수명이 중복되면 안되지만 자바에서는 하나의 메서드 이름으로 여러 기능을 구현하기 때문에 이 이름을 붙여준 것으로 보인다고 한다.

```java
void print(String s) {}
void print(int i) {} // 타입이 다르거나
void print(String s, String s) {} // 개수가 다르거나
```

<hr>

## 10. 추상 클래스 (Abstract Class)

>일부 메서드는 구현되어 있지만, 일부는 **자식 클래스가 꼭 구현하도록** 남겨두는 클래스.

<font color="#990000">추상 클래스는 직접 객체 생성이 불가(인스턴스도 불가)</font>하며, <u>상속받는 클래스만 객체 생성이 가능</u>하다.

```java
abstract class animal {
    void sleep() {
        System.out.println("잔다");
    }

    abstract void sound(); // 구현은 자식 클래스에서
}
```

```java
class dog extends animal {
    void sound() {
        System.out.println("멍멍");
    }
}
```

<hr>

## 11. 인터페이스 (Interface)

>모든 메서드가 구현 없이 "이름만 있는" 약속.

**구현은 무조건 다른 클래스가 해야 한다.**

```java
interface RemoteControl {
    void turnOn();
    void turnOff();
}
```

```java
class TV implements RemoteControl {
    public void turnOn() { System.out.println("TV ON"); }
    public void turnOff() { System.out.println("TV OFF"); }
}
```

**추상 클래스와 인터페이스 비슷해 보이는데... 차이는?**

이 비유가 적절할지 모르지만, 자동차로 예를들어 보겠다.

자동차에는 엔진, 페달, 브레이크 등이 있다.

'추상 클래스'는 전세계가 <u>약속한 규칙을 갖고 엔진, 페달, 브레이크는 반드시 이 규격으로 만들어야 하고, 제조사들에게 커스텀 가능한 일정 부분만 남겨두는 것</u>이다.

'인터페이스'는 <u>엔진, 페달, 브레이크 등 반드시 있어야 하는 것만 정해주고, 제조사 너희들 입맛에 맞게 알아서 만들어!</u> 하는 것이다.

<hr>

## 12. static

>객체를 만들지 않아도 클래스 이름만으로 사용할 수 있게 해주는 키워드.

클래스 전체에서 공통으로 사용되는 변수나 메서드에 붙는다.

```java
public class calculator {
    public static int add(int a, int b) {
        return a + b;
    }
}
```

`Calculator.add(3, 5);` 식으로 객체 없이 바로 호출이 가능하다.

<hr>

## 13. final

>더 이상 변경되지 못하도록 막는 키워드.

변수, 메서드, 클래스에 모두 사용 가능하다.

- `final 변수` = 값 변경 불가
- `final 메서드` = 오버라이딩 불가
- `final 클래스` = 상속 불가

```java
final int maxValue = 100; // 값 변경 불가

final class one {} // 상속 불가
```

<hr>

## 14. JDK / JRE

- **JDK (Java Development Kit)**
JRE + 개발 도구(컴파일러 등) (책상 + 책 + 연필, 자, 지우개 (개발할 도구까지 포함))

- **JRE (Java Runtime Environment)**
JVM + 실행에 필요한 자바 라이브러리 (책상 + 책)

<hr>

## 15. JVM (Java Virtual Machine)

>자바 코드를 실행하는 가상의 컴퓨터.

자바 코드를 운영체제가 알아들을 수 있도록 바꿔준다.

덕분에 **<font color="#990000">"Write Once, Run Anywhere"</font>**이 가능한 것이다.

- 컴파일러는 `.java → .class`로 번역  
- JVM은 `.class → 실행` 시켜줌

<hr>

## 16. 예외 (Exception) 처리

>프로그램 실행 중 예상치 못한 문제가 발생했을 때 처리하는 방식 중 하나.

문제가 생겨도 프로그램이 멈추지 않고, 문제가 발생한 구간을 건너뛰고 실행한다.

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("0으로 나눌 수 없습니다.");
}
```

<hr>

## 17. 컬렉션 (Collection)

>여러 데이터를 하나로 묶어서 저장하고 관리할 수 있게 해주는 구조.

대표적으로 `List`, `Set`, `Map`이 있다.

**List**

>순서 있음, 중복 허용 (같은 사람도 여러 번 들어올 수 있음)

```java
List<String> list = new ArrayList<>();
list.add("홍길동");
list.add("홍길동"); // 가능
```

**Set**

>순서 없음, 중복 허용 안 함 (참석자 명단)

```java
Set<String> set = new HashSet<>();
set.add("홍길동");
set.add("홍길동"); // 무시됨
```

**Map**

>키(Key)-값(Value) 쌍으로 저장 ('키'로 값을 꺼낸다)

```java
Map<String, Integer> map = new HashMap<>();
map.put("홍길동", 100);
System.out.println(map.get("홍길동")); // 100
```

<hr>

## 18. 가비지 컬렉터 (Garbage Collector)

>더 이상 쓰지 않는 객체를 메모리에서 자동으로 지워주는 시스템.

<hr>

## 19. 스트림 (Stream API)

>데이터 처리(필터링, 정렬, 변환 등)를 연결해서 처리할 수 있게 해주는 기능.

반복문 없이도 데이터 흐름을 자연스럽게 처리할 수 있다.

```java
List<String> names = List.of("홍길동", "이몽룡", "성춘향");

names.stream()
    .filter(name -> name.startsWith("홍"))
    .map(String::toUpperCase)
    .forEach(System.out::println);
```

"홍"으로 시작하는 이름을 대문자로 바꿔서 출력.

<hr>

## 20. 어노테이션 (Annotation)

>자바 코드에 메타데이터(설명)를 추가하는 문법.

```java
@Override // 어노테이션
public String toString() {
    return "이름: 홍길동";
}
```

`@Override`는 "이 메서드는 부모 메서드를 오버라이딩한 거야"라고 알려준다.

스프링에선 더 자주 활용된다.
