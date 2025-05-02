---
title: "[Spring] 입력정보 return 하기"
excerpt: "usegeneratedkeys를 사용해서 입력정보 return 하기"

categories:
  - Spring
tags:
  - [spring, usegeneratedkeys]

permalink: /spring/useGeneratedKeys/

toc: true
toc_sticky: true

date: 2025-05-02
last_modified_at: 2025-05-02
---

## 1. 목적

신규 가입자 정보를 입력 -> DB 추가 -> 생성된 ID로 return -> 클라이언트 화면에 출력

<hr>

## 2. 조건

- 입력 form 으로 value -> DB에 추가
- @ModelAttribute로 parameter를 받는다.
- generatedkeys 사용
- thymeleaf로 return

![클라이언트](/assets/images/posts_img/spring/usegeneratedkeys/view.png)
![리턴](/assets/images/posts_img/spring/usegeneratedkeys/return.png)

<hr>

## 3. 코드

### 3-1. Controller

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;

@RequestMapping("/realtor")
@Controller
public class realtorController {


  @Autowired
  private realtorBO realtorBO;


  @RequestMapping("/add-realtor")
  public String addRealtor() {
    return "realtor/addRealtor";
  }


  @PostMapping("/after-add-realtor")
  public String afterAddRealtor(
      @ModelAttribute realtor realtor,
      Model model) {
    
    realtor = realtorBO.addRealtor(realtor); //  get value -> realtor에 저장
    
    model.addAttribute("realtor", realtor); // DB에서 데이터를 꺼내 출력해주기 위해 model에 DTO 저장
    
    return "realtor/afterAddRealtor"; // return html path
  }
}
```

### 3-2. BO

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class realtorBO {

  @Autowired
  private realtorMapper realtorMapper;


  public realtor addRealtor(realtor realtor) {
    realtorMapper.addRealtor(realtor);
    return realtorMapper.selectRealtor(realtor.getId()); // keyProperty="id" get
  }
}
```

### 3-3. Mapper

```java
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface realtorMapper {

  public void addRealtor(realtor realtor);


  public realtor selectRealtor(int id);
}
```

### 3-4. xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.realtor.realtorMapper">
  <insert id="addRealtor" parameterType="com.example.realtor.realtor" useGeneratedKeys="true" keyProperty="id">
    INSERT INTO `realtor`
    (
      `office`
      ,`phoneNumber`
      ,`address`
      ,`grade`
      ,`createdAt`
      ,`updatedAt`
    )
    VALUES
    (
      #{office}
      ,#{phoneNumber}
      ,#{address}
      ,#{grade}
      ,NOW()
      ,NOW()
    )
  </insert>


  <select id="selectRealtor" parameterType="int" resultType="com.example.realtor.realtor">
    SELECT
      `id`
      ,`office`
      ,`phoneNumber`
      ,`address`
      ,`grade`
      ,`createdAt`
      ,`updatedAt`
    FROM
      `realtor`
    WHERE
      `id` = #{id}
  </select>
</mapper>
```

### 3-5. view

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
  <head>
    <meta charset="utf-8">
    <title>Add Realtor</title>
    <!-- bootstrap -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.7.1.js" integrity="sha256-eKhayi8LEQwp4NKxN+CfCh+3qOVUtJn3QNZ0TciWLP4=" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-Fy6S3B9q64WdZWQUiU+q4/2Lc9npb8tCaSX9FK7E8HnRr0Jz8D6OP9dO5Vg3Q9ct" crossorigin="anonymous"></script>
  </head>
  <body>
    <div class="container">
      <h1>공인중개사 추가</h1>
      <form method="post" action="/realtor/after-add-realtor">
        <div>
          <label>상호명</label>
          <input type="text" id="office" name="office" class="form-control col-6">
        </div>
        <div>
          <label>전화번호</label>
          <input type="text" id="phoneNumber" name="phoneNumber" class="form-control col-6">
        </div>
        <div>
          <label>주소</label>
          <input type="text" id="address" name="address" class="form-control col-6">
        </div>
        <div>
          <label>등급</label>
          <input type="text" id="grade" name="grade" class="form-control col-6">
        </div>
        
        <input type="submit" value="추가" class="btn btn-primary">
      </form>
    </div>
  </body>
</html>
```

### 3-6. return

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
  <head>
    <meta charset="utf-8">
    <title>New Realtor</title>
    <!-- bootstrap -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.7.1.js" integrity="sha256-eKhayi8LEQwp4NKxN+CfCh+3qOVUtJn3QNZ0TciWLP4=" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-Fy6S3B9q64WdZWQUiU+q4/2Lc9npb8tCaSX9FK7E8HnRr0Jz8D6OP9dO5Vg3Q9ct" crossorigin="anonymous"></script>
  </head>
  <body>
    <div class="container">
      <h1>공인중개사 정보</h1>
      <table class="table">
        <tr>
          <th>ID</th>
          <td th:text="${realtor.id}"></td>
        </tr>
        <tr>
          <th>상호명</th>
          <td th:text="${realtor.office}"></td>
        </tr>
        <tr>
          <th>전화번호</th>
          <td th:text="${realtor.phoneNumber}"></td>
        </tr>
        <tr>
          <th>주소</th>
          <td th:text="${realtor.address}"></td>
        </tr>
        <tr>
          <th>등급</th>
          <td th:text="${realtor.grade}"></td>
        </tr>
      </table>
    </div>
  </body>
</html>
```

<hr>

## 4. 흐름도

클라 -> controller -> BO -> mapper -> repository (xml) -> DB -> repository -> mapper -> BO -> controller -> model -> view -> 클라 (DTO 형태로 데이터 전달)

![MVC](/assets/images/posts_img/spring/usegeneratedkeys/MVC.png)
