---
title: "[Spring] JPA 연관관계"
excerpt: "본문의 주요 내용을 여기에 입력하세요"

categories:
  - Categories3
tags:
  - [tag1, tag2]

permalink: /categories3/post-name-here-3/

toc: true
toc_sticky: true

date: 2022-07-24
last_modified_at: 2022-07-24
---

# 양방향 연관관계
- 반대 방향으로 객체 그래프 탐색이 가능
  - 서로를 양쪽으로 참조할 수 있음
- 테이블은 단방향일 때와 같다
  - 테이블의 연관관계는 외래키 하나로 양방향이 다 있다
- 사실상 테이블의 연관관계에는 방향이 없다. 하지만 객체에서 다르다
  - TEAM은 Memeber를 가질 수 없다
  - TEAM에 List members를 넣어야 한다 -> mapped by 사용

- 객체의 연관관계는 2개
 - team -> member, member->team
 - 사실은 단방향 연관관계가 2개인데 이걸 양방향 연관관계라고 부른다
- 테이브를의 연관관계는 1개
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
