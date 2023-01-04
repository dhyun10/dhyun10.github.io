---
title: "[Effective Java] Item 1"
date: 2022-12-29 10:00:00 +0900
categories: [Study, Effective Java]
tags: [Java, Effective Java]
---

# Effective Java - Item 1

<aside>
💡 생성자 대신 정적 팩토리 메서드를 고려하라

</aside>

## ❓ 정적 팩토리 메서드(Static Factory Method)란?

오직 클래스의 인스턴스만을 반환하기 위해 정의된 정적 메소드를 정적 팩토리 메소드라고 한다.

- 코드
    
    ```java
    public final class LocalTime
    			implements Temporal, TemporalAdjuster, Comparable<LocalTime>, Serializable {
    	
    	...
    		public static LocalTime of(int hour, int minute, int second) {
    		        HOUR_OF_DAY.checkValidValue(hour);
    		        if ((minute | second) == 0) {
    		            return HOURS[hour];  // for performance
    		        }
    		        MINUTE_OF_HOUR.checkValidValue(minute);
    		        SECOND_OF_MINUTE.checkValidValue(second);
    		        return new LocalTime(hour, minute, second, 0);
    		}
    
    	...
    		private LocalTime(int hour, int minute, int second, int nanoOfSecond) {
    		        this.hour = (byte) hour;
    		        this.minute = (byte) minute;
    		        this.second = (byte) second;
    		        this.nano = nanoOfSecond;
    		}
    	...
    }
    ```
    

그렇다면 왜 생성자를 호출하지 않고 정적 팩토리 메소드를 사용하는지, 정적 팩토리 메소드의 장/단점을 정리해보자.

### 장점

**첫 번째, 이름을 가질 수 있다.**

: 객체를 생성자로 호출할 때에는 클래스의 매개변수가 많을 수록, 같은 타입의 매개변수가 여러 개 존재할수록, 개발자들은 각 생성자들이 어떤 의미인지 쉽게 파악할 수 없다. 또한, public 생성자는 하나의 시그니처로 하나만 생성할 수 있다. 예를 들어, 학교를 의미하는 클래스에 학생과 선생님이 존재한다. 이때 학생의 이름인 String 변수를 받는 생성자가 존재한다면 선생님의 이름인 String 변수를 받는 생성자는 오류가 발생한다. 이미 하나의 String 변수를 받는 생성자가 존재하기 때문에 다른 생성자가 호출될 수 있기 때문이다.
반면 정적 팩토리 메서드에서는 메서드명을 지정할 수 있기 때문에 한 클래스에 시그니처가 같은 생성자가 여러 개 존재하는 경우에는 생성자를 정적 팩토리 메서드로 바꾸고 메소드명으로 각각의 특성을 나타내어 사용할 수 있다.

- 코드
    
    ```java
    public class School {
    	String studentName;
    	String teacherName;
    	int grade;
    
    	public School(String studentName) {
    		this.studentName = studentName;
    	}
    
    // 에러
    	public School(String teacherName) { 
    		this.teacherName= teacherName;
    	}
    
    	public static School createStudent(String name, int grade) {
    		return new School(name, grede);
    	}
    
    	public static School createTeacher(String name, int grade) {
    		return new School(name, grede);
    	}
    
      public static void main(String[] args) {
    		School student = createStudent("학생", 3);
    		School teacher = createTeacher("선생님", 1);
    	}
    }
    ```
    → 매개변수의 종류와 개수가 같지만 메소드명으로 인해 반환되는 인스턴스의 명확한 구분이 가능하다.
    

**두 번째, 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.**

: 주로 사용되는 불변 객체가 존재한다면 미리 생성된 인스턴스를 캐싱하여 재활용함으로써 불필요하게 객체를 생성할 필요가 없다.
대표적으로 `Boolean.valueOf(boolean)` 메소드가 있다. `Boolean` 클래스에서는 TURE, FALSE를 상수로 정의해 놓고 `valueOf()` 메소드가 호출 되었을 때 객체를 새로 생성하지 않고 존재하는 상수를 반환해준다.

- 코드
    
    ```java
    public final class Boolean implements java.io.Serializable,
                                          Comparable<Boolean>
    {
        public static final Boolean TRUE = new Boolean(true);
        public static final Boolean FALSE = new Boolean(false);
    	...
    		public static Boolean valueOf(boolean b) {
            return (b ? TRUE : FALSE);
        }
    	...
    }
    ```
    → valueOf(b) 메소드를 호출하면 이미 생성되어 있는 객체를 반환한다.
    

**세 번째, 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.**

: 하위 클래스들이 하나의 상위 클래스를 상속 받는 구조인 경우에 상위 클래스에서는 어떠한 조건에 따라 하위 클래스 객체들을 반환할 수 있다. 이때, 하위 클래스의 구현체를 공개하지 않고도 반환이 가능하기 때문에 API를 작게 유지할 수 있다.
대표적으로, `java.util.Collections` 클래스에서는 수정 불가나 동기화 등의 기능을 포함하여 총 45개의 유틸리티 구현체를 제공하는데 대부분 정적 팩토리 메소드를 통하여 얻을 수 있다.

- 코드
    
    ```java
    public class Arrays {
    	...
    		public static <T> List<T> asList(T... a) {
            return new ArrayList<>(a);
        }
    	...
    }
    ```
    → java.util.Arrays 클래스에서 asList() 메소드를 통해 List의 하위 클래스인 ArrayList를 반환한다.
    

