---
title: "[Spring] select에 대한 이해"
excerpt: "DB로부터 데이터를 가져와 view에 출력하기"

categories:
  - Spring
tags:
  - [spring, select]

permalink: /spring/select/

toc: true
toc_sticky: true

date: 2025-04-26
last_modified_at: 2025-04-26
---

우리가 사용하고 있는 `웹`, `앱` 심지어 일상생활에서도 이 패턴은 자연스럽게 사용되고 있다.

식당에서 손님이 웨이터, 키오스크에 주문을 하고 주문이 주방으로 전달되고 받은 주문에 맞는 재료를 꺼내 요리를 하고 웨이터, 서빙봇이 받아 손님 식탁에 놓기까지의 과정은 너무나 자연스럽고 당연했다.

데이터도 크게 다르지 않다.

완벽하게 이해하면 쉽고 당연하게 느껴지겠지만 그 전까지는 미로처럼 느껴질지도 모른다.

이 글은 완벽한 이해로 가기위한 과정이다.


<hr>


## 1. Spring MVC pattern

![springmvcpattern](/assets/images/posts_img/spring-select/springmvc.png)

view가 우리 눈앞에 보이는 화면이다.

그리고 이 화면은 우리에게 보이지 않는 DB로 부터 나온다.


### 1-1. Presentation Layer

>view 화면을 나타내기 위한 계층

- controller


### 1-2. Business Layer

>비즈니스 로직(데이터 가공 및 처리)이 위치한 계층

- service
- BO (Business Object)


### 1-3. Persistence Layer

>데이터를 DB에 저장해두고 지속적으로 사용할 수 있도록 하는 계층

- repository
- DAO (Data Access Object)
- mapper (interface)


### 1-4. Entity

>DB의 테이블 데이터를 담기 위한 객체

`field`, `method`만 갖는 순수한 java bean이다.

**테이블 column과 일치하는 field로만 구성**


### 1-5. DTO (Data Transfer Object)

entity 개념을 포함하고 있다.

굳이 entity와 구분을 하자면, entity는 본래 DB의 table을, DTO는 더하거나 뺀(수정) 객체를 말한다.

<u>DTO는 `domain`, `model` 등으로 불리기도 한다.</u>


<hr>


## 2. Controller

view는 주로 FE의 영역이므로 controller부터 이해해보자.


### 2-1. View 파일로 응답

```java
@Controller // HTML로 보내는 컨트롤러 @ResponseBody가 있으면 안됨
public class Lesson01Ex02Controller {

@RequestMapping("/lesson01/ex02")
public String ex02() {
  // return되는 String은 html의 경로이다. (@ResponseBody가 없을 때)
  // /templates/lesson01/ex02.html -> 타임리프가 앞뒤 생략 후 주소 바꿔줌 lesson01/ex02
  return "lesson01/ex02"; // response html view 경로 (파일 경로 / 위 Mapping 주소와 아무런 연관이 없음)
  
  // @ResponseBody가 아닌 상태로 String을 return하면 viewresolver가 경로를 찾아 해당 파일이 보여짐
}
}
```


### 2-2. HTML or JSON으로 응답

```java
// parameter (X)
@RestController // view를 별도로 만들지 않았을 때 리턴하겠다는 스프링빈 어노테이션
public class lesson02Quiz01Controller {
	
	@Autowired // 개발자가 직접 new를 하지 않아도 new를 해서 객체를 사용할 수 있다. (DI)
	private storeBO storeBO;
	
	@RequestMapping("/lesson02/quiz01") // 해당 URI로 연결
	public List<store> quiz01() {
		return storeBO.getStoreList(); // view에 출력 (쿼리문에 따라 달라짐)
	}
}


// parameter (O)
@RequestMapping("/lesson03/quiz01") // 컨트롤러 위에서 mapping시 메서드들 공통 주소를 만들 수 있다.
@RestController
public class lesson03Quiz01RestController {
	
	@Autowired
	public realEstateBO realEstateBO;
	
	@RequestMapping("/1")
	public realEstate quiz01_1(
			@RequestParam(value = "id", defaultValue = "1") int id
      // 파라미터가 있을 경우 ex) http://localhost/lesson03/quiz01/1?id=(value) / 파라미터 타입은 int
	) {
		return realEstateBO.getRealEstateById(id); // BO에게 id v 전달 및 리턴
	}
	
	@RequestMapping("/2")
	public List<realEstate> quiz01_2(
			@RequestParam(value = "rent_price", required = false) Integer rentPrice
	) {
		if (rentPrice == null) { // 파라미터가 null일 때의 조건문
			return realEstateBO.getRealEstateAll();
		}
		return realEstateBO.getRealEstateByRentPrice(rentPrice);
	}
	
	@RequestMapping("/3")
	public List<realEstate> quiz01_3(
			@RequestParam(value = "area", required = false) Integer area,
			@RequestParam(value = "price", required = false) Integer price
	) {
		if (area == null || price == null) {
			return realEstateBO.getRealEstateAll();
		}
		return realEstateBO.getRealEstate(area, price);
	}
}
```


