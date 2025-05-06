---
title: "[Spring] MVC Cycle 복습"
excerpt: "MVC와 더 친해지기"

categories:
  - Spring
tags:
  - [spring, mvccycle]

permalink: /categories4/mvccycle/

toc: true
toc_sticky: true

date: 2025-05-06
last_modified_at: 2025-05-06
---

## 1. MVC

>Model-View-Controller

웹개발에서 **<font color="##000099">구조를 역할별로 분리해서 보다 효율적으로 개발, 유지보수를 하기 위해 사용</font>**한다.

<u>처리 흐름으로 보면 View-Controller-Model</u>인데, MVC가 아니라 VCM이 맞는 거 아닐까 싶었다.

**그런데 왜 MVC라 부를까?**

결론부터 말하자면 **MVC는 처리 흐름이 아니라 관계 구조의 명칭**이다.

controller가 중심이 되어 model과 view를 관리하는 구조다.

즉, `model`과 `view`를 `controller`가 **연결하고 제어한다는 의미**에서 MVC라 부른다고 한다.

1. view: 사용자가 입력, 버튼 클릭 -> 요청 발생
2. controller: 요청을 받아 해석
3. model: 필요한 데이터 로직 실행
4. controller: 결과를 다시 view에 전달
5. view: 클라이언트에게 출력

<hr>

## 2. Model

- 데이터, 비지니스 로직 담당
- DB와 연동 및 연산 처리

**spring MVC**에서 **<font color="#000099">model은 데이터와 비지니스 로직을 담당하는 핵심 요소</font>**로 **실무에서는 DTO, service, repository 등으로 세분화 시켜 사용**한다.

이렇게 model을 세분화 하는 이유는, 책임을 분산-강화 시키고 협업과 유지보수를 원활하게 하기 위함이다.

### 2-1. Entity, DTO를 분리하는 이유

**엔터티는 DB 스키마와 1:1 매칭**되므로 **<font color="#990000">API 응답에 엔터티를 직접 리턴하면 보안 문제가 발생할 수 있다.</font>**

즉 DTO를 사용하는 이유는 필요한 데이터만 포함하고 유연한 유지보수를 위해서다.

```java
public class seller {

  private String nickname;
  private String profileImgUrl;
  private Double temperature;
  
  public String getNickname() {
    return nickname;
  }
  public void setNickname(String nickname) {
    this.nickname = nickname;
  }
  public String getProfileImgUrl() {
    return profileImgUrl;
  }
  public void setProfileImgUrl(String profileImgUrl) {
    this.profileImgUrl = profileImgUrl;
  }
  public Double getTemperature() {
    return temperature;
  }
  public void setTemperature(Double temperature) {
    this.temperature = temperature;
  }
}
```

### 2-2. Service를 사용하는 이유

controller에서 직접 repository를 호출하지 않고 service를 사용하는 이유가 있는데, 만약 controller에서 repository를 직접 호출한다면?

1. 컨트롤러가 비지니스 로직을 갖게 된다.
   1. 컨트롤러는 요청을 받아 모델에 전달하고 그 결과를 뷰로 리턴하는 역할이다. 그러나 컨트롤러에서 직접 레퍼지토리에서 데이터를 가져오는 역할까지 하게되면 컨트롤러의 책임이 커지고 유지보수가 어려워진다.
2. 비지니스 로직을 추가하기 어렵다.
   1. 컨트롤러에 직접 로직을 추가할 경우 다른 API에서 동일한 로직이 필요한 경우 중복 코드가 많아진다. 때문에 유지보수가 어렵고 비지니스 로직이 흘어져 코드의 일관성이 깨지게 된다.

```java
@Service
public class sellerBO {
  
  @Autowired
  private SellerMapper sellerMapper;
  
  public void insertSeller(String nickname, String profileImgUrl, Double temperature) {
    sellerMapper.insertSeller(nickname, profileImgUrl, temperature);
  }
  
  public seller sellerInfoView(Integer id) {
    if (id == null) {
      return sellerMapper.sellerInfoView();
    } else {
      return sellerMapper.sellerInfoSearch(id);
    }
  }
}
```

### 2-3. Repository를 분리하는 이유

데이터 접근을 추상화하여 유지보수성을 높이기 위함이 목적이다.

서비스가 레퍼지토리를 통해 데이터를 가져오면, DB가 바뀌어도 서비스 로직을 그대로 유지할 수 있다.

즉 JPA, MyBatis 등 다양한 방식으로 쉽게 변경이 가능하다.

