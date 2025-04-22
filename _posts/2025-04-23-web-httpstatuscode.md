---
title: "[Web] HTTP response status codes"
excerpt: "HTTP 상태 코드 암기 노트"

categories:
  - Web
tags:
  - [httpstatuscode]

permalink: /web/httpstatuscode/

toc: true
toc_sticky: true

date: 2025-04-23
last_modified_at: 2025-04-23
---

HTTP(Hypertext Transfer Protocol)는 웹 서버와 웹 클라이언트 사이에서 데이터를 주고받기 위해 사용하는 통신 방식으로 TCP/IP 프로토콜 위에서 동작한다.

즉, 웹을 이용하려면 웹 서버와 웹 클라이언트는 각각 TCP/IP 동작에 필수적인 IP 주소를 가져야 한다는 의미다.

HTTP란 이름대로라면 하이퍼텍스트(Hypertext)만 전송할 수 있어 보이지만, 실제로는 HTML이나 XML과 같은 하이퍼텍스트뿐 아니라 이미지, 음성, 동영상, Javascript, PDF와 각종 문서 파일 등 컴퓨터에서 다룰 수 있는 데이터라면 무엇이든 전송할 수 있다.

웹 브라우저의 주소창에 https://www.naver.com을 입력하고 Enter 를 누르면 웹 클라이언트와 웹 서버 사이에 HTTP 연결이 맺어지고 웹 클라이언트는 웹 서버에 HTTP 요청 메시지를 보내고, 웹 서버는 요청에 따른 처리를 진행한 후에 그 결과를 웹 클라이언트에 HTTP 응답 메시지로 보낸다.

이처럼 요청 메시지와 응답 메시지가 반복적으로 오가므로 우리는 웹을 사용할 수 있는 것이다.

서버에서의 처리 결과는 응답 메시지의 상태 라인에 있는 상태 코드(status code)를 보고 파악할 수 있다.

상태 코드는 세 자리 숫자로 되어 있으며 첫 번째 숫자는 HTTP 응답의 종류를 구분하는 데 사용하며 나머지 2개의 숫자는 세부적인 응답 내용 구분을 위한 번호이다.

1. **<font color="#000099">Informational responses (100 – 199)</font>** 정보를 제공하는 응답
   1. 1xx: 임시 응답으로 현재 클라이언트의 요청까지는 처리되었으니 계속 진행
2. **<font color="#000099">Successful responses (200 – 299)</font>** 성공적인 응답
   1. 2xx: 클라이언트의 요청이 서버에서 성공적으로 처리됨
3. **<font color="#000099">Redirection messages (300 – 399)</font>** 리다이렉트
   1. 3xx: 완전한 처리를 위해 추가 동작이 필요한 경우. 주로 요청한 주소(URL)가 이동됨을 뜻함
4. **<font color="#000099">Client error responses (400 – 499)</font>** 클라이언트 에러
   1. 4xx: 클라이언트에서 없는 페이지를 요청한 경우
5. **<font color="#000099">Server error responses (500 – 599)</font>** 서버 에러
   1. 5xx: 서버 부하, DB 처리 오류, 익셉션이 발생하는 경우

<hr>

## 1. Informational responses (100 – 199)

### 100
**Continue**
임시적인 응답. 지금까지의 상태가 괜찮으며 클라이언트가 계속해서 요청을 하거나 이미 `요청을 완료한 경우에는 무시`해도 됨을 말한다.

### 101
**Switching Protocol**
프로토콜을 `Upgrade`할 때 응답 헤더에 표시. (현재는 HTTP 1.1이 최신이므로 사용할 일이 없음)

### 102
**Processing**
서버가 요청을 수신하였으며 이를 `처리하고 있지만(처리중)`, 아직 제대로 된 응답을 알려줄 수 없음을 뜻한다.

### 103
**Early Hints**
주로 Link 헤더와 함께 사용되어 서버가 응답을 준비하는 동안 사용자 에이전트가(user agent) 사전 로딩(preloading)을 시작할 수 있도록 한다.

<hr>

## 2. Successful responses (200 – 299)

### <font color="#990000">200</font>
**OK**
요청이 `성공`적으로 되었음을 말한다. (성공의 의미는 HTTP 메소드에 따라 달라짐)
- GET: 리소스를 불러와서 메시지 바디에 전송
- HEAD: 개체 해더가 메시지 바디에 있음
- PUT 또는 POST: 수행 결과에 대한 리소스가 메시지 바디에 전송됨
- TRACE: 메시지 바디는 서버에서 수신한 요청 메시지를 포함하고 있음

### <font color="#990000">201</font>
**Created**
요청이 성공적이었으며 그 결과로 `새로운 리소스가 생성`됨. (이 응답은 일반적으로 POST 요청 또는 일부 PUT 요청 이후에 따라옴)

### <font color="#990000">202</font>
**Accepted**
요청을 수신하였지만 처리가 완료되지 않음. `나중에 처리`한다는 걸 뜻한다. (주로 비동기 작업 요청 시 사용)

### 204
**No Content**
요청에 대해서 보내줄 수 있는 콘텐츠가 없다. `삭제` 요청에 주로 사용.

### 206
**Partial Content**
콘텐츠 `일부만 보낼 때` 사용한다. (1500 바이트 리소스 중, 처음 500 바이트만 보낼 때 사용할 수 있다)

