---
title: "[Effective Java] Item 6"
date: 2023-01-27 09:00:00 +0900
categories: [Study, Effective Java]
tags: [Java, Effective Java]
---

# Effective Java - Item 6

<aside>
💡 불필요한 객체 생성을 피하라

</aside>

똑같은 기능의 객체를 여러 번 생성하는 것보다 재사용하는 것이 성능 향상에 도움이 될 수 있다. 특히 불변 객체는 언제든 재사용할 수 있다.

```java
String s1 = new String(”homework!”); // 하지 말아야 할 코드

String s2 = "homework!";
```

해당 코드의 s1, s2는 결과적으로 같은 기능을 한다.

s1의 경우 “homework!” 자체가 해당 생성자로 만들어 내려는 String 인스턴스와 완전히 같지만, `new String()` 으로 감싸 실행될 때마다 String 인스턴스가 새로 생성된다.

해당 코드가 빈번히 호출되는 메서드 안에 존재한다면 쓸데없는 String 인스턴스가 수백만 개씩 만들어질 수도 있다.

반면 s2와 같은 경우에는 문자열 리터럴을 재사용하므로 같은 JVM에 동일한 문자열 리터럴이 존자한다면 재사용이 가능하다.

정적 팩터리 메서드를 사용하는 경우에도 불필요한 객체 생성을 피할 수 있다.

→ 항상 새로운 객체를 생성하는 `Boolean(String)` 생성자 대신  기존에 캐싱한 객체를 반환해주는`Boolean.valueOf(String)` 팩터리 메소드를 사용

또한 데이터 베이스 커넥션과 같이 생성 비용이 비싼 객체가 반복해서 필요한 경우에는 캐싱을 통해 재사용 해야 한다. 다만 자신이 생성해야 하는 객체가 비싼 객체인지를 매번 명확하게 알 

수는 없다.

예를 들어, 아래와 같이 `String.matches()` 메소드는 정규표현식으로 문자열 형태를 확인하는 가장 쉬운 방법이지만, 성능이 중요한 상황에서 반복해 사용하기에는 적합하지 않다.  

```java
static boolean isRomanNumeral(String s) {
	return s.matches("^(?=.)M*(C[MD]|D?C{0,3})"
				+ "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
}
```

→ `matches()` 메소드 내부에서 정의하는 Pattern 인스턴스는 매번 새로 생성되어 한 번 쓰고 버려져 곧바로 가비지 컬렉션의 대상이 된다.

String 클래스의 `matches()` 메소드가 반환하는 Pattern은 불변 클래스(final class) 이고, 생성 비용이 비싸기 때문에 재사용하기를 권장한다. Pattern 은 입력받은 정규 표현식에 해당하는 유한 상태 머신(finite state machine)을 만들기 때문에 인스턴스 생성 비용이 높다.

```java
public class RomanNumerals {
	private static final Pattern ROMAN = Pattern.compile(
				"^(?=.)M*(C[MD]|D?C{0,3})"
				+ "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");

	static boolean isRomanNumeral(String s) {
		return ROMAN.matcher(s).matches();
	}
}
```

→ Pattern 인스턴스를 `static final` 필드로 꺼내고 이름을 지정하여 어떤 코드인지 명확하게 알 수 있다.

성능을 개선하기 위해서는 필요한 정규표현식을 표현하는 Pattern 인스턴스를 클래스 초기화 과정에서 직접 생성하여 캐싱하고, 나중에 메서드가 호출될 때마다 해당 인스턴스를 재사용하도록 해야한다.

다만, 개선된 isRomanNumeral 방식의 클래스가 초기화된 후 메서드를 한 번도 호출하지 않는다면 ROMAN 필드는 쓸데없이 초기화된 꼴이다. 메서드가 처음 호출될 때 필드를 초기화하는 **지연 초기화(lazy initialzation)**로 불필요한 초기화를 막을 수는 있지만, 코드가 복잡해지는 것에 비해 성능은 크게 개선되지 않을 때가 많다.

또 다른 불필요한 객체를 만들어내는 예시로 오토박싱(auto boxing)을 들 수 있다.

❓오토박싱(auto boxing)

> 자바 컴파일러가 primitive data type을 그에 상응하는 wrapper class로 자동 변환 시켜주는 것, 예를 들면 int를 Integer로, double을 Double로 변환한다.
> 

```java
private static long sum() {
	Long sum = 0L;
	for (long i = 0; i <= Integer.MAX_VALUE; i++) 
		sum += i;

	return sum;
}
```

→ 모든 양의 정수의 총합을 구하는 메소드

위 코드에서 sum을 Long으로 선언함에 따라 불필요한 Long 인스턴스가 계속해서 생성된다. 

단순히 sum의 타입을 long으로만 바꿔줘도 오토박싱이 일어나지 않고, 불필요한 인스턴스 생성이 줄어 성능이 좋아진다.

의미상으로는 별다를 것이 없지만 성능에 있어서는 다르기 때문에 박싱된 기본 타입보다는 기본 타입을 사용하고 의도치 않은 오토박싱이 숨어들지 않도록 주의해야한다.