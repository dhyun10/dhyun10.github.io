---
title: "[Spring] Entity와 DTO"
date: 2021-05-03 10:00:00 +0900
categories: [Programming, Springbot]
tags: [Spring, Springboot]
---

## Entity와 DTO

---

### Entity

- 실제 DB의 테이블과 1:1로 매핑되는 클래스로 DB 테이블 내에 존재하는 컬럼만을 속성으로 가져야한다.

- 상속을 받거나 구현체여서는 안된다.

- 테이블내에 존재하지 않는 컬럼을 가지면 안된다.

  ---

  - 예시

    ```java
    @Getter
    @NoArgsConstructor
    @Entity
    @Table(name = "POSTS")
    public class Posts extends BaseTimeEntity {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        @Column(length = 500, nullable = false)
        private String title;
    
        @Column(columnDefinition = "TEXT", nullable = false)
        private String content;
    
        private String author;
    
        @Builder
        public Posts(String title, String content, String author) {
            this.title=title;
            this.content=content;
            this.author=author;
        }
    
        public void update(String title, String content) {
            this.title=title;
            this.content=content;
        }
    }
    ```

    - @Entity - 테이블과 매핑할 클래스
      - 속성
        - name : JPA에서 사용할 엔티티 이름
        - 기본값 : 클래스 이름을 그대로 사용
    - @Table - Entity가 매핑될 테이블을 지정한다
      - 속성 
        - Name : 매핑할 테이블 이름
          - 기본값 : 엔티티 이름을 사용
        - Catalog : catalog 기능이 있는 DB에서 catalog를 매핑
        - schema : schema 기능이 있는 DB에서 schema를 매핑
        - uniqueConstraints : DDL 생성시 유니크 제약조건 생성
    - @Getter - 클래스 내 모든 필드의 Getter 메소드를 자동 생성
    - @NoArgsConstructor - 파라미터가 없는 기본 생성자를 생성, protected Posts() {}와 같은 효과
    - @Id - 해당 테이블의 pk 필드 매핑
    - @Column - 컬럼 매핑
      - 속성
        - name : 객체명과 DB 컬럼명을 다르게 하고 싶은 경우, DB 컬럼명으로 설정할 이름을 적는다.
        - length : 문자 길이 제약 조건, String 타입에만 사용
        - nullable : NOT NULL 제약조건
    - @Builder - 해당 클래스의 빌더 패턴 클래스 생성, 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함

### DTO

- 계층간 데이터 교환을 위한 객체, 주로 비동기 처리에 사용

- getter/setter 메서드만을 갖는다

- Request와 Response용 DTO는 View를 위한 클래스

  ---

  - 예시

    ```java
    @Getter
    @NoArgsConstructor
    public class PostsSaveRequestDto {
        private String title;
        private String content;
        private String author;
        
        @Builder
        public PostsSaveRequestDto(String title, String content, String author) {
            this.title=title;
            this.content=content;
            this.author=author;
        }
    
        public Posts toEntity() {
            return Posts.builder()
                    .title(title)
                    .content(content)
                    .author(author)
                    .build();
        }
    }
    ```



### Entity 클래스와 DTO 클래스를 분리하는 이유

- 수많은 서비스 클래스나 비지니스 로직들이 Entity 클래스를 기준으로 동작하기 때문에 Entity 클래스가 변경되면 여러 클래스에 영향을 끼치게 된다.
- Controller에서 결과값으로 여러 테이블을 조인해서 줘야하는 경우가 빈번하기 때문에 Entity 클래스만으로는 표현하기가 어려운 경우가 많다.

