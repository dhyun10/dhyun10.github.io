---
title: "[Effective Java] Item 7"
date: 2023-02-02 09:00:00 +0900
categories: [Study, Effective Java]
tags: [Java, Effective Java]
---

# Effective Java - Item 7

<aside>
💡 다 쓴 객체 참조를 해제하라

</aside>

자바처럼 가비지 컬렉터를 갖춘 언어를 사용하는 경우 메모리 관리에 신경 쓰지 않아도 된다고 생각할 수 있지만, 절대 무시해서는 안된다.

```java
import java.util.Arrays;
import java.util.EmptyStackException;

public class Stack {
	private Object[] elements;
	private int size = 0;
	private static final int DEFAULT_INITIAL_CAPACITY = 16;
	
	public Stack() {
		elements = new Object[DEFAULT_INITIAL_CAPACITY];
	}
	
	public void push(Object e) {
		ensureCapacity();
		elements[size++] = e;
	}
	
	public Object pop() {
		if (size == 0) 
			throw new EmptyStackException();
		return elements[--size];
	}
	
	public void ensureCapacity() {
		if (elements.length == size) 
			elements = Arrays.copyOf(elements, 2 * size + 1);
	}
}
```

→ 스택을 구현한 간단한 코드

위 코드는 **스택**을 간단하게 구현한 코드이다.

언뜻 보면 아무런 문제가 없는 코드라고 생각하기 쉽지만, 해당 코드에서는 스택에서 꺼내진 객체들을 가비지 컬렉터가 회수하지 않는다.

스택이 배열에서 여전히 그 객체들을 참조하고 있기 때문이다. 이렇게 객체 참조를 하나 살려두면 해당 객체 뿐 아니라 그 객체가 참조하고 있는 모든 객체를 회수 하지 못한다.

때문에 계속해서 실행된다면 가비지 컬렉션의 활동과 메모리 사용량이 늘어나 성능이 저하된다.

메모리 누수가 심한 경우에는 *디스크 페이징*이나 *OutOfMemoryError*가 발생하여 예기치 않게 프로그램이 될 수도 있다.

이러한 문제의 해법은 바로 다 쓴 객체는 null 처리하여 참조를 해제하는 것이다.

```java
public Object pop() {
	if (size == 0)
	  throw new EmptyStackException();
	Object result = elements[--size];
	element[size] = null; // 다 쓴 객체 참조 해제
	return result;
}
```

스택은 자체적으로 메모리를 관리하고, 배열의 활성화 영역에 속한 객체 만을 사용하고 비활성화 영역은 사용하지 않는데 가비지 컬렉터는 이 사실을 알 수가 없다. 그렇기 때문에 null 처리를 통해 가비지 컬렉터에게 이 객체를 사용하지 않는다는 것을 알려야 한다.

일반적으로 스택처럼 자기 메모리를 직접 관리하는 클래스라면 객체를 다 사용한 즉시 null 처리하여 메모리 누수에 주의해야 한다.

다만 항상 모든 객체를 일일이 null 처리를 하는 것은 프로그램을 지저분하게 만들 뿐이므로 객체 참조를 null 처리하는 일은 예외적인 경우여야 한다.

객체 참조를 해제하는 가장 좋은 방법은 그 참조를 담은 변수를 유효 범위(Scope) 밖으로 밀어내는 것이다.

**캐시** 역시 자기 메모리를 직접 사용하기 때문에 메모리 누수를 일으킨다.

만약 key를 참조하는 동안에만 entry가 살아있으면 되는 캐시라면 `WeakHashMap`을 사용하여 캐시를 생성하자. 다 쓴 entry는 그 즉시 자동으로 제거된다.

하지만 대부분 캐시 엔트리의 유효 기간을 정확히 정의하기가 어렵다. 그래서 캐시에 넣은 시간이 길수록 엔트리의 가치를 떨어뜨리는 방식을 주로 사용한다.

이런 방식에서는 ScheduledThreadPoolExcutor 같은 백그라운드 스레드를 사용하거나 부수 작업으로 엔트리를 청소해주어야 한다.

마지막으로 listener와 callback 역시 메모리 누수가 일어나는 주범이다.

클라이언트가 콜백을 등록만 하고 명확하게 해지하지 않는다면, 무언가 조치해주지 않는 이상 계속해서 쌓여간다. 이럴 때 콜백을 약한 참조(weak reference)로 저장하면 가비지 컬렉터가 직접 수거해갈 수 있다.

> 메모리 누수는 겉으로 잘 드러나지 않기 때문에 발견하기가 어렵다.
철저한 코드 리뷰나 힙 프로파일러 같은 디버깅 도구를 동원해야만 발견하는 경우도 있다. 그렇기 때문에 예방법을 철저히 익혀두는 것이 가장 중요하다.
>