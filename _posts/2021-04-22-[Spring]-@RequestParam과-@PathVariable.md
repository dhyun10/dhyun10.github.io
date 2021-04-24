---
title: "[Spring] @RequestParam과 @PathVariable"
date: 2021-04-22 10:00:00 +0900
categories: [Programming, Spring]
tags: [Spring, SpringBoot]
---

## @RequestParam과 @PathVariable

---

### Controller에서 데이터를 전달받는 방법 

controller단에서 데이터를 받아오는 타입에는 여러가지가 있다.

ServletRequest, ServletResponse, HttpSession을 이용한 **Sevlet API**,

WebRequest, MultipartRequest를 이용하는 **Spring API**,

그리고 **Spring Annotation**이 있다.

오늘은 그중에서도 Spring Annotation의 @RequestParam과 @PathVariable에 대해서 알아보자!



### @RequestParam

- 요청 파라미터를 넣어주는 어노테이션이며 `http://localhost:8080?userId=test` 형식의 url 주소로 파라미터를 전달받을 수 있다.

```java
@GetMapping(value = "/")
public String test(
	@RequestParam(value = "userId", required = false) String userId
	) {
}
```

value 값에는 넘어오는 파라미터의 name값을 작성해주고

선언한 파라미터가 없는 경우 required = false를 작성하지 않았다면 400 에러가 발생한다. 



### @PathVariable

- 파라미터를 URL 경로에 포함시키는 방법. 예를 들어 id로 해당하는 정보를 요청할 때의 url은 `http://localhost:8080/test` 이고 {} 중괄호에 명시된 값을 변수로 받을 수 있다.
- 주로 RESTful 방식에서 사용된다.

```java
@GetMapping(value = "/{userId}")
public String test(
	@PathVarialble("userId") String userId
	) {
}
```



- 물론 @RequestParam 또는 @PathVariable 두가지를 복합적으로 사용하는 것도 가능하다