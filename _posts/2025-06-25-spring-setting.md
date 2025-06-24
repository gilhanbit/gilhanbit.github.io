---
title: "[Project] 스프링 프레임워크를 이용한 웹개발 프로젝트"
excerpt: "개인용 환경설정"

categories:
  - Spring
tags:
  - [spring, web, project]

permalink: /spring/setting/

toc: true
toc_sticky: true

date: 2025-06-25
last_modified_at: 2025-06-25
---

## 환경세팅 (개인 저장용)

1. https://start.spring.io/ 프로젝트 생성
2. 아래 Dependencies 선택
   * Spring Web
   * Spring boot dev tools
   * Thymeleaf
   * MySQL Driver
   * Mybatis Framework
   * Spring Data JPA
   * Lombok (추가로 플러그인 설치 필요 file-setting-plugin)
![롬복플러그인](/assets/images/posts_img/spring/setting/lombok.png)
3. 동작 확인 (controller / html)
   * ProjectnameApplication 코드 입력 -> @EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class}) -> DB 설정 잠시 생략
4. build.gradle / application.yml file setting
![파일세팅](/assets/images/posts_img/spring/setting/gradle.png)
5. 기능 테스트
   * @ResponseBody
   * Thymeleaf
   * Mybatis
   * DB
