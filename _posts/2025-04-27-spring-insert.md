---
title: "[Spring] insert에 대한 이해"
excerpt: "데이터를 DB에 넣어보자"

categories:
  - Spring
tags:
  - [spring, insert]

permalink: /spring/insert/

toc: true
toc_sticky: true

date: 2025-04-27
last_modified_at: 2025-04-27
---

DB에 데이터를 저장하는 건 DB나 데이터를 관리하는 네 가지 기본 작업 CRUD(Create, Read, Update, Delete) 중 하나다.

insert는 create에 해당하는 작업으로 신규 데이터를 DB에 저장한다.

<hr>

## 1. Controller

```java
@RequestMapping("/lesson03/quiz02")
@RestController
public class lesson03Quiz02RestController {

  @Autowired
  private realEstateBO realEstateBO;


  @RequestMapping("/1") // getter, setter로 넣기
  public String quiz02_1() {
    realEstate realEstate = new realEstate();
    realEstate.setRealtorId(3);
    realEstate.setAddress("푸르지오 리버 303동 1104호");
    realEstate.setArea(89);
    realEstate.setType("매매");
    realEstate.setPrice(100000);
    
    boolean check = realEstateBO.addRealEstate(realEstate); // boolean은 Mybatis에서 제공하는 리턴타입은 아니다. void, int만 제공
    
    return "성공?" + " " + check;
  }


  @RequestMapping("/2") // 파라미터로 넣기
  public String quiz02_2(@RequestParam(value="realtor_id", defaultValue = "5") int realtorId) {
    int rowCount = realEstateBO.addRealEstateAsField(realtorId, "쌍떼빌리버 오피스텔 814호", 45, "월세", 100000, 120); // int는 입력 성공한 row개수를 리턴한다.
    
    return "success :" + rowCount;
  }
}
```

<hr>

## 2. Service

```java
@Service
public class realEstateBO {

@Autowired
public realEstateMapper realEstateMapper;


public boolean addRealEstate(realEstate realEstate) { // 객체로 받는다.
  return realEstateMapper.insertRealEstate(realEstate);
}


public int addRealEstateAsField(int realtorId, String address, int area, String type, int price, Integer rentPrice) {
  return realEstateMapper.insertRealEstateAsField(realtorId, address, area, type, price, rentPrice);
}
}
```

<hr>

## 3. Mapper

```java
@Mapper
public interface realEstateMapper {

  public boolean insertRealEstate(realEstate realEstate);

  public int insertRealEstateAsField(
    @Param("realtorId") int realtorId,
    @Param("address") String address, 
    @Param("area") int area, 
    @Param("type") String type, 
    @Param("price") int price, 
    @Param("rentPrice") Integer rentPrice);
}
```

<hr>

## 4. .xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.quiz.lesson03.mapper.realEstateMapper">

  <insert id="insertRealEstate" parameterType="com.quiz.lesson03.domain.realEstate">
  <!-- insert에 resultType을 사용할 경우 컴파일에러 발생 -->
    INSERT INTO `real_estate`
    (
      `realtorId`
      ,`address`
      ,`area`
      ,`type`
      ,`price`
      ,`rentPrice`
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

  <insert id="insertRealEstateAsField" parameterType="map">
    INSERT INTO `real_estate`
    (
      `realtorId`
      ,`address`
      ,`area`
      ,`type`
      ,`price`
      ,`rentPrice`
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
