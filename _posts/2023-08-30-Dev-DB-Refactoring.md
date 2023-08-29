---
title: "[DB] DB-Refactoring-필요성-및-근거-찾기"
excerpt: "왜 DB refactoring을 해야만 하는가? 방법, 절차, 근거"

categories:
  - Dev
tags:
  - [Database, DB, refactoring,리팩토링, 이유, 근거, 필요성]

permalink: /Dev/DB Refactoring 필요성 및 근거 찾기/

toc: true
toc_sticky: true

date: 2023-08-30
last_modified_at: 2023-08-30
---
# 서론
DB Refactoring의 필요성이 팀 내에서 대두되었다. 내가 제안했다.  
Json으로 만들어진 열에 접근할 일이 많았다. 쿼리 하나를 완성하는데 시간이 오래걸렸고,
차라리 하나의 독립된 table이면 훨씬 편했을 것이라는 생각이 많이 들었다.

주위의 자문을 구했을 때,  
Json 타입으로 만들거면 해당 열은 log 처럼 create, read만 할 경우에 적절하다고 했다.  
update가 자주 일어나면 이후에 쿼리를 수정할 때도 그 쿼리를 이해하기 힘들 뿐만 아니라 유지보수도 하기 힘들어진다고 들었다.
하지만 이미 json으로 만들어진건 엎어진 물이었다. 그래서 해당 Json으로 된 열 (이제 `Json 열`이라고 칭할 것이다.)을
1. 그냥 둘지
2. 바꿀지(다른 테이블로 빼내는 등)
3. 바꿀 거면 왜 바꿔야 하는지 근거 찾기

중 3번을 택했다

# 정보관리기술사 DB Refactoring 관련 지식

### 참고 자료
- [DB Refactoring](https://itpenote.tistory.com/640)
- [DB Smell](https://itpenote.tistory.com/641)

## DB Refactoring
> 데이터베이스의 의미 변환 없이 디자인을 개선하는 것

## 데이터 리팩토링 필요성
![image](https://github.com/choiiis/minimal-mistakes-choiiis-customized/assets/41178045/8e079b85-19ae-46d8-828b-5fe536aaaea1)
이건 지식적인 부분인 것 같고, 나는 `개발자의 불편함`도 추가하고 싶다.
`개발자의 불편함`을 수치화하여 내 고통을 드러내고 해결하는 것이 목표이다.

## DB Refactoring 절차
DB Refactoring 절차 중 내가 하고 싶은 것만 골라 적었다..
1. DB Smell에 의한 DB Refactoring 필요 요건 확인
2. 필요 DB Refactoring 유형 선택
3. 기존 스키마 분석
4. 데이터베이스 스키마 수정
5. 원본 데이터에 대한 Migration
6. 외부 액세스 프로그램 업데이트

## DB Smell
- Multi-Purpose column
- Multi-Purpose Table
- Redundant Data
- Tables with Many Columns
- Tables with Many Rows
- Smart Columns
- Fear of Change

## DB Refactoring 유형
- 구조 리팩토링
- 데이터 품질 리팩토링
- 참조무결성 리팩토링
- 아키텍처 리팩토링
- 기능 리팩토링
- 변환

# 프로젝트에 적용
## 1. DB Smell
정확히 일치하는 DB Smell이 있진 않지만 **Multi-Purpose Table** 라고 생각했다.
> Multi-Purpose Table
  단일 테이블이 여러 유형의 엔티티를 저장하는데 사용
처음 ERD를 설계할 때 해당 Json 열은 사실 하나의 Entity 였다.
그래서 이런 문제 생겼다고 판단했다.
+) 나중에 ERD 전 후 넣으면 좋을 것 같다

## 2. DB Refactoring 유형 선택
`Multi-Purpose Table`을 해결하기 위해한 DB Refactoring 방법으로 `구조 리팩토링`이 적절하다고 생각했다.
> 구조 리팩토링
  데이터베이스 스키마의 테이블 구조 변경
Json 열을 다시 하나의 Entity로 가정하여 새로운 테이블을 추가하는 방법을 생각했다.  
하지만 이건 내 생각이지 근거가 부족한 것 같다.

## 3. 기존 스키마 분석
ChatGPT에 물어본 결과 SQL Profiler, SQL Query Analyzer라는 **SQL문 분석도구**가 있는데 이건 현 프로젝트에서 사용하는 PostgreSQL에는 적용할 수 없는 도구였다. 대신 PostgreSQL에서 제공하는 Explain과 Analyze를 사용해보기로 했다.

### 방식
대상은 최근 짠 쿼리 중에 `Explain과 Analyze`를 적용하고,  
실제로 Json 열을 Table로 빼내서 같은 기능을 하는 쿼리를 실행했을 때의  
`Execution Time`을 비교해보려 한다.  

### 기존 쿼리 분석
```sql
EXPLAIN ANALYZE with rankingCert as(
select
  m.kakao_id,
  m.username,
  cert_data->>'date' AS date,
  cert_data->>'check' as check,
  data_row ->> 'challenge_id' as c_id
FROM "member" m ,
     jsonb_array_elements(m.certification) AS data_row,
     jsonb_array_elements(data_row->'cert') AS cert_data
where CAST(cert_data ->> 'date' as date) >= date_trunc('week',current_date)::date
and cert_data->>'check' = 'PASS')
select r.username as username, sum(c.saved_money) as smoney, count(*) as cnt,
rank() over (order by sum(c.saved_money) desc,  count(*) desc)
from rankingCert r join challenge c on r.c_id::int = c.id
group by r.username;
```
<img width="783" alt="image" src="https://github.com/choiiis/minimal-mistakes-choiiis-customized/assets/41178045/ebcde235-c447-4bc3-8e0a-a1d0b4dc4d8f">

사진과 같이 `Execution Time`은 8.388ms 였다

### 새 쿼리 분석
Json 열을 새로운 테이블로 만드는 쿼리를 돌렸다
```sql
CREATE TABLE certification AS
SELECT
m.username ,
data_row ->> 'challenge_id' AS challenge_id,
  cert_data->>'date' AS date,
  cert_data->>'image' AS image,
  cert_data->>'check' as check,
  cert_data->>'message' AS message
FROM "member" m ,
     jsonb_array_elements(m.certification) AS data_row,
     jsonb_array_elements(data_row->'cert') AS cert_data;
```

그 이후 분석을 진행했다
```sql
EXPLAIN ANALYZE  select r.username as username, sum(c.saved_money) as smoney, count(*) as cnt,
rank() over (order by sum(c.saved_money) desc,  count(*) desc)
from certification r join challenge c on r.challenge_id::int = c.id
WHERE r.date::date>= date_trunc('week',current_date)::date
AND r.CHECK = 'PASS'
group by r.username;
```
<img width="783" alt="image" src="https://github.com/choiiis/minimal-mistakes-choiiis-customized/assets/41178045/069eb057-1e33-4cf5-8b9c-c9391dc93dc2">
`Execution Time`은 3.173ms 였다

# 결론
쿼리 시간도 짧고 실행계획도 짧아졌다.
다음에는 DB 형상관리 툴을 조사하여 DB Refactoring 으로 바뀔 데이터베이스 관리를 할 수 있도록 해야겠다.

### Reference
- [사진 1](https://quizlet.com/kr/393279073/2-db-hony0426-flash-cards/)