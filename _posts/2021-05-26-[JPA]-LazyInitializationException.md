---
title: "[JPA] LazyInitializationException"
date: 2021-05-26 10:00:00 +0900
categories: [Programming, JPA]
tags: [JPA, SpringBoot]
---

## org.hibernate.LazyInitializationException

---

- Reservation.java

  ```java
  @Entity
  public class Reservation extends BaseJpaModel<Long> {
      @Id
      @Column(name = "ID", precision = 19, nullable = false)
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      @ColumnPosition(1)
      private Long id;
  
      ...
  
      @OneToMany(cascade = CascadeType.ALL, fetch = FetchType.LAZY)
      @NotFound(action = NotFoundAction.IGNORE)
      @JoinColumn(name = "RSV_NUM", referencedColumnName = "RSV_NUM", insertable = false, updatable = false)
      private List<ReservationMemo> memoList;
  }
  ```



- ReservationResponseDto.java

  ```java
  @Getter
  public class ReservationResponseDto {
      private Long id;
      ...
  
      private List<ReservationMemo> memoList = new ArrayList<ReservationMemo>();
  
      public ReservationResponseDto (Reservation entity) {
          this.id = entity.getId();
          ...
  
          for (ReservationMemo memo : entity.getMemoList()) {
              if(memo.getDelYn().equals("N")) {
                  this.memoList.add(memo);
              }
          }
      }
  }
  ```

  

- ReservationServiceTest.java

  ``` java
    @Log
    @RunWith(SpringRunner.class)
    @SpringBootTest(classes = AXBootApplication.class)
    @FixMethodOrder(MethodSorters.NAME_ASCENDING)
    public class ReservationServiceTest {
        private static Logger logger= LoggerFactory.getLogger(ReservationServiceTest.class);
    
        @Autowired
        private ReservationService reservationService;    
    	
        @Test
        public void test2_예약_상세조회() {
            ReservationResponseDto dto = reservationService.selectOne(1752L);
            assertTrue(dto.getId() == 1752L);
        }
    }
  ```

- 오류메세지

  @OneToMany 어노테이션으로 매핑된 Reservation(주인 객체)와 ReservationMemo(종속 객체)를 조회하는 테스트코드를 실행했을 때 LazyInitializationException 오류 발생.

  ![lazy](https://user-images.githubusercontent.com/70506979/119601198-a23d6f00-be23-11eb-9858-e610eea03a77.PNG)

  

- 원인

  영속성 컨텍스트에 있던 Entity가 준영속(Detached) 상태가 됐을 때 Lazy Loading을 실행해서 영속성 컨텍스트에서 해당 객체를 조회해오려고 했을 때 발생 (출처: https://dundung.tistory.com/237 [DunDung])

  - 조회결과가 반환되며 트랜잭션이 이미 종료된 이후에 MemoList에 접근하려 했기 때문에

- 해결방법
  1.  @OneToMany(cascade = CascadeType.ALL, fetch = FetchType.EAGER) 로 FetchType 옵션 변경 (즉시 로딩)
     - 문제점 
       - 즉시 로딩을 사용할 경우 예상하지 못한 SQL이 발생할 수 있다.
       - 즉시 로딩은 JPQL 사용 시 N+1 문제를 유발한다.
  2.  단위 테스트 메소드에 @Transactional 어노테이션 추가
     - 메소드 전체를 하나의 트랜잭션으로 감싸 정상적으로 조회 가능