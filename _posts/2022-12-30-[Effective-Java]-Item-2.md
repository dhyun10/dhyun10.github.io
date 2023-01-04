---
title: "[Effective Java] Item 2"
date: 2022-12-30 10:00:00 +0900
categories: [Study, Effective Java]
tags: [Java, Effective Java]
---

# Effective Java - Item 2

<aside>
💡 생성자에 매개변수가 많다면 빌더를 고려하라

</aside>

정적 팩토리와 생성자는 선택적 매개변수가 많을 때에는 대응하기 어렵다는 동일한 제약이 존재한다. 이런 문제의 해결책으로 세 가지 패턴을 들 수 있는데, 바로 점층적 생성자 패턴, 자바빈즈 패턴 그리고 빌더 패턴이다.

## ❓ 점층적 생성자 패턴 (Telescoping Constructor Pattern)이란?

필수 매개변수만 받는 생성자, 필수 매개변수와 선택 매개변수 1개를 받는 생성자, 선택 매개변수를 2개 받는 생성자 형태로 선택 매개변수를 전부 다 받는 생성자까지 늘려가는 방식이다. 

- 코드
    
    ```java
    public class NutritionFacts {
    	private final int servingSize; //필수
    	private final int servings; //필수
    	private final int calories; //선택
    	private final int fat; //선택
    	private final int sodium; //선택
    	private final int carbohydrate; //선택
    	
    	public NutritionFacts(int servingSize, int servings, int calories) {
    		this(servingSize, servings, calories, 0);
    	}
    	
    	public NutritionFacts(int servingSize, int servings, int calories, int fat) {
    		this(servingSize, servings, calories, fat, 0);
    	}
    
    	public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium) {
    		this(servingSize, servings, calories, fat, sodium, 0);
    	}
    	
    	public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium, int carbohydrate) {
    		this.servingSize = servingSize;
    		this.servings = servings;
    		this.calories = calories;
    		this.fat = fat;
    		this.sodium = sodium;
    		this.carbohydrate = carbohydrate;
    	}
    }
    ```
    → 해당 클래스에서 필수 매개변수는 2개뿐이지만, 이외의 선택 매개변수들도 어쩔 수 없이 값을 지정 해줘야 하기 때문에 효율이 떨어진다.
    

하지만 매개변수 개수가 많아지면 많아질수록 많은 조합이 만들어지고, 생성자의 수가 많아져 클라이언트 코드 작성 효율과 가독성이 떨어진다. 

또한 인자로 받는 매개변수 타입이 같은 경우에는 생성자를 만들 수 없을 뿐더러 생성자를 호출하는 입장에서는 해당 매개변수와 개수가 맞는지 항상 확인 해야한다는 불편함이 따른다.

## ❓ **자바빈즈 패턴 (JavaBeans Pattern)이란?**

매개변수가 없는 생성자로 객체를 만든 후, 세터(setter) 메서드들을 호출해 원하는 매개변수의 값을 설정하는 방식이다. 코드는 길어지지만 점층적 생성자 패턴에 비해 인스턴스를 만들기 쉽고 가독성 또한 좋다.

- 코드
    
    ```java
    public class NutritionFacts {
    	private int servingSize = -1;
    	private int servings = -1;
    	private int calories = 0;
    	private int fat = 0;
    	private int sodium = 0;
    	private int carbohydrate = 0;
    	
    	public NutritionFacts() {}
    	
    	public void setServingSize(int val) { servingSize = val; }
    	public void setServings(int val) { servings = val; }
    	public void setCalories(int val) { calories = val; }
    	public void setFat(int val) { fat = val; }
    	public void setSodium(int val) { sodium = val; }
    	public void setCarbohydrate(int val) { carbohydrate = val; }
    }
    ```
    → 해당 클래스에서 필수 매개변수는 2개뿐이지만, 이외의 선택 매개변수들도 어쩔 수 없이 값을 지정 해줘야 하기 때문에 효율이 떨어진다.
    
    

그러나 객체 하나를 생성하려면 메서드를 여러 개 호출해야 한다는 단점도 존재한다. 또한 한 번의 생성자 호출만으로 생성이 완료되지 않기 때문에 일관성(consistency)이 무너지고 변하지 않는 불변(immutable) 객체를 만들 수 없다.

마지막으로, 점층적 생성자 패턴의 **안전성**과 자바빈즈 패턴의 **가독성**을 결합한 디자인 패턴인 빌더 패턴이 존재한다.

## ❓ 빌더 패턴 (Builder Pattern)이란?

필수 매개변수만으로 정적 팩토리 메서드를 이용해 빌더 객체를 얻은 뒤, 빌더 객체가 제공하는 세터 메서드들로 필요한 선택 매개변수를 입력할 수 있다.

각각의 세터 메서드는 값을 설정한 뒤 자기 자신(Builder)을 반환하기 때문에 연속해서 메서드 호출이 가능하고, 마지막으로 `build()` 메서드를 사용해 필요한 객체를 완성하여 반환할 수 있다.

