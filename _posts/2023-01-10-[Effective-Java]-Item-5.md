---
title: "[Effective Java] Item 5"
date: 2023-01-10 10:00:00 +0900
categories: [Study, Effective Java]
tags: [Java, Effective Java]
---

# Effective Java - Item 5

<aside>
💡 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

</aside>

많은 클래스가 하나 이상의 자원에 의존한다.

이펙티브 자바에서는 맞춤법 검사기를 예시로 설명하고 있는데, 여기서 맞춤법 검사기(SpellChecker)는 사전(Dictionary)에 의존하고 있고, 정적 유틸리티 클래스나 싱글턴 패턴으로 구현되어 있다.

```java
public class SpellChecker {
	private static final Lexicon dictionary = new KoreanDictionary();

	private SpellChecker() {} //객체 생성 방지

	public static boolean isValid(String word) { ... }
	public static List<String> suggestions(String type) { ... }
}
```

→ 정적 유틸리티를 잘못 사용한 예 - 유연하지 않고 테스트가 어렵다.

```java
public class SpellChecker {
	private final Lexicon dictionary = new koreanDictionary();

	private SpellChecker(...) {}
	public static SpellChecker INSTANCE = new SpellChecker(...);

	public boolean isValid(String word) { ... }
	public List<String> suggestions(String type) { ... }
}
```

→ 싱글턴 패턴을 잘못 사용한 예 - 유연하지 않고 테스트하기 어렵다.

두 방식은 모두 하나의 사전 만을 사용한다고 가정한다는 점에서 그리 적절하지 않다. 만약 다른 언어의 사전을 사용하고 싶은 경우에는 코드 자체를 변경 해야만 한다.

SpellChecker가 여러 사전을 사용할 수 있도록 만들기 위해서는 간단하게 dictionary 필드에서 final을 제거하고 다른 사전으로 교체하는 메소드를 추가할 수도 있지만, 이 방식은 오류가 발생하기 쉽고 **멀티 스레드 환경**에서는 사용할 수 없다.

이렇듯 사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않다.

대신 클래스가 여러 자원 인스턴스를 지원해야 하며 클라이언트가 원하는 자원을 사용해야 한다.

이 조건을 만족하는 간단한 패턴이 있는데 바로 **인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨주는 방식**이다.

의존 객체 주입의 한 형태로 맞춤법 검사기를 생성할 때, 의존 객체인 사전을 주입해주면 된다.

```java
public class SpellChecker {
	private final Lexicon dictionary;

	public SpellCheck(Lexicon dictionary) {
		this.dictionary = Objects.reqquireNonNull(dictionary);
	}

	public boolean isValid(String word) { ... }
	public List<String> suggestions(String type) { ... }
}
```

→ 의존 객체 주입은 유연성과 테스트 용이성을 높여준다.

의존 객체 주입 패턴은 매우 단순하여 많은 프로그래머들이 이 방식에 이름이 있다는 사실을 모른 채 사용해왔다.

예시에서는 dictionary라는 하나의 자원만을 사용하지만, 자원이 몇 개든 의존 관계가 어떻든 상관없이 잘 작동된다. 또한 불변을 보장하여 같은 자원을 사용하려는 여러 클라이언트가 의존 객체들을 안심하고 공유할 수 있기도 하다.

의존 객체 주입은 생성자, 정적 팩터리, 빌더 모두에 똑같이 응용할 수 있다.

의존 객체 주입이 **유연성**과 **테스트 용이성**을 개선해주긴 하지만, 의존성이 수천 개나 되는 큰 프로젝트에서는 코드를 어지럽게 만들기도 한다.

그래서 대거(Dagger), 주스(Guice), 스프링(Spring) 같은 의존 객체 주입 프레임워크를 사용하면 이런 어질러짐을 해소할 수 있다.

이런 프레임워크들은 의존 객체를 직접 주입하도록 설계된 API를 알맞게 응용해 사용하고 있다.
