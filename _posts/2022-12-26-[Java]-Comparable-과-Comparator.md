---
title: "[Java] Comparable 과 Comparator"
date: 2022-12-26 10:00:00 +0900
categories: [Programming, Java]
tags: [Java]

---
# Comparable 과 Comparator

<aside>
💡 Java에서 객체를 정렬할 수 있는 인터페이스(interface)에 대해 학습해보자

</aside>

## Comparable

> `java.lang`  패키지에 속해있으며, Comparable 인터페이스를 사용하기 위해서는 반드시 선언되어 있는 `compareTo(T o)` 메소드를 재정의(Override) 해야한다.
>
— Comparable 인터페이스는 compareTo() 메소드를 이용해 자기 자신과 매개 변수인 객체를 비교할 수 있다.

- compareTo(T o) 구현

```java
public class Test {

	public static void main(String[] args) {
		Person a = new Person("AAA", 27);
		Person b = new Person("BBB", 31);

		int isCompare = a.compareTo(b);

		if(isCompare > 0) {
			System.out.println("AAA가 BBB보다 나이가 많다.");
		} else if(isCompare == 0){
			System.out.println("AAA와 BBB는 나이가 같다.");
		} else {
			System.out.println("AAA가 BBB보다 나이가 적다.");
		}
	}

}

class Person implements Comparable<Person>{
  String name;
  int age;

  public Person(String name, int age) {
    this.name = name;
    this.age = age;
  }

  @Override
  public int compareTo(Person o) {
    return this.age - o.age;
  }
}
```

## Comparator

> `java.util` 패키지에 속해 있으며 해당 인터페이스는 `@FunctionalInterface` , 즉 함수형 인터페이스이다.  Comparator 인터페이스를 사용하기 위해서는 반드시 선언되어 있는 `compare(T o1, T o2)` 메소드를 재정의(Override) 해야한다.
>
— Comparator 인터페이스는 compareTo() 메소드를 이용해 두 매개변수 객체를 비교할 수 있다.
- 구현

```java
import java.util.Comparator;

public class Test {

	public static void main(String[] args) {
		Person a = new Person("AAA", 27);
		Person b = new Person("BBB", 31);

		int isCompare = a.compare(a, b);
		int isCompare2 = b.compare(a, b);

		if(isCompare > 0) {
			System.out.println("AAA가 BBB보다 나이가 많다.");
		} else if(isCompare == 0){
			System.out.println("AAA와 BBB는 나이가 같다.");
		} else {
			System.out.println("AAA가 BBB보다 나이가 적다.");
		}

		if(isCompare2 > 0) {
			System.out.println("AAA가 BBB보다 나이가 많다.");
		} else if(isCompare2 == 0){
			System.out.println("AAA와 BBB는 나이가 같다.");
		} else {
			System.out.println("AAA가 BBB보다 나이가 적다.");
		}
	}

}

class Person implements Comparator<Person>{
	String name;
	int age;

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public int compare(Person o1, Person o2) {
		return o1.age - o2.age;
	}
}
```

### comparator 활용

> comparator 인터페이스의 compare() 메소드는 두 개의 매개 변수를 받아 호출된다. 그렇기 때문에 익명를 생성하여 구현한다면 더욱 효율적으로 사용이 가능하다.
>
- 구현

```java
import java.util.Comparator;

public class Test {

	public static void main(String[] args) {
		Person a = new Person("AAA", 27);
		Person b = new Person("BBB", 31);

		Comparator<Person> c = new Comparator<Person>() {

			@Override
			public int compare(Person o1, Person o2) {
				return o1.age - o2.age;
			}

		};

		int isCompare = c.compare(a, b);

		if(isCompare > 0) {
			System.out.println("AAA가 BBB보다 나이가 많다.");
		} else if(isCompare == 0){
			System.out.println("AAA와 BBB는 나이가 같다.");
		} else {
			System.out.println("AAA가 BBB보다 나이가 적다.");
		}
	}

}

class Person {
	String name;
	int age;

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
}
```
