---
title: "[Javascript] 얕은 복사와 깊은 복사"
date: 2021-04-23 10:00:00 +0900
categories: [Programming, JavaScript]
tags: [JavaScript]
---

## 얕은 복사와 깊은 복사

---

### 얕은 복사(Shallow Copy)

- 객체를 복사할 때, 해당 객체만 복사하여 새 객체를 생성한다.
- 복사된 객체의 인스턴스 변수는 원본 객체의 인스턴스 변수와 같은 메모리 주소를 참조하며 해당 메모리 주소의 값이 변경되면 다른 객체의 변수 값 역시 동일하게 변경된다.

```javascript
var objA = [{ id: 1, name: "david" }];
var objB = objA;
objB.name = "jeff";
console.log("objB--", objB);
console.log("objA--", objA);
```

#### Test 실행 결과화면

![0423_1](https://user-images.githubusercontent.com/70506979/115887615-14214200-a48d-11eb-8f10-dedc95db65ce.PNG)



### 깊은 복사(Deep Copy)

- 객체를 복사할 때 해당 객체와 인스턴스 변수까지 복사한다.
- 데이터 참조가 아닌 객체의 형태를 그대로 복사함으로써 한 객체가 변경되어도 다른 객체의 데이터에는 영향을 주지 않는다.

```javascript
var objA = { id: 1, a: {name : "david"} };
var objB = {...objA};

objA.id=2;
objA['a'].name = "jeff";

console.log("objB--", objB);
console.log("objA--", objA);
```

#### Test 실행 결과화면

![캡처](https://user-images.githubusercontent.com/70506979/115890458-e984b880-a48f-11eb-982c-d499db44b08c.PNG)

objA의 id 값을 재할당하였으나 objB의 id값은 변경되지 않아 깊은 복사가 되었다고 생각할 수 있다. 그러나 결과화면에서 볼 수 있듯이 objA 내부의 {name: "david"} 객체의 값을 변경하였을때 objB 내부의 name 까지 변경되었다.

*즉*, 완벽한 복사는 아니라고 할 수 있다. (Object.assign() 역시 비슷한 결과.)



### **<u>완벽한 깊은 복사</u>**

- JSON.parse()/JSON.stringity()
  - JSON.stringity 함수를 이용하여 Object 전체를 문자열로 변환한 뒤 다시 JSON.parse 함수를 이용하여 문자열을 Object 형태로 변환한다. 문자열로 변환하는 과정에서 객체에 대한 참조가 사라지며 새로운 객체로 깊은 복사가 가능하다.

```javascript
var objA = [{ id: 1, name: "david" }];
var objB = JSON.parse(JSON.stringify(objA[0]));
objB.name = "jeff";
console.log("objB--", objB);
console.log("objA--", objA);
```

#### Test 실행 결과화면

![0423_2](https://user-images.githubusercontent.com/70506979/115889436-e806c080-a48e-11eb-8ea3-cc4d51a1b8ea.PNG)

