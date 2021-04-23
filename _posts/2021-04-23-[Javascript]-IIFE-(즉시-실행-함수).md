---
title: [JavaScript] IIFE (즉시 실행 함수)
date: 2021-04-23 10:00:00 +0900
categories: [Programming, JavaScript]
tags: [JavaScript]

---

## IIFE (즉시 실행 함수)



일단 IIFE를 공부하기전 함수의 선언과 함수의 표현에 대해서 알아보자 !

### 함수 선언식 / 함수 표현식

- 함수 선언식
  - 함수를 정의하고, 함수를 할당할 변수를 만들지 않는다.
- 함수 표현식
  - 함수가 변수로 할당될 수 있다. 또한 함수가 익명일 수도 있고, 다른 함수 표현식의 일부가 될 수도 있다.

```javascript
// 함수선언식
function hello() {
	// ...
};

// 함수표현식
var hello=function() {
	// ...
};
```



### IIFE(Immediately Invoked Function Expressions)

- <u>정의되자마자 즉시 호출되는 함수</u>

- 기본적인 형태

```javascript
(function() {
	// 생략 ...
})();

(function(name) {
    // 생략 ...
})('dahyun');
```

- 특징
  - 불필요한 전역 변수와 함수를 생성하지 않는다.
  - IIFE안에 존재하는 코드는 외부에서 접근할 수 없다.
  - IIFE에서 생성된 변수와 함수의 이름은 전역 Scope를 오염시키지 않는다.(충돌하지 않는다.)
- 다양한 표현식

```javascript
!function(a, b) { return console.log(a + b) }(1, 2); // 3
void function(a, b) { return console.log(a + b) }(1, 2); // 3
+function(a, b) { return console.log(a + b) }(1, 2); // 3
-function(a, b) { return console.log(a + b) }(1, 2); // 3
~function(a, b) { return console.log(a + b) }(1, 2); // 3
*function(a, b) { return console.log(a + b) }(1, 2); // 3
^function(a, b) { return console.log(a + b) }(1, 2); // 3
&function(a, b) { return console.log(a + b) }(1, 2); // 3
```
