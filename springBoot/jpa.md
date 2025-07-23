# JPA

## JPA 정의

Java 진영에서 ORM 기술 표준으로 사용하는 인터페이스 모음

- JPA는 인터페이스이고 구현체는 주로 하이버네이트 사용

## 주요 용어
- ORM : 객체 지향 프로그래밍 언어에서 사용하는 객체와 관계형 데이터베이스의 테이블을 연결해주는 기술
- 영속성 컨텍스트 : 엔티티를 영구 저장하는 환경 (내 생각엔 디비에 저장하기 전 저장되는 공간)
    - 영속 : 영속성 컨텍스트에서 관리 중인 상태 => persist()
    - 비영속 : 영속성 컨텍스트와 연관이 없는 상태
    - 준영속 : 영속성 컨텍스트에 저장되어 있다가 분리된 상태 => detach(),clear(),close()
    - 삭제 : 영속성 컨텍스트에서 완전히 삭제된 상태 => remove()

## 연관관계
- @OneToOne : 일대일 매핑
    - 단방향일 때
      - user 테이블

            @OneToOne
            @JoinColumn(name = "blog_no")
            private Blog blog;

    - 양방향일 때 -> 단방향을 양쪽 테이블에서 연결
      - user 테이블

            @OneToOne
            @JoinColumn(name = "blog_no")
            private Blog blog;
      
      - blog 테이블

            @OneToOne(mappedBy = "blog")
            private User user;

- @ManyToOne : 다대일 매핑
  - 단방향일 때
    - member 테이블
      
          @ManyToOne
          @JoinColumn(name ="TEAM_ID")
          private Team team;

  - 양방향일 때 -> 단방향을 양쪽 테이블에서 연결
    - member 테이블
        
              @ManyToOne
              @JoinColumn(name ="TEAM_ID")
              private Team team;

    - team 테이블

              @OneToMany(mappedBy="team")
              private List<Member> members = new ArrayList<>();

- mappedBy는 테이블 컬럼에 추가되지 않는다.
- @MapsId : 복합키일 때 하나의 pk를 fk로 사용하고 싶을 때 사용 (@ManyToOne위에 붙임)

=> 주인은 joinColumn만 주인이 아니면 mappedBy 속성 적용
      OneToMany 단방향은 성능이 좋지 않음 -> 양방향 추천

양방향일 경우 양쪽 다 set을 해줘야한다.

ex) member.setTeam(team), team.getMembers().add(member)

## 복합키 설정 방법
1. ID로 사용할 객체 생성 -> 이 객체에 @Embeddable, implements serializable 구현
2. 엔티티에서 ID 객체 사용하고 ID 필드에 @EmbeddedId 어노테이션 붙여줌

## primary key가 AUTO_INCREMENT일 때
@GeneratedValue(strategy = GenerationType.IDENTITY) 사용

### GenerationType
- TABLE : 키 생성을 위해 별도의 시퀀스 테이블을 만들어 사용하는 방식
- SEQUENCE : DB의 시퀀스 객체를 사용하여 기본 키 생성
- IDENETITY : 기본 키 생성을 db에 위임하여 AUTO_INCREMENT 같은 기능 사용
- AUTO : JPA 구현체가 db에 맞게 자동으로 전략을 선택

## 객체지향 쿼리
1. jpql -> Entity 객체를 대상으로 쿼리(sql과 비슷하면서 조금 다름)

    ```
    query = em.createQuery("SELECT m FROM MemberEntity m WHERE m.username = :username", MemberEntity.class).setParameter("username",usernameParam)
    List result = query.getResultList()
    ```
       
    - return 타입은 TypedQuery가 있고 Query가 있는데 TypedQuery는 리턴타입이 확실할 때(위와 같이 확실히 member가 응답될 때), Query는 일부 필드만 검색되서 확실하지 않을 때 
    - 파라미터 바인딩은 이름 기준 파라미터 :username 또는 위치기반 파라미터 ?1

2. criteria -> jpql을 빌더패턴으로 바꿔서 사용 -> 오타를 줄일 수 있다.(queryDsl을 더 많이 씀)
    
    ```
    CriteriaBuilder cb = em.getCriteriaBuilder();
    
    CriteriaQuery<Member> cq = cb.createQuery(Member.class);

    Root<Member> m = cq.from(Member.class);

    Predicate usernameEqual = cb.equal(m.get("username"), name);

    cq.select(m).where(usernameEqual);

    List<Member> result = em.createQuery(cq).getResultList();
    ```

3. queryDsl -> criteria와 마찬가지로 jpql을 빌더패턴으로 바꿔서 사용

    ```
    query.selectFrom(member)
         .where(
                member.username.eq(name)
                    .and(member.age.goe(minAge))
            )
         .fetch();
    ```

- 서브쿼리 사용 시 New JpaSubQuery() 사용
- where 조건은 BooleanBuilder로 만듬
- 쿼리 타입 객체(Q)를 사용
- from, join 절에는 서브쿼리 불가능 -> jpql로 대체 가능

4. 네이티브 쿼리 -> sql을 직접 작성

   ```
   String sql = "SELECT id, age FROM MEMBER WHERE name = 'KIM'"
   List<Member> resultList = em.createNativeQuery(sql,Member.class).getResultList()
   ```

## Spring Data JPA
> CRUD를 편하게 사용할 수 있도록 Spring에서 제공해주는 기능

### Repository 자동 구현
- 인터페이스 정의 시 Spring Data JPA에서 기본으로 제공하는 함수들 사용 가능(findById, save 등)

    ```
    public interface MemberRepository extends JpaRepository<Member, Long> {}
    ```
###  메소드 이름 기반 쿼리 작성
- findByUserName(String userName) : select * from Member where userName = :userName 쿼리 생성

### 주의
- 대량으로 save 시 일정 개수마다 영속성 컨텍스트 초기화 해줘야 함 -> outOfMemory 방지

    ```
    em.flush();
    em.clear();
    ```
