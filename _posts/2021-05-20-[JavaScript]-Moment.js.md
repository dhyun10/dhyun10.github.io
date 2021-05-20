---
title: "[JavaScript] Moment.js"
date: 2021-05-20 10:00:00 +0900
categories: [Programming, JavaScript]
tags: [JavaScript]
---

## Moment.js

---

- 날짜를 손쉽게 다루는 javascript 라이브러리



#### moment()

- 현재 날짜 값



#### format()

````javascript
moment().format('YYYY-MM-DD') // 현재 날짜를 YYYY-MM-DD 형식으로 format
````



#### add()

```javascript
moment().add(1, 'days') // 현재 날짜로부터 1일 후
moment().add(1, 'months') // 현재 날짜로부터 1개월 후
moment().add(1, 'years') // 현재 날짜로부터 1년 후 
```



#### subtract()

```javascript
moment().subrtract(1, 'days') // 현재 날짜로부터 1일 전
moment().subrtract(1, 'months') // 현재 날짜로부터 1개월 전
moment().subrtract(1, 'years') // 현재 날짜로부터 1년 전
```



#### diff()

```javascript
moment('2021-05-20').diff('2021-05-21', 'days') // 현재 날짜로부터 날짜 간격 (일단위)
moment('2021-05-20').diff('2021-06-21', 'months') // 현재 날짜로부터 날짜 간격 (월 단위)
moment('2021-05-20').diff('2022-05-21', 'years') // 현재 날짜로부터 날짜 간격 (년단위)
```



#### isBefore(), isAfter()

```javascript
moment().isBefore('2021-05-19') // 현재 날짜로부터 이전 날짜인지 비교
moment().isAfter('2021-05-21') // 현재 날짜로부터 이후 날짜인지 비교
```



#### isBetween()

```javascript
moment('2021-05-20').isBetween('2021-05-19', '2021-05-21'); // 두 날짜 사이의 값이 맞는지 비교
```



- 참고 : https://momentjs.com/