<hr>

## 3. Redirection messages (300 – 399)

### <font color="#990000">301</font>
**Moved Permanently**
요청한 `리소스의 URI(Uniform Resource Identifier)가 새로운 URI로 변경되었음`을 의미한다.

### 302
**Found**
요청한 리소스의 URI가 일시적으로 변경되었음을 의미한다. (302를 정확하게 개선해 307을 정의하였으므로 이 응답 코드의 사용은 권장하지 않음)

### <font color="#990000">303</font>
**See Other**
클라이언트가 `요청한 리소스를 다른 URI에서 GET 요청을 통해 얻어야 할 때`, 서버가 클라이언트로 직접 보내는 응답이다. (다른 위치로 요청할 것)

### 304
**Not Modified**
마지막 요청 이후 클라이언트에게 응답이 수정되지 않았음을 알려준다.

### <font color="#990000">307</font>
**Temporary Redirect**
임시로 리다이렉션 요청이 필요. `요청한 주소가 임시로 바뀌었을 때`를 말한다. 만약 첫 요청에 POST가 사용되었다면, 두번째 요청도 반드시 POST를 사용해야 한다.

### 308
**Permanent Redirect**
`요청한 주소가 임시가 아니라 완전히 바뀌었을 때`를 말한다. 만약 첫 요청에 POST가 사용되었다면, 두번째 요청도 반드시 POST를 사용해야 한다.

<hr>

## 4. Client error responses (400 – 499)

### <font color="#990000">400</font>
**Bad Request**
`잘못된 문법`으로 인하여 서버가 요청을 이해할 수 없음을 의미한다.

### <font color="#990000">401</font>
**Unauthorized**
리소스에 대한 `액세스 권한이 없다.`

### 402
**Payment Required**
디지털 결제 시스템에 사용하기 위하여 만들어졌지만 지금 사용되고 있지는 않다.

### <font color="#990000">403</font>
**Forbidden**
`클라이언트가 콘텐츠에 접근할 권리를 가지고 있지 않음`을 의미한다. 401과 다른 점은 서버가 클라이언트가 누구인지 알고 있다는 것이다.

### <font color="#990000">404</font>
**Not Found**
`서버가 요청받은 리소스를 찾을 수 없음.` 브라우저에서는 알려지지 않은 URL을 의미한다. API 종점으로 적절하지만 리소스 자체는 존재하지 않음을 의미할 수도 있으며, 서버들은 인증받지 않은 `클라이언트로부터 리소스를 숨기기 위하여 이 응답을 403 대신에 전송할 수도 있다.`

### 405
**Method Not Allowed**
요청한 메소드는 `서버에서 알고 있지만 사용할 수 없는 경우`를 말한다. 예를 들어, API에 삭제 요청을 보냈지만 리소스를 삭제하는 것이 금지된 경우가 있다.

### 408
**Request Timeout**
`요청을 한지 시간이 오래된 연결에 서버가 전송`한다. 어떨 때에는 이전에 클라이언트로부터 어떠한 요청이 없었다고 하더라도 보내지기도 합니다. 이것은 서버가 사용되지 않는 연결을 끊고 싶어한다는 것을 의미한다. 특정 몇몇 브라우저에서 빈번하게 보이는데, Chrome, Firefox 27+, 또는 IE9와 같은 웹서핑 속도를 올리기 위해 HTTP 사전 연결 메카니즘을 사용하는 브라우저들이 해당된다.

### 409
**Conflict**
요청이 `현재 서버의 상태와 충돌될 때` 보낸다. (이미 존재하는 아이디로 회원가입을 시도할 때 등)

### 429
**Too Many Requests**
사용자가 `짧은 시간 동안 너무 많은 요청을 보낼 때.` 서버가 봇이나 해킹을 의심해서 보내는 응답이다.

<hr>

## 5. Server error responses (500 – 599)

### <font color="#990000">500</font>
**Internal Server Error**
`서버에 버그가 발생했거나 예외처리를 하지 못해서 발생`한다.

###  <font color="#990000">501</font>
**Not Implemented**
요청한 URI의 메서드에 대해 `서버가 구현하고 있지 않을 때` 발생한다.

### <font color="#990000">502</font>
**Bad Gateway**
`게이트웨이가 엉뚱한 응답을 받았을 때` 발생. 서버가 다른 서버에 요청을 보내는데, 그쪽에서 잘못된 응답을 보낸 경우에 해당한다. (프록시 서버나 API 게이트웨이가 외부 서버와 통신 중 에러 발생했을 경우)

### 503
**Service Unavailable**
일반적인 원인은 `유지보수를 위해 작동이 중단되거나, 과부하가 걸렸을 때` 발생한다.

### 504
**Gateway Timeout**
서버의 `응답 시간 초과` 시 발생한다.

<hr>

**참고**

[참고 1](https://developer.mozilla.org/ko/docs/Web/HTTP/Reference/Status)
[참고 2](https://hongong.hanbit.co.kr/http-%EC%83%81%ED%83%9C-%EC%BD%94%EB%93%9C-%ED%91%9C-1xx-5xx-%EC%A0%84%EC%B2%B4-%EC%9A%94%EC%95%BD-%EC%A0%95%EB%A6%AC/)