<hr>


## 3. Service

```java
@Service
public class realEstateBO {
	
	@Autowired
	public realEstateMapper realEstateMapper;
	
	public realEstate getRealEstateById(int id) { // controller와 소통
		return realEstateMapper.selectRealEstateById(id); // mapper와 소통
	}
	
	public List<realEstate> getRealEstateByRentPrice(Integer rentPrice) {
		return realEstateMapper.selectRealEstateByRentPrice(rentPrice);
	}
	
	public List<realEstate> getRealEstateAll() {
		return realEstateMapper.selectRealEstateAll();
	}
	
	public List<realEstate> getRealEstate(Integer area, Integer price) {
		return realEstateMapper.selectRealEstate(area, price);
	}
	
	public boolean addRealEstate(realEstate realEstate) {
		return realEstateMapper.insertRealEstate(realEstate);
	}
}
```


<hr>


## 4. Repository (Mapper)

```java
@Mapper
public interface realEstateMapper {
	
	public realEstate selectRealEstateById(int id); // xml과 소통
	
	public List<realEstate> selectRealEstateByRentPrice(Integer rentPrice);
	
	public List<realEstate> selectRealEstateAll();
	
	public List<realEstate> selectRealEstate(
			// xml로 파라미터를 보낼 때 한개만 가능
			// 즉, 2개 이상일 시 Map으로 담아 보내야함
			// @Param 어노테이션을 붙이면 하나의 Map이 됨.
			@Param("area") Integer area,
			@Param("price") Integer price
	);
	
	public boolean insertRealEstate(realEstate realEstate);
}
```


**n개일 때 @Param을 사용해야 하는 이유**는 아래 글 참고

- [참고](https://blog.naver.com/hj_kim97/222739563456)


<hr>


## 5. .xml (query)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd"> <!-- mybatis 문법을 사용하겠다는 의미 -->


<mapper namespace="com.quiz.lesson03.mapper.realEstateMapper"> <!-- mapper 인터페이스 경로 -->
	
	<select id="selectRealEstateById" parameterType="int" resultType="com.quiz.lesson03.domain.realEstate">
  <!-- id: mapper 인터페이스의 메서드 / parameterType: v 타입 / resultType: 조회된 값을 저장할 DTO 경로 -->
  <!-- 즉, insert만 하고 리턴이 없을 경우 resultType은 사용 X -->
		SELECT
			`id`
			,`realtorId`
			,`address`
			,`area`
			,`type`
			,`price`
			,`rentPrice`
			,`createdAt`
			,`updatedAt`
		FROM
			`real_estate`
		WHERE
			`id` = #{id}
	</select>
	
	<select id="selectRealEstateByRentPrice" parameterType="Integer" resultType="com.quiz.lesson03.domain.realEstate">
		SELECT
			`id`
			,`realtorId`
			,`address`
			,`area`
			,`type`
			,`price`
			,`rentPrice`
			,`createdAt`
			,`updatedAt`
		FROM
			`real_estate`
		WHERE
			`rentPrice` &lt;= #{rentPrice}
	</select>
	
	<select id="selectRealEstateAll" resultType="com.quiz.lesson03.domain.realEstate">
		SELECT
			`id`
			,`realtorId`
			,`address`
			,`area`
			,`type`
			,`price`
			,`rentPrice`
			,`createdAt`
			,`updatedAt`
		FROM
			`real_estate`
	</select>
	

	<!-- 2개 이상은 parameterType 생략 가능, 그러나 어떤 타입으로 넘어오는지 (공부를 위해) 익숙해지기 위함 -->
	<select id="selectRealEstate" parameterType="map" resultType="com.quiz.lesson03.domain.realEstate">
		SELECT
			`id`
			,`realtorId`
			,`address`
			,`area`
			,`type`
			,`price`
			,`rentPrice`
			,`createdAt`
			,`updatedAt`
		FROM
			`real_estate`
		WHERE
			`type` = '매매'
		AND
			`area` &gt;= #{area}
		AND
			`price` &lt;= #{price}
	</select>
	
	<insert id="insertRealEstate" parameterType="com.quiz.lesson03.domain.realEstate">
		INSERT INTO `real_estate`
		(
			`realtorId`
			,`address`
			,`area`
			,`type`
			,`price`
			,`rentPrice` <!-- null이더라도 코드 중복을 방지(재사용성 높임)하기 위해 table column은 써준다. -->
			,`createdAt`
			,`updatedAt`
		)
		VALUES
		(
			#{realtorId}
			,#{address}
			,#{area}
			,#{type}
			,#{price}
			,#{rentPrice}
			,NOW()
			,NOW()
		)
	</insert>

</mapper>
```
