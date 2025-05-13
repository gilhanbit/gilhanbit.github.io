---
title: "[Spring] 검사 기능 만들기"
excerpt: "AJAX로 중복검사 하기"

categories:
  - Spring
tags:
  - [spring, ajax]

permalink: /spring/duplicate/

toc: true
toc_sticky: true

date: 2025-05-13
last_modified_at: 2025-05-13
---

회원가입 시 아이디, 이메일 등 중복검사를 위한 기능을 구현하기 전에 이런 기능을 구현할 수 있는 AJAX에 대해 간단히 알아보자.

- [참고](https://jbground.tistory.com/4)

AJAX는 'Asynchronous JavaScript And XML'의 약자로, 말 그대로 JavaScript와 XML 형식을 이용한 **비동기적** 정보 교환 기법이다.

js 코드로 서버에 요청 / 응답을 할 수 있다.

새로고침 시 전체 페이지를 새로 가져오는 것이 아니라, **전체 페이지 중 일부만 가져오는 것이 가능**하다.

![비동기의예](/assets/images/posts_img/spring/duplicate/asynchronous.png)

이미지 하단의 뉴스 더보기 버튼이 여기에 해당하며, 누르면 뉴스 영역만 서버에 요청한다.

과거에는 다른 방법으로 ajax를 사용했으나, jquery 점유율이 높아지며 주로 jquery를 이용해 실행한다.

그럼 회원가입 시 아이디, 이메일 등의 간단한 중복검사는 어떻게 하는지 코드를 통해 알아보자.

<hr>

## 1. View

![연습문제](/assets/images/posts_img/spring/duplicate/duplicate.png)

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
<head>
    <meta charset="utf-8">
    <title>bookmark</title>
    <!-- bootstrap -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.7.1.js" integrity="sha256-eKhayi8LEQwp4NKxN+CfCh+3qOVUtJn3QNZ0TciWLP4=" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-Fy6S3B9q64WdZWQUiU+q4/2Lc9npb8tCaSX9FK7E8HnRr0Jz8D6OP9dO5Vg3Q9ct" crossorigin="anonymous"></script>
</head>
<body>
  <div id="wrap" class="container">
    <h1>add bookmark</h1>
    <div class="form-group">
      <label>name</label>
      <input type="text" id="name" class="form-control">
    </div>
    <div class="form-group">
      <label>url</label>
      <div class="d-flex">
        <input type="text" id="url" class="form-control" placeholder="http:// or https://">
        <input type="button" id="checkBtn" class="btn btn-primary ml-3" value="중복확인">
      </div>
      <small id="statusArea"></small>
    </div>
    <input type="button" id="addBtn" class="btn btn-success w-100" value="추가">
  </div>
  
<script>
  $(function () {
    $("#checkBtn").on("click", function () {
      $("#statusArea").empty();
      
      let url = $("#url").val().trim();
      
      if (!url) {
        $("#statusArea").append('<span class="text-danger">url을 입력하세요.</span>')
      }
      
      $.ajax({
        url:"/bookmark/duplicate"
        , data:{"url":url}
        ,success:function(data) {
          if (data.is_duplicate) {
            $("#statusArea").append('<span class="text-danger">중복된 url입니다.</span>')
          }
        }
        , error:function(error) {
        }
      });
    });
    
    $("#addBtn").on("click", function () {
      let name = $("#name").val().trim();
      let url = $("#url").val().trim();
      
      if (!name) {
        alert("이름을 입력하세요.")
        return;
      }
      
      if (!url) {
        alert("주소를 입력하세요.")
        return;
      }
      
      if (!(url.startsWith("http://") && url.startsWith("https://"))) {
        alert("양식에 맞춰서 입력하세요.")
        return;
      }
      
      console.log(name);
      console.log(url);
      
      if ($("#statusArea").children().length<1) {
        $.ajax({
          type:"post"
          ,url:"/bookmark/add"
          ,data:{"name":name, "url":url}
          ,success:function(data) {
              if (data == "success") {
                location.href="/bookmark/list"
              }
          }
          ,error:function(error,xhr,status) {
            alert("입력에 실패했습니다.")
          }
        });
      }
    });
  });
</script>
</body>
</html>
```

비어있거나 중복확인 버튼을 눌러 중복일 경우 출력되는 메시지가 없을 경우 저장하도록 `if ($("#statusArea").children().length<1)` 조건문을 추가하였다.

그러나 문제는 중복확인을 한 번도 하지 않을 경우(이 또한 문구가 뜨지 않으므로)에도 저장이 된다는 것이었다.

(처음에는 $.ajax 안에 코드를 넣고 '이게 왜 되는 거지?' 했던.... success 내부에 들어가는 것과 관계없이 ajax로 들어가는 순간 저장. success는 그냥 **성공 시 출력을 담당**한다 라고 생각하면 될듯함)

```js
$(function () {
  let isChecked = false;
  
  $("#checkBtn").on("click", function () {
    $("#statusArea").empty();
    
    let url = $("#url").val().trim();
    
    if (!url) {
      $("#statusArea").append('<span class="text-danger">url을 입력하세요.</span>')
    }
    
    $.ajax({
      url:"/lesson06/bookmark-duplicate"
      , data:{"url":url}
      , success:function(data) {
        if (data.is_duplicate) {
          $("#statusArea").append('<span class="text-danger">중복된 url입니다.</span>')
        } else {
          $("#statusArea").append('<span class="text-success">사용가능한 url입니다.</span>')
          isChecked = true;
        }
      }
      , error:function(error) {
        alert("false")
      }
    });
  });
  
  $("#addBtn").on("click", function () {
    let name = $("#name").val().trim();
    let url = $("#url").val().trim();
    
    if (!name) {
      alert("이름을 입력하세요.")
      return;
    }
    
    if (!url) {
      alert("주소를 입력하세요.")
      return;
    }
    
    if (!url.startsWith("http://") && !url.startsWith("https://")) {
      alert("양식에 맞춰서 입력하세요.")
      return;
    }
    
    console.log(name);
    console.log(url);
    
    if (isChecked) {
      $.ajax({
        type:"post"
        ,url:"/lesson06/bookmark-add"
        ,data:{"name":name, "url":url}
        ,success:function(data) {
          if (data == "success") {
            location.href="/lesson06/bookmark-list"
          } 
          
        }
        ,error:function(error,xhr,status) {
          alert("입력에 실패했습니다.")
        }
      })
    } else {
      alert("중복확인이 필요합니다.")
      return;
    }
  });
});
```

그래서 `isChecked`라는 flag 변수를 사용했다. (현업에서도 이렇게 사용하는지는 모름....)

<hr>

## 2. AJAX에서 자주 사용하는 설정값

>ajax를 사용하기 위해서는 필수로 알아야 할 설정값들이 있다.

| **구분** | **정의** | **값** |
| -------- | ------------- | -------------- |
| **url**         | ajax 요청을 보낼 URL | - |
| **type**        | HTTP 요청 방식 (기본값:get`) | get, post, delete, put |
| **dataType**    | 서버로부터 응답받을 데이터 타입. 생략 시 jQuery가 MIME 타입을 보고 자동 결정 | xml, html, json, script, jsonp, text |
| **contentType** | 전송 시 데이터의 콘텐츠 타입 지정 (기본값: `application/x-www-form-urlencoded; charset=UTF-8`) | application/json 등 |
| **timeout**     | 요청 제한 시간 (ms 단위). 초과 시 실패 처리됨 | 숫자 (ex: 3000) |
| **data**        | 서버로 전송할 데이터. `Object`, `String`, `Array` 등 사용 가능. `Object`일 경우 key-value 구조를 따르며, 값이 배열이면 자동 직렬화됨 | {name: "홍길동", age: 20} 등 |
| **beforeSend**  | 요청이 전송되기 전에 실행되는 콜백 함수. `false`를 반환하거나 `jqXHR.abort()`를 호출하면 요청이 취소됨 | function(xhr) { ... } |
| **success**     | 통신이 성공했을 때 실행되는 콜백 함수 | function(response) { ... } |
| **error**       | 통신이 실패했을 때 실행되는 콜백 함수 (단, `jsonp` 또는 `cross-domain` 요청은 해당 안 될 수 있음) | function(xhr, status, error) { ... } |
| **complete**    | `success` 또는 `error` 후 항상 호출되는 콜백 함수 | function(xhr, status) { ... } |

<hr>

## 3. Controller

```java
@RequestMapping("/bookmark")
@Controller
public class Controller {

  @Autowired
  private BookmarkBO bookmarkBO;
  
  @GetMapping("/view")
  public String view() {
    return "addBookmark";
  }
  
  @PostMapping("/add")
  @ResponseBody
  public String add(
      @RequestParam("name") String name,
      @RequestParam("url") String url) {
    
    bookmarkBO.setBookmark(name,url);
    
    return "success";
  }
  
  @GetMapping("/list")
  public String list(Model model) {
    
    List<Bookmark> bookmark = bookmarkBO.getBookmark();
    
    model.addAttribute("bookmark", bookmark);
    
    return "bookmarkList";
  }
  
  @ResponseBody
  @GetMapping("/duplicate")
  public Map<String, Object> isDuplicate(
      @RequestParam("url") String url) {
    
    boolean isDuplicate = bookmarkBO.isDuplicate(url);
        
    Map<String, Object> result = new HashMap<>();
    result.put("is_duplicate", isDuplicate);
    
    return result;
  }
}
```
