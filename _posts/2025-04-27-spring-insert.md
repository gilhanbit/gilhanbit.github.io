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
  <!-- insert 태그에 resultType을 사용할 경우 컴파일에러 발생 -->
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

<hr>

## 5. insert 후 PK로 입력 데이터 리턴 받기

클라이언트가 회원가입을 하고 제대로 회원가입을 했는지 입력한 아이디, 닉네임, 연락처, 주소 등을 보여주려면?

int말고 입력(저장)한 데이터를 리턴 받아야 한다.

<u>그럼 다시 select으로 조회해서 받아야 하나? 너무 비효율적인데?</u>

이럴 때 `useGeneratedKeys`, `keyProperty`를 사용하면 된다.

아래는 레이어별 수정 코드다.

- [참고 1](https://seyuuu.tistory.com/12)
- [참고 2](https://maivve.tistory.com/348)

### xml

`insert` 태그에 `useGeneratedKeys`, `keyProperty`를 추가한다.

- useGeneratedKeys: 자동 생성된 PK를 넘겨준다.
- keyProperty: PK을 담을 변수

나는 만들어둔 DTO의 id에 그대로 담아 조회하면 되기 때문에 id로 만들었다.

**<font color="#990000">여기서 주의할 점! 비밀번호 등 암호화 시킨 데이터는 절대로 리턴하면 안된다.</font>**

```xml
<insert id="insertRealEstate" parameterType="com.quiz.lesson03.domain.realEstate" useGeneratedKeys="true" keyProperty="id">
```

### Mapper

<u>파라미터 타입이 달라졌으니 오버로딩으로 같은 메서드를 써도 될까?</u>

**결론은 불가능하다.**

**<font color="#990000">mybatis에서는 오버로딩이 불가능하기 때문이다.</font>**

즉, select 태그로 조회 쿼리문을 만들고 새로운 메서드와 연결해줘야 한다.

다시 select으로 조회하는 건 비효율적이라며? -> 이건 클라이언트가 다시 조회해야 한다는 걸 뜻한다.

쉽게 말해서 **'회원가입 -> 가입 축하 -> 마이페이지 조회' 프로세스**에서 **'가입 축하 + 마이페이지 정보'**를 보여주는 것이다.

```java
public realEstate selectRealEstate(int id);
```

### Service

```java
public realEstate addRealEstate(realEstate realEstate) {
  realEstateMapper.insertRealEstate(realEstate) // 데이터 입력
  return realEstateMapper.selectRealEstate(realEstate.getId()); // 입력 결과 출력
}
```

### Controller

```java
@RequestMapping("/1")
public String quiz02_1() {
  realEstate realEstate = new realEstate();
  realEstate.setRealtorId(3);
  realEstate.setAddress("푸르지오 리버 303동 1104호");
  realEstate.setArea(89);
  realEstate.setType("매매");
  realEstate.setPrice(100000);

  return realEstateBO.addRealEstate(realEstate); // 자동 생성된 PK로 조회한 데이터 출력
}
```
