---
title: "[Spring] Caused by: org.thymeleaf.exceptions.TemplateProcessingException: Exception evaluating SpringEL expression"
excerpt: "SpringEL Exception의 다양한 원인"

categories:
  - Spring
tags:
  - [springEL, error]

permalink: /spring/null_data/

toc: true
toc_sticky: true

date: 2025-06-11
last_modified_at: 2025-06-11
---

```html
<div th:each="card : ${cardList}" class="card border rounded mt-3">
  <div class="p-2 d-flex justify-content-between">
    <span class="font-weight-bold" th:text="${card.user.name}"></span>

    <a href="#" class="more-btn">
      <img src="https://www.iconninja.com/files/860/824/939/more-icon.png" width="30">
    </a>
  </div>
```

MVC 패턴으로 view에 타임리프 문법으로 데이터를 뿌리는데 자꾸 아래 런타임 에러가 발생.

**Caused by: org.thymeleaf.exceptions.TemplateProcessingException: Exception evaluating SpringEL expression**

'오타가 있나?' 생각해서 꼼꼼하게 전부 확인해봤지만 오타는 찾을 수 없었다.

그리고 에러 문구 하단에서 **Caused by: org.springframework.expression.spel.SpelEvaluationException: EL1007E: Property or field 'name' cannot be found on null** 문구를 확인하고 디버깅으로 확인했지만 값은 제대로 들어오고 있는 상태.

'제대로 넘어오고 있는데 대체 왜 null이라고 하는 걸까?' 약 2시간을 view부터 역추적해서 코드를 확인했다.

그러나 코드에서 별다른 에러는 확인할 수 없었는데...

원인은 DB에 있었다.

과거 테스트 중에 null로 들어간 데이터가 있었던 것.

null로 들어와 not null로 수정 후, 테스트 데이터를 삭제하지 않아 계속해서 런타임 에러가 발생하고 있었다.