- 코드
    
    ```java
    package effectiveJava.item02;
    
    public class NutritionFacts3 {
    	private final int servingSize;
    	private final int servings;
    	private final int calories;
    	private final int fat;
    	private final int sodium;
    	private final int carbohydrate;
    	
    	public static class Builder {
    		private final int servingSize;
    		private final int servings;
    		
    		private int calories = 0;
    		private int fat = 0;
    		private int sodium = 0;
    		private int carbohydrate = 0;
    		
    		public Builder(int servingSize, int servings) {
    			this.servingSize = servingSize;
    			this.servings = servings;
    		}
    		
    		public Builder calories(int val) {
    			calories = val; 
    			return this;
    		}
    		
    		public Builder fat(int val) {
    			fat = val; 
    			return this;
    		}
    		
    		public Builder sodium(int val) {
    			sodium = val; 
    			return this;
    		}
    		
    		public Builder carbohydrate(int val) {
    			carbohydrate = val; 
    			return this;
    		}
    		
    		public NutritionFacts3 build() {
    			return new NutritionFacts3(this);
    		}
    	}
    	
    	private NutritionFacts3(Builder builder) {
    		servingSize = builder.servingSize;
    		servings = builder.servings;
    		calories = builder.calories;
    		fat = builder.fat;
    		sodium = builder.sodium;
    		carbohydrate = builder.carbohydrate;
    	}
    }
    ```
    → 점층적 생성자 패턴에 비해 필요한 인스턴스들만 생성하기 쉽다.
    
    👉 계층적으로 설계된 클래스에 빌더 패턴 사용 예제
    - 코드

        ```java
        package effectiveJava.item02;

        import java.util.Objects;
        import java.util.EnumSet;
        import java.util.Set;

        public abstract class Pizza {
            public enum Topping {HAM, MUSHROOM, ONION, PEPPER, SAUSAGE}
            final Set<Topping> toppings;

            abstract static class Builder<T extends Builder<T>> {
                EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class);

                public T addTopping(Topping topping) {
                    toppings.add(Objects.requireNonNull(topping));
                    return self();
                }

                abstract Pizza build();

                // 하위 클래스는 이 메서드를 오버라이딩하여 "this"를 반환해야한다.
                protected abstract T self();
            }

            Pizza(Builder<?> builder) {
                toppings = builder.toppings.clone();
            }
        }
        ```
        → 추상 메서드 self() 는 하위 클래스에서 형변환을 하지 않고도 자기 자신을 리턴하여 메서드를 연쇄적으로 호출할 수 있도록 한다.

        ```java
        package effectiveJava.item02;

        public class NyPizza extends Pizza {
          public enum Size {SMALL, MEDIUM, LARGE}
            private final Size size;

            public static class Builder extends Pizza.Builder<Builder> {
                private final Size size;

                public Builder(Size size) {
                    this.size = size;
                }

                @Override
                NyPizza build() {
                    return new NyPizza(this);
                }

                @Override
                protected Builder self() {
                    return this;
                }
            }

            private NyPizza(Builder builder) {
                super(builder);
                size = builder.size;
            }
        }
        ```
        → Pizza 의 하위 클래스, 크기(size)를 필수 매개변수로 받는다.

        ```java
        package effectiveJava.item02;

        public class Calzone extends Pizza { 
            private final boolean sauceInside;

            public static class Builder extends Pizza.Builder<Builder> {
              private boolean sauceInside = false;

                public Builder sauceInside() {
                    this.sauceInside = true;
                    return this;
                }

                @Override
                Calzone build() {
                    return new Calzone(this);
                }

                @Override
                protected Builder self() {
                    return this;
                }
            }

            private Calzone(Builder builder) {
                super(builder);
                sauceInside = builder.sauceInside;
            }
        }
        ```
        → Pizza 의 하위 클래스, 소스 선택(sauceInside)를 필수 매개변수로 받는다.

        ```java
        NyPizza pizza = new NyPizza.Builder(SMALL)
                .addTopping(ONION).build();
        Calzone calzone = new Calzone.Builder().sauceInside()
                .addTopping(HAM).build();
        ```
        → NyPizza와 Calzone 객체 생성
    
    위의 코드에서 `NyPizza.Builder`는 `NyPizza`를 반환하고 `Calzone.Builder`는 `Calzone`를 반환한다. 이처럼 하위 클래스의 메소드가 상위 클래스의 메소드에서 정의한 반환 타입이 아닌, 그 하위 타입을 반환하는 기능을 공변 반환 타이핑(convariant return typing)이라고 한다.<JDK 1.5 이상> 해당 기능 덕분에 클라이언트는 형변환에 신경 쓰지 않고 빌더를 사용할 수 있다.
    


물론 빌더 패턴도 단점이 없는 것은 아니다. 객체를 생성하기 위해서는 먼저 빌더 클래스부터 정의해야 한다. 빌더의 생성 비용이 큰 것은 아니지만, 성능에 민감한 상황에서는 문제가 될 수 있다.

또한 매개변수가 몇 개 없고, 변경될 가능성이 아예 없다면 코드가 다소 장황한 빌드 패턴보다는 점층적 생성자 패턴을 사용하는 것이 더 유리하다.

그리고 추가로 빌더 패턴을 작성하는 쉽고 편리한 방법이 존재하는데 바로 **Lombok** 라이브러리의 `@Builder` 어노테이션이다. `@Builder` 어노테이션은 모든 멤버 필드에 대해서 매개변수를 받는 기본 생성자를 만들어주기 때문에 따로 Builder Class를 작성할 필요가 없어 코드 작성에 매우 용이하다.

다만 접근 레벨이 default이기 때문에, 동일 패키지 내에서 해당 생성자를 호출 할 수 있는 문제가 생긴다. 그러므로 객체 생성 시 받아야 하는 매개변수들만 존재하는 생성자를 만들고 그 생성자에 `@Builder` 를 지정하는 것이 바람직하다.
