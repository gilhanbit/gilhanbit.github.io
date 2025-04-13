---
title: "[CS] 프로그래밍 패러다임 : 선언형"
excerpt: "함수형 프로그래밍"

categories:
  - CS
tags:
  - [cs, programmingparadigm]

permalink: /cs/programmingparadigm/

toc: true
toc_sticky: true

date: 2025-04-13
last_modified_at: 2025-04-13
---

## 1. 프로그래밍 패러다임이란?

프로그래밍 패러다임은 프로그래머에게 프로그래밍의 관점을 갖게 해주는 역할을 하는 개발 방법론이다.

같은 문제를 풀더라도 **<font color="#990000">어떤 방식으로 설계하고 구현할 것인가</font>**를 결정하는 것을 의미한다.

<hr>

## 2. 함수형 프로그래밍 특징

함수형 프로그래밍(Functional Programming)은 수학적 함수의 계산을 기반으로 하여, 상태와 가변 데이터를 지양하는 선언형 프로그래밍 패러다임이다.

이는 프로그램을 명령의 집합이 아닌, 순수 함수들의 조합으로 구성하여 예측 가능하고 안정적인 코드를 작성하는 데 중점을 두고있다.


**<font color="#000099">순수 함수 (Pure Function)</font>**
동일한 입력에 대해 항상 동일한 출력을 반환하며, 함수 내부에서 인자의 값을 변경하거나 프로그램 상태를 변경하는 Side Effect가 없는 것.

`side-effect`란? 함수가 만들어진 목적과는 다른 효과 즉, 예상치 못한 일이 발생할 가능성이 있는 것을 말한다.
- 함수 외부의 변수의 값을 변경하는 행위
- 입력된 파라미터를 변경하는 행위
- 네트워크 요청을 보내는 행위 (예외나 오류가 발생할 수 있음)
- 데이터 입출력을 하는 항위 ()

**<font color="#000099">불변성 (Immutability)</font>**
데이터를 변경하지 않고, 필요 시 새로운 복사본을 생성하여 사용. 이는 상태 변화로 인한 오류를 줄이고, 병렬 처리에 유리하다.

**<font color="#000099">1급 객체로서의 함수 (First-Class Functions)</font>**
함수를 변수에 할당하거나, 다른 함수의 인자로 전달하거나, 반환값으로 사용할 수 있다. 이를 통해 고차 함수를 구현할 수 있다.

**<font color="#000099">참조 투명성 (Referential Transparency)</font>**
함수 호출을 해당 함수의 결과값으로 대체해도 프로그램의 동작에 영향을 주지 않는 특성. 이는 코드의 예측 가능성과 테스트 용이성을 높인다.

**<font color="#000099">선언형 프로그래밍 (Declarative Programming)</font>**
어떻게 수행할지를 명시하는 명령형 프로그래밍과 달리, 무엇을 수행할지를 말한다. 


### 장점

- **예측 가능성**: 부수 효과가 없으므로 함수의 동작을 쉽게 예측할 수 있다.
- **병렬 처리 용이성**: 상태 변경이 없으므로 동시적으로 일어나는 문제를 줄일 수 있다.
- **테스트 용이성**: 순수 함수는 독립적이므로 단위 테스트가 간편하다.
- **코드 재사용성**: 함수 조합을 통해 재사용이 용이하다.


### 단점

- **성능 이슈**: 불변성으로 인해 객체 복사가 빈번하게 발생할 수 있으며, 이에따른 성능 저하가 있을 수 있다.
- **디버깅의 어려움**: 상태 변화가 없기 때문에 디버깅 시 추적이 어려울 수 있다.

<hr>

## 3. 코드 예시

텍스트만으로는 이해가 쉽지 않아 gpt에게 예시 코드를 요청해봤다.


### 🧱 명령형 스타일
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class ImperativeExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        List<Integer> result = new ArrayList<>();

        for (Integer num : numbers) {
            if (num % 2 == 0) {
                result.add(num * num);
            }
        }

        System.out.println(result); // [4, 16]
    }
}
```
> 👷🏻‍♂️ "이렇게 반복해서 조건 검사하고 저장해!"  
> → 일일이 작업을 지시하는 방식.


### 🧼 함수형 스타일 (Java 8 스트림 + 람다)
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class FunctionalExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        List<Integer> result = numbers.stream()              // 스트림 생성
            .filter(n -> n % 2 == 0)                         // 짝수만 필터링
            .map(n -> n * n)                                 // 제곱
            .collect(Collectors.toList());                   // 리스트로 수집

        System.out.println(result); // [4, 16]
    }
}
```
> 🍳 "이런 재료들 가져와서 이런 식으로 처리해줘~"  
> → 선언형이고, 무엇을 할지를 설명하는 느낌.


| 비교 항목   | 명령형 스타일      | 함수형 스타일 (Java 8+)            |
| ----------- | ------------------ | ---------------------------------- |
| 코드 스타일 | 직접 지시          | 선언 방식                          |
| 데이터 처리 | for/if로 직접 조작 | `stream().filter().map()`으로 처리 |
| 가독성      | 복잡할 수 있음     | 간결하고 직관적                    |
| 변경 여부   | 리스트 직접 수정   | 원본 유지, 새 리스트 반환          |
| 주요 특징   | 상태 관리 중심     | 순수 함수 중심                     |

<hr>

자바 명령형 프로그래밍에 익숙해진 내게 선연형인 함수형 스타일은 아직은 전혀 다른 언어처럼 느껴졌다.

둘을 적재적소에 쓸 수 있을 때, 명확한 장단점을 느낄 수 있을 것 같고 아직까지는 익숙하지 않아 멀게 느껴질 뿐...

여기저기 자료를 찾아보며 느낀점은, 어떤 기능을 구현하고자 하는 것인지 쉽게 알 수 있을 것 같지만, 간단한 기능임에도 불변성인 특징으로 인해 코드가 길어질 수 있다는 것이다.

<hr>

**참고 자료**

- [참고1](https://jongminfire.dev/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%B4%EB%9E%80)
- [참고2](https://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
- [참고3](https://hik-coding.tistory.com/330)
- [참고4](https://www.youtube.com/watch?v=UZUBfwiXXPA)
