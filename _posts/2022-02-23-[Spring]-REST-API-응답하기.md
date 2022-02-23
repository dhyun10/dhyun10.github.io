---
title: "[Spring] RESTful API 제대로 알아보기"
date: 2022-02-23 10:00:00 +0900
categories: [Programming, Spring]
tags: [Spring]

---

# REST란 
- HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.
- HTTP URI를 통해 자원을 명시하고, HTTP Method (POST,GET,PUT,DELETE)를 통해 해당 자원에 대한 CRUD OPERATION을 적용하는 것을 의마한다.
- 즉, REST는 자원 기반의 구조 설계의 중심에 Resource가 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐를 의미한다.
- 웹의 모든 자원에 고유한 ID인 HTTP URI를 부여한다.

---

# REST 기본 구성
- REST API는 REST(REpresentational State Transfer) 아키텍처 스타일의 디자인 원칙을 준수하는 API입니다.
- 자원 (Resource) - URL
  - 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
- 행위 (Verb) - Http Method
  - 기본 Method
  |Method|의미|역할|
  |------|---|---|
  |POST|Create|POST를 통해 해당 URI를 요청하면 리소스를 생성한다.|
  |GET|Select|GET를 통해 해당 리소스를 조회한다. 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져온다.|
  |PUT|Update|PUT를 통해 해당 리소스를 수정한다.|
  |DELETE|Delete|DELETE를 통해 해당 리소스를 삭제한다.|
- 표현 (Representations)
  - Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답을 보낸다.
  - REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타낼 수 있다.
  - 현재는 대부분 JSON으로 주고받는다.

---

# REST 특징
1. 클라이언트 / 서버 구조
   - 클라이언트는 유저와 관련된 처리를, 서버는 REST API를 제공함으로서 각각의 열할이 확실하게 구분되고 일괄적인 인터페이스로 분리되어 작동할 수 있게 한다.
   - REST Server : API를 제공하고 비지니스 로직 처리 및 저장을 책임진다.
   - Client : 사용자 인증이나 context(세션, 로그인정보) 등을 직접 관리하고 책임진다.
   - 서로간의 의존성이 줄어든다.
2. 무상태성 (Stateless)
   - REST는 HTTP의 특성을 이용하기 때문에 무상태성을 갖는다.
   - 즉 서버에서 어떤 작업을 하기 위해 상태정보를 기억할 필요가 없고 들어온 요청에 대해ㅐ 처리만 해주면 되기 때문에 구현이 쉽고 단순해진다.
3. 캐시 처리 기능 (Cacheable)
   - HTTP라는 기존 웹표준을 사용하는 REST의 특징 덕분에 기본 웹에서 사용하는 인프라를 그대로 사용 가능하다.
   - 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
   - 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상 시킬 수 있다.
4. 자체 표현 구조 (Self - descriptiveness)
   - JSON을 이용한 메세지 포맷을 이용하여 직관적으로 이해할 수 있고 REST API 메세지만으로 그 요청이 어떤 행위를 하는지 알 수 있다.
5. 계층화 (Layered System)
   - 클라이언트와 서버가 분리되어 있기 때문에 중간에 프록시 서버, 암호화 계층 등 중간매체를 사용할 수 있어 자유도가 높다.
6. 유니폼 인터페이스 (Uniform)
   - Uniform Interface는 HTTP 표준에만 따른다면 모든 플랫폼에서 사용이 가능하며, URI로 지정한 리소스에 대한 조작을 가능하게 하는 아키텍쳐 스타일을 말한다.
   - URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
   - 즉, 특정 언어나 기술에 종속되지 않는다.

---

# REST API 중심규칙
- URI는 정보의 자원을 표현해야 한다.
- 자원에 대한 행위는 HTTP Method로 표현한다.

- 유의
1. 슬래시 구분자(/)는 계층 관계를 나타내는데 사용한다.
2. URI 마지막 문자로 슬래시를 포함하지 않는다.
   - 즉 URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는 것은 리소스가 다르다는 것
   - 역으로 리소스가 다르면 URI도 달라져야 한다.
3. 하이픈(-)은 URI 가독성을 높이는데 사용하며 밑줄(_)은 사용하지 않는다.
4. URI 경로에는 소문자가 적합하다.
   ```
   user/viewUser (x) 
   user/view-user (o) 
   ```
5. 파일 확장자는 URI에 포함하지 않는다.
   - REST API 에서는 메세지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI안에 포함시키지 않는다.
   - 대신 Accept Header 사용 ex) `GET` orders/2/Accept: image/jpg
6. 리소스간에 연관 관계가 있는 경우
   - /리소스명/리소스ID/관계가 있는 다른 리소스명
   - ex) `GET` /users/2/orders (일반적으로 소유의 관계를 표현할 때 사용)

---

# RESTful API 디자인
1. 리소스와 행위를 명시적이고 직관적으로 분리해야한다.
2. Messaage 는 Header와 Body를 명확하게 분리해서 사용한다.
3. API 버전을 관리한다.
4. 서버와 클라이언트가 같은 방식을 사용해서 요청하도록 한다.

---

# RESTful API 장단점

- Open API를 제공하기 쉽다.
- 멀티플랫폼 지원 및 연동이 용이하다.
- 원하는 타입으로 데이터를 주고 받을 수 있다.
- 기존 웹 인프라(HTTP)를 그대로 사용할 수 있다.
  
- 사용할 수 있는 메소드가 한정적이다.
- 분산환경에는 부적합하다.
- HTTP 통신 모델에 대해서만 지원한다.