**네 번째, 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.**

: 단순히 하위 클래스만을 반환하는 것이 아니라 매개변수에 따라서 각각 다른 하위 클래스의 객체를 반환하는 것이 가능하다. 이때, 클라이언트는 어떤 객체가 반환 되는 지에 대해서는 알 필요가 없다.
예를 들어 `EnumSet` 클래스의 `noneOf()`메소드는 universe 값에 따라서 다른 하위 클래스를 반환해준다.

- 코드
    
    ```java
    public abstract class EnumSet<E extends Enum<E>> extends AbstractSet<E>
        implements Cloneable, java.io.Serializable
    {
    		public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
            Enum<?>[] universe = getUniverse(elementType);
            if (universe == null)
                throw new ClassCastException(elementType + " not an enum");
    
            if (universe.length <= 64)
                return new RegularEnumSet<>(elementType, universe);
            else
                return new JumboEnumSet<>(elementType, universe);
        }
    }
    ```
    → EnumSet 클래스의 noneOf() 메소드는 universe의 length 값이 64개 이하일 경우 RegularEnumSet, 아닐 경우에는 JumboEnumSet 객체를 반환해준다.
    

**다섯 번째, 정적 팩토리 메서드를 작성하는 시점에는 반환할 객체의 클래스는 존재하지 않아도 된다.**

: 지금까지는 이미 구현되어 있는 구현체가 기준이 되었지만, 정적 팩토리 메소드를 활용했을 때에 구현되어있지 않은 객체의 클래스가 존재하지 않아도 된다.
이러한 특징은 **서비스 제공자 프레임워크(Service Provider Framework)** 의 근간이 된다. 
서비스를 제공하는 제공자는 서비스의 구현체이고, 이 구현체들을 클라이언트에 제공하는 역할을 프레임워크가 통제하여 클라이언트를 구현체로부터 분리해준다.
서비스 제공자 프레임워크는 3개의 핵심 컴포넌트로 이루어져 있다.

- **서비스 인터페이스 (service interface)** : 구현체의 동작을 정의하는 인터페이스
- **제공자 등록 API (provider registration API)** : 제공자가 구현체를 등록하는 API
- **서비스 접근 API (service access API)** : 클라이언트가 서비스의 인스턴스를 얻을 때 사용하는 API

이 밖에 종종 네 번째 컴포넌트인 **서비스 제공자 인터페이스 (service provider interface)** 가 쓰인다.

해당 인터페이스가 없다면 각 구현체를 인스턴스로 만들 때 리플렉션을 사용해야 한다.

대표적인 서비스 제공자 프레임워크로는 JDBC(Java Database Connectivity)가 존재한다.

- JDBC : java에서 데이터베이스에 접속할 수 있도록 하는 java API
    
    — `Connection` : 서비스 인터페이스
    
    — `DriverManager.registerDriver` : 제공자 등록 API
    
    — `DriverManager.getConnection` : 서비스 등록 API
    
    — `Driver` : 서비스 제공자 인터페이스
    

### 단점

**첫 번째, 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩토리 메서드만 제공하면 하위 클래스를 만들 수 없다.**

: 예를 들어 java.util.Collections 클래스의 경우에는 모든 생성자가 private로 선언되어 있기 때문에 java.util.Collections로 만든 구현체는 상속이 불가능하다.
다만 이러한 점은 상속보다 컴포지션을 사용하도록 유도하고 불변 타입으로 만들기 위해 제약을 지켜야 한다는 점에서 오히려 장점이 될 수 있다.

**두 번째, 정적 팩토리 메소드는 프로그래머가 찾기 어렵다.**

: public 생성자는 API 설명에서 확인할 수 있기 때문에 프로그래머들이 알기 쉽지만, 정적 팩토리 메소드같은 경우에는 javadoc에서도 따로 정리하지 않기 때문에 API 문서를 잘 작성하고 메소드명을 알려진 규약에 따라 지어 프로그래머가 알기 쉽게 해주어야 한다.

- **from** : 매개변수를 하나를 받아 해당 타입의 인스턴스를 반환하는 형변환 메서드
    
    → `Date d = Date.from(instant);`
    
- **of** : 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
    
    → `Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);`
    
- **valueOf** : from과 of의 더 자세한 버전
    
    → `BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);`
    
- **instance** / **getInstance** : (매개변수를 받는다면) 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.
    
    → `StackWalker luke = StackWalker.getInstance(options);`
    
- **create** / **newInstance** : instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
    
    → `Object newArray = Array.newInstance(classObject, arrayLen);`
    
- **get*Type*** : getInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩토리 메서드를 정의할 때 쓴다. “*Type*”은 팩토리 메소드가 반환할 객체의 타입이다.
    
    → `FileStore fs = Files.getFileStore(path);`
    
- **new*Type*** : newInstance와 같으나 생성할 클래스가 아닌 다른 클래스에 팩토리 메서드를 정의할 때 쓴다. “*Type*”은 팩토리 메소드가 반환할 객체의 타입이다.
    
    → `BufferedReader br = Files.newBufferedReader(path);`
    
- ***type*** : get*Type*과 new*Type*의 간결한 버전
    
    → `List<Complaint> litany = Collections.list(legacyLitany);`
