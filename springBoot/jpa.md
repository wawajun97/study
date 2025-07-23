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

## Spring Data JPA
> CRUD를 편하게 사용할 수 있도록 Spring에서 제공해주는 기능

- 대량으로 save 시 일정 개수마다 영속성 컨텍스트 초기화 해줘야 함

```
em.flush();
em.clear();
```
