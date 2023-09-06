---
title: "[Spring] JPA 연관관계로 Join 구현"
excerpt: "연관관계, 주인, 다중성"

categories:
  - Spring
tags:
  - [jpa, spring, 연관관계, join]

permalink: /Programming/Spring/JPA-연관관계/

toc: true
toc_sticky: true

date: 2023-08-21
last_modified_at: 2023-09-04
---
# 연관관계
## 단방향 연관관계
## 양방향 연관관계
- 반대 방향으로 객체 그래프 탐색이 가능
  - 서로를 양쪽으로 참조할 수 있음
- 테이블은 단방향일 때와 같다
  - 테이블의 연관관계는 외래키 하나로 양방향이 다 있다
- 사실상 테이블의 연관관계에는 방향이 없다. 하지만 객체에서 다르다
  - TEAM은 Memeber를 가질 수 없다
  - TEAM에 List members를 넣어야 한다 -> mapped by 사용

- 객체의 연관관계는 2개
 - team -> member, member->team
 - **사실은 단방향 연관관계가 2개인데 이걸 양방향 연관관계라고 부른다**
- 테이블의 연관관계는 1개
  - team <-> member (양방향)
  - foreign key 하나로 모든 연관관계가 끝!
- 객체는 참조가 2개이기 때문에 member에 수정이 생기면 member테이블의 값을 바꿀지 team의 List members를 바꿀지 고민된다
  - 그래서 둘 중 하나로 주인을 정해서 외래키를 관리할지 정해야 한다 -> 연관관계의 주인
 
# 연관관계의 주인
- 객체의 두 관계 중 하나를 연관관계의 주인으로 지정
- 주인이 아닌 쪽은 조회만. mappedBy로 주인지정
- 주인은 수정, 등록.
## 누가 주인으로 하면 좋을까
- 외래키가 있는 쪽이 주인
  - N:1이면 N 쪽
  - member
- 외래키가 없는 쪽이 가짜 주인 -> mappedBy
  - team

# 다중성
데이터베이스를 기준으로 다중성을 결정한다
## N:1 (다대일)
- Member(N) : Team(1) 
- N 쪽이 외래키를 가진다
### 단방향 연관관계로 구현
- N 쪽 테이블에서 해당 열에 @ManyToOne을 붙혀 구현
- @JoinColumn으로 외래키 관계를 맺는다
- 1 쪽 테이블에서는 참조하지 않는다
- N 이 연관관계의 주인이 됌

### 양방향 연관관계로 구현
- N 쪽 테이블에서 해당 열에 @ManyToOne을 붙혀 구현도 하고, 1 쪽 테이블에 @OneToMany를 추가한다.
- 연관관계의 주인을 mappedBy로 지정한다 -> 비즈니스 로직에 따라 결정

## 1:N (일대다)
- 1 쪽이 연관관계의 주인이 되는 상황이다.
- 실무에서 뿐만 아니라 DB입장에서도 N 쪽에서 외래키를 관리하기 때문에 잘 쓰지 않는다.
- 그러므로 패스

## N:N (다대다)
- 실무에서 사용시에 복잡 Join Query 발생의 위험으로 잘 사용하지 않는다.
- 다대다로 인해 생성된 중간 테이블은 두 테이블의 외래키가 저장되어 문제 발생 확률이 높다.
- 그래서 다대다는 일대다와 다대일로 풀어서 중간 entity 테이블을 두는 것이 좋다.


### Reference
- [[JPA] @ManyToMany, 다대다[N:M] 관계](https://ict-nroo.tistory.com/127)
- [JPA 연관 관계 한방에 정리 (단방향/양방향, 연관 관계의 주인, 일대일, 다대일, 일대다, 다대다)](https://jeong-pro.tistory.com/231)