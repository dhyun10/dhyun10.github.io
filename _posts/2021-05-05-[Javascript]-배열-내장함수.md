---
title: "[Javascript] 배열 내장함수"
date: 2021-05-05 10:00:00 +0900
categories: [Programming, JavaScript]
tags: [JavaScript, JQuery]
---

## 배열 내장함수

---



### Array.prototype.forEach()

- 주어진 함수를 배열 요소 각각에 대해 실행합니다.

  - 예시

  ```javascript
  const a=[1,2,3,4,5];
  
  for(let i=0; i<a.length; i++) {
  	console.log(a[i]);
  } // 출력 : 1, 2, 3, 4, 5
  ```



### Array.prototype.map()

- 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.

  - 예시

  ```javascript
  const a=[1,2,3,4,5];
  
  const b=a.map(function(s) {
  	return s*s;
  })
  
  console.log(b); // 출력 : [1, 4, 9, 16, 25]
  ```



### Array.prototype.filter()

- 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환합니다.

  - 예시

  ```javascript
  const a=[1,2,3,4,5];
  
  const b=a.filter(function(s) {
  	return s%2==0;
  })
  
  console.log(b); // 출력 : [2, 4]
  ```



### Array.prototype.indexOf()

- 배열에서 지정된 요소를 찾을 수 있는 첫 번째 인덱스를 반환하고 존재하지 않으면 -1을 반환합니다.

  - 예시

  ```javascript
  const a = ['호랑이', '사자', '고양이', '멍멍이'];
  console.log(a.indexOf('고양이')); // 출력 : 2
  ```

  

### Array.prototype.findIndex()

- 주어진 판별 함수를 만족하는 배열의 첫 번째 요소에 대한 인덱스를 반환합니다. 만약 만족하는 요소가 없는 경우에는 -1을 반환합니다.

  - 예시

  ```javascript
  const a = [
     { name : '호랑이' },
     { name : '사자' },
     { name : '고양이' },
     { name : '멍멍이' }
  ]
  console.log(a.findIndex(ary => ary.name === '고양이')); // 출력 : 2
  ```

  

### Array.prototype.find()

- 주어진 판별 함수를 만족하는 첫 번째 요소의 값을 반환합니다. 그런 요소가 없다면 undefined를 반환합니다.

  - 예시

  ```javascript
  const a = [
     { name : '호랑이' },
     { name : '사자' },
     { name : '고양이' },
     { name : '멍멍이' }
  ]
  console.log(a.find(ary => ary.name === '고양이')); // 출력 : {name: '고양이'}
  ```

  

### Array.prototype.splice()

- 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경합니다.

  - 예시

  ```javascript
  const a = [1,2,3,4,5];
    
  const b = a.splice(0,2,10,11);
   
  console.log(b); // 출력 : [1,2]
  console.log(a); // 출력 : [10,11,3,4,5]
  ```

  

### Array.prototype.slice()

- splice 메서드와는 다르게 해당 구간 인덱스만을 가지는 새로운 배열을 반환하며 원본 배열은 바뀌지 않는다.

  - 예시

  ```javascript
  const a = [1,2,3,4,5];
   
  const b = a.slice(1,3);
    
  console.log(b); // 출력 : [2,3]
  console.log(a); // 출력 : [1,2,3,4,5]
  ```

  

### Array.prototype.reduce()

- 배열의 각 요소에 대해 주어진 reducer 함수를 실행하고, 하나의 결과값을 반환합니다.

  - 예시

  ```javascript
  const a = [1,2,3,4,5];
    
  const b = a.reduce(function(a,b) {
       return a + b;
  },0)
    
  console.log(b); // 출력 : 15
  ```

  

### Array.prototype.shift() / Array.prototype.pop()

- shift() : 배열에서 첫 번째 요소를 제거하고, 제거된 요소를 반환합니다.

- pop() : 배열에서 마지막 요소를 제거하고 그 요소를 반환합니다.

  - 예시

  ```javascript
  const a=[1,2,3,4,5];
  
  a.shift();
  console.log(a); // 출력 : [2,3,4,5]
  a.pop();
  console.log(a); // 출력 : [2,3,4]
  ```



### Array.prototype.unshift() / Array.prototype.push()

- unshift() : 새로운 요소를 배열의 맨 앞쪽에 추가하고, 새로운 길이를 반환합니다.

- push() : 배열의 끝에 하나 이상의 요소를 추가하고, 배열의 새로운 길이를 반환합니다.

  - 예시

  ```javascript
  const a=[1,2,3,4,5];
  
  a.unshift(10);
  console.log(a); // 출력 : [10,1,2,3,4,5]
  a.push(11);
  console.log(a); // 출력 : [10,1,2,3,4,5,11]
  ```

  

### Array.prototype.concat()

- 인자로 주어진 배열이나 값들을 기존 배열에 합쳐 새 배열을 반환합니다.

  - 예시

  ```javascript
  const arr1=[1,2,3];
  const arr2=[4,5,6];
  
  const concated=arr1.concat(arr2);
  console.log(concated); // 출력 : [1,2,3,4,5,6]
  ```



### Array.prototype.join()

- 배열의 모든 요소를 연결해 하나의 문자열로 만듭니다.

  - 예시

  ```javascript
  const a=[1,2,3,4,5];
  console.log(a.join('*')); // 출력 : 1*2*3*4*5
  ```

  