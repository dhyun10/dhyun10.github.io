---
title: "[Effective Java] Item 3"
date: 2023-01-04 10:00:00 +0900
categories: [Study, Effective Java]
tags: [Java, Effective Java]
---

# Effective Java - Item 3

<aside>
💡 private 생성자나 열거 타입으로 싱글턴임을 보증하라

</aside>

## ❓ 싱글턴(singleton)

객체의 유일성을 보장하기 위해서 인스턴스를 오직 하나만 생성할 수 있는 패턴을 의미한다.

일반적으로 싱글턴 객체에 대한 참조를 public static 필드나 public static 메서드로 노출하기 때문에 어디에서나 싱글턴 객체 접근이 가능하다.

싱글턴 패턴을 활용하면 객체를 재사용 할 수 있기 때문에 메모리 사용 낭비를 막을 수 있고 전역 객체이기 때문에 다른 객체와도 순조롭게 공유가 가능하다.

보통 두 가지 방법을 사용하여 싱글턴을 생성한다.
첫 번째는 public static 멤버를 **final**로 선언하여 생성하는 방법이다.
또한 외부에서는 생성자를 통해 객체를 생성할 수 없도록 접근 권한을 **private**로 선언해야한다.

- public static final 필드
    
    ```java
    public class Singleton { 	 	
    	public static final Singleton INSTANCE = new Singleton(); 	 	
    	private Singleton() {}  
    } 
    ```
    
    → 클래스 생성시에 인스턴스를 딱 한 번 생성한다.
    
    해당 방법은 클래스가 싱글턴임을 명확하게 알 수 있고 간결하게 표현할 수 있다는 장점을 가지고 있다.
    
    다만, 리플렉션 API를 사용하여 예외적으로 강제로 생성자를 생성할 수 있는 방법이 존재하기 때문에 이를 주의하여 코드를 작성해야 한다.  
    
    👉 리플렉션 API를 사용하여 생성자를 호출할 경우
    
    - 코드
    
    ```java
    import java.lang.reflect.Constructor;
    import java.lang.reflect.InvocationTargetException;
    
    public class SingletonReflect {
    
    	public static void main(String[] args) throws NoSuchMethodException, SecurityException, InstantiationException, IllegalAccessException, IllegalArgumentException, InvocationTargetException {
    		Constructor<Singleton> declaredConstructor = Singleton.class.getDeclaredConstructor();
    		declaredConstructor.setAccessible(true);
    		
    		Singleton singleton = declaredConstructor.newInstance();
    	}
    
    }
    ```
    
    →  `setAccessible(boolean)`메서드를 사용하여 Singleton 클래스의 **private** 생성자에 접근이 가능하다.
    
    ```java
    public class Singleton {
    	
    	public static final Singleton INSTANCE = new Singleton();
    	
    	private Singleton() {
    		if(INSTANCE != null) {
    			throw new UnsupportedOperationException("생성자를 호출할 수 없습니다.");
    		}
    	}
    
    }
    ```
    
    → 예외를 사용하여 인스턴스가 존재할 경우 생성자 호출 불가하도록 막는다.
    

두 번째는 생성자는 private로 선언하고 정적 팩토리 메소드를 활용하는 방법이다.

- 코드

```java
public class Singleton {
	
	private static Singleton INSTANCE = new Singleton();
	
	private Singleton() {}
	
	public static Singleton getInstance() {
		if(INSTANCE == null) {
			INSTANCE = new Singleton();
		}
		return INSTANCE;
	}

}
```

→ 클래스 생성시에 인스턴스를 딱 한번만 생성한다. `getInstance()` 메소드를 사용하는 경우에 이미 존재하는 인스턴스를 반환해주어 항상 같은 객체의 참조를 반환한다.

정적 팩토리 메소드를 활용한다면 싱글턴이 아니도록 하고 싶을 때에도 API를 변경하지 않고도 쉽게 다른 객체를 반환하도록 변경할 수 있다. 또한 메소드 참조를 공급자로 사용할 수 있고(Singleton::getInstance), 정적 팩토리를 제네릭 싱글턴 팩토리로 만들 수 있다.

❓**제네릭 싱글턴 팩토리**

> 제네릭으로 타입설정 가능한 인스턴스를 만들어두고, 반환 시에 제네릭으로 받은 타입을 이용해 타입을 결정하는 것이다.
> 

해당 방법도 역시 리플렉션 API를 사용하여 강제로 인스턴스를 생성할 수 있기 때문에 주의가 필요하다.

public static 필드나 정적 팩토리 메소드를 활용하여 작성한 싱글턴 클래스를 직렬화하려면 단순히 Serializable을 구현하고 선언하는 것만으로는 부족하다. 모든 인스턴스 필드를 일시적(**transient**)이라고 선언하고 `ReadResolve` 메서드를 제공해야한다. 그렇지 않으면 역직렬화시 새로운 인스턴스가 생성되기 때문이다.

마지막으로, Effective Java에서 싱글턴 패턴을 생성하는데 가장 이상적인 방법으로 알리고 있는 Enum을 활용하여 원소가 하나인 열거 타입을 선언하는 방법이 존재한다.

```java
public enum SingletonEnum {
	INSTANCE;
}
```

❓ **Enum**

> 클래스처럼 보이게 하는 상수이며, 서로 관련있는 상수들끼리 모아 상수들을 정의할 수 있다.
> 

해당 방법은 간결하게 작성할 수 있을뿐더러 어떠한 방식에서도 인스턴스가 추가로 생성되는 것을 막을 수 있다. 또 직렬화를 위해 추가적인 코드를 작성할 필요가 없기 때문에 싱글턴 객체를 생성할 때에는 Enum을 활용하는 방법을 가장 좋은 방법이라고 말할 수 있다.