```java
@Mapper
public interface SellerMapper {

  public void insertSeller(
      @Param("nickname") String nickname,
      @Param("profileImgUrl") String profileImgUrl,
      @Param("temperature") Double temperature);
  
  public seller sellerInfoView();
  
  public seller sellerInfoSearch(Integer id);
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.quiz.seller.mapper.SellerMapper">
  <insert id="insertSeller" parameterType="map">
    INSERT INTO `seller`
    (
      `nickname`
      ,`profileImgUrl`
      ,`temperature`
      ,`createdAt`
      ,`updatedAt`
    )
    VALUES
    (
      #{nickname}
      ,#{profileImgUrl}
      ,#{temperature}
      ,NOW()
      ,NOW()
    )
  </insert>
  
  <select id="sellerInfoView" resultType="com.quiz.seller.domain.seller">
    SELECT
      `nickname`
      ,`profileImgUrl`
      ,`temperature`
    FROM
      `seller`
    ORDER BY
      `id`
    DESC LIMIT
      1
  </select>
  
  <select id="sellerInfoSearch" resultType="com.quiz.seller.domain.seller">
    SELECT
      `nickname`
      ,`profileImgUrl`
      ,`temperature`
    FROM
      `seller`
    WHERE
      `id` = #{id}
  </select>
</mapper>
```

<hr>

## 3. View

- 클라이언트가 보는 화면
- 클라이언트의 입력을 받아 controller에 전달
- 모델의 데이터를 출력

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
<head>
<meta charset="UTF-8">
<title>판매자 추가</title>
  <!-- bootstrap -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
  <script src="https://code.jquery.com/jquery-3.7.1.js" integrity="sha256-eKhayi8LEQwp4NKxN+CfCh+3qOVUtJn3QNZ0TciWLP4=" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-Fy6S3B9q64WdZWQUiU+q4/2Lc9npb8tCaSX9FK7E8HnRr0Jz8D6OP9dO5Vg3Q9ct" crossorigin="anonymous"></script>
</head>
<body>
  <div class="container">
    <h1>판매자 추가</h1>
    <form method="post" action="/seller/afterAdd">
      <div class="form-group">
        <label for="nickname">닉네임</label>
        <input type="text" id="nickname" name="nickname" class="form-control col-4">
      </div>
      <div class="form-group">
        <label for="profileImgUrl">프로필 사진</label>
        <input type="text" id="profileImgUrl" name="profileImgUrl" class="form-control">
      </div>
      <div class="form-group">
        <label for="temperature">매너 온도</label>
        <input type="text" id="temperature" name="temperature" class="form-control col-4">
      </div>
      
      <input type="submit" value="추가" class="btn btn-primary">
    </form>
  </div>
</body>
</html>
```
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
<head>
<meta charset="UTF-8">
<title>판매자 정보</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
  <script src="https://code.jquery.com/jquery-3.7.1.js" integrity="sha256-eKhayi8LEQwp4NKxN+CfCh+3qOVUtJn3QNZ0TciWLP4=" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-Fy6S3B9q64WdZWQUiU+q4/2Lc9npb8tCaSX9FK7E8HnRr0Jz8D6OP9dO5Vg3Q9ct" crossorigin="anonymous"></script>
</head>
<body>
  <div class="container">
    <h1>판매자 정보</h1>
    <img th:src="${seller.profileImgUrl}" width="200">
    <div class="h1" th:text="${seller.nickname}"></div>
    <div class="text-warning h3 font-weight-bold" th:text="${seller.temperature}"></div>
  </div>
</body>
</html>
```

<hr>

## 4. Controller

- model-view를 연결
- 클라이언트의 요청을 model에 전달
- model의 데이터를 view에 전달

```java
@RequestMapping("/seller")
@Controller
public class SellerController {
	
	@Autowired
	private sellerBO sellerBO;
	
	@RequestMapping("/add")
	public String addSellerView() {
		return "seller/addSeller";
	}
	
	@PostMapping("/afterAdd")
	public String addSeller(
			@RequestParam("nickname") String nickname,
			@RequestParam("profileImgUrl") String profileImgUrl,
			@RequestParam("temperature") Double temperature) {
		
		sellerBO.insertSeller(nickname, profileImgUrl, temperature);
		
		return "seller/afterAddSeller";
	}
	
	@GetMapping("/info")
	public String sellerInfoView(
			@RequestParam(value = "id", required = false) Integer id,
			Model model) {
		
		seller seller = sellerBO.sellerInfoView(id);
		model.addAttribute("seller", seller);
		
		return "seller/sellerInfo";
	}
}
```

<hr>

**참고**

- [참고](https://velog.io/@chlek95/Spring-MVC-%EB%8D%B0%EC%9D%B4%ED%84%B0-%ED%9D%90%EB%A6%84-Entity-DTO-Controller-Service-Repository)
