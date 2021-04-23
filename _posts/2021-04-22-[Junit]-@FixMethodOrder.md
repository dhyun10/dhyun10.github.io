---
title: Junit/@FixMethodOrder
date: 2021-04-22 10:00:00 +0900
categories: [Programming, JAVA]
tags: [JAVA, Junit]  
---

## **@FixMethodOrder**(Option) 

***

### MethodSorters Option

- MethodSorters.DEFAULT 
  - hashcode를 기반으로 순서를 결정한다. 
  - 실행 순서를 예측하기 힘들다.

- MethodSorters.JVM
  - 시스템의 리소스 상황에 따라 다른 순서로 결과를 보낸다.
- MethodSorters.NAME_ASCENDING
  - 메소드 명을 오름차순으로 정렬한 순서대로 실행된다.

```java
@FixMethodOrder(MethodSorters.NAME_ASCENDING)
public class JunitOrderTest {
    @Test
    public void test1() {
        System.out.println("첫번째로 실행!");
    }
    @Test
    public void test2() {
        System.out.println("두번째로 실행!");
    }
}
```

### Test 실행 결과화면

![OrderTest](https://user-images.githubusercontent.com/70506979/115645003-89402a80-a35a-11eb-92c5-80c81a38d900.PNG)

