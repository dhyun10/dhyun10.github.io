---
title: "[Javascript] jQuery.extend()"
date: 2021-04-23 10:00:00 +0900
categories: [Programming, JavaScript]
tags: [JavaScript]

---

## jQuery.extend()

---

- 두개 이상의 객체를 합치는(Merge) 함수
- 기본적인 형태

```javascript
var object=$.extend([deep], target, object1, object2);
```

```javascript
var objA = {
    id: 1
};

var objB = $.extend({}, objA, {
    name: "dahyun"
});
console.log(objB);
```

#### 결과화면

![zzx](https://user-images.githubusercontent.com/70506979/115896926-e3dea100-a496-11eb-9664-6242fc04482a.PNG)

- deep이 true일 경우 깊은 수준 복사가 가능하다.

```javascript
var objA = {
  list: [{
    id: 1
  }]
};

var objB = $.extend({}, objA, {
  list: [{
    name: "dahyun"
  }]
});
console.log(objB);

var objB = $.extend(true, {}, objA, {
  list: [{
    name: "dahyun"
  }]
});
console.log(objB);
```

#### 결과화면

![image-20210424003830390](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20210424003830390.png)

