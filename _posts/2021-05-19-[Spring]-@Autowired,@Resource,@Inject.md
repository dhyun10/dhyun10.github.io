---
title: "[Spring] @Autowired,@Resource,@Inject"
date: 2021-05-19 10:00:00 +0900
categories: [Programming, Spring]
tags: [Spring]

---

- 모두 DI(의존성 주입) 을 위해 사용한다.

- 특정 Bean 기능을 수행하기 위해 기능에 필요한 특정한 Bean을 참조해야 하는데 이 때 특정 Bean에 자동 연결을 위해 위 어노테이션들을 사용한다.



### @Autowired

- 스프링 프레임워크에서 제공하는 어노테이션이기 때문에 타 프레임워크에서는 사용할 수 없다.
- 속성, Setter, 생성자에 모두 사용 가능하며 주입하려는 객체 타입이 일치하는 객체를 자동으로 주입
- 기본적으로 특정 빈을 찾지 못하면 예외를 던진다. 이 때 required 속성값을 false로 지정할 경우에는 빈 객체가 존재하지 않더라도 예외처리를 발생시키지 않는다.
  - @Qualifier 
    - 동일한 타입의 빈 객체가 여러개 정의되어 있을 경우 XML 설정파일에서 우선적으로 사용할 빈 객체의 <bean> 태그 하위에 <qualifier> 태그를 설정한다.
    - @Autowired와 함께 @Qualifier를 사용하여 XML 설정 파일에서 설정한 <qualifier> 태그의 value 값을 지정해준다. 이렇게 하면 동일한 타입의 빈이 여러 개일 경우 우선적으로 특정 빈이 주입된다.



### @Resource

- 자바에서 지원하는 어노테이션이기 때문에 프레임워크에 비종속적이다.

- 속성, Setter에서 사용 가능하며 주입하려는 객체 이름이 일치하는 객체를 자동으로 주입, 생성자는 사용할 수 없다.
- Resource를 통해 자동 주입을 하려면 주입하려는 Class에서 Bean의 id 이름과 동일해야 한다.



### @Inject

- 자바에서 지원하는 어노테이션이기 때문에 프레임워크에 비종속적이다.
- @Inject를 사용하기 위해서는 maven이나 gradle에 javax 라이브러리 의존성을 추가해야한다.
- 속성, Setter, 생성자에 모두 사용 가능하며 주입하려는 객체 타입이 일치하는 객체를 자동으로 주입
- @Autowired와 비슷하지만 Qualifier 대신 명시적으로 Bean을 표기한다.
  - @Named
    - @Autowired의 @Qualifier와 유사하지만 @Named에는 빈 이름(id)를 지정하므로 @Qualifier를 사용할 때에 비해 XML 설정 파일이 다소 짧아진다는 특징이 있다.
    - XML 설정 파일에 추가적으로 설정할 것이 없으며 @Inject와 함께 @Named를 사용하여 빈 이름을 지정해야한다.