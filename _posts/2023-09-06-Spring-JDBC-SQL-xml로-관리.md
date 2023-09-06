---
title: "[Spring] SQL 직접 사용 시(JDBC) xml로 쿼리 관리하기"
excerpt: "PropertySource, xml, sql,쿼리, 관리, 저장,jdbc,spring,boot"

categories:
  - Spring
tags:
  - [jpa, spring, 연관관계, join]

permalink: /Programming/Spring/JDBC-SQL-xml로-관리/

toc: true
toc_sticky: true

date: 2023-09-06
last_modified_at: 2023-09-06
---
# 문제 제기
JDBC에 직접 SQL 쿼리 문을 넣고 있었다.  
하지만 String concat 이 되어 재사용도 어려웠고, 가독성도 좋지 않았다.  
무엇보다 쿼리가 길어지면서 전체적인 파일 길이가 너무 길어지게 되었다.  

> 그래서 SQL문만을 다른 파일로 빼내어 관리하는 것이 편할 것이라는 생각이 들었고,  
  xml 파일에 sql 쿼리 문을 모아놓고 가져와서 사용할 수 있게 하였다.   

# 해결 전 상황

```java
@Override
    public List<MyRankingInfoDto> findRankingInfoList(){
        private String rankingList="SELECT * FROM member"
        List<MyRankingInfoDto> certRankingList = template.query(rankingList, rankingRowMapper());
        return certRankingList;
    }
```
위의 예시에서는 간단한 쿼리를 넣어서 왜 따로 빼야하는지 눈에 잘 보이지 않지만, 길다고 상상해주면 좋을 것 같다.  

# 해결 과정

## xml 파일 생성
resources 아래에 xml 파일을 만들어준다
<img width="312" alt="image" src="https://github.com/chae52/Auto-Comment/assets/41178045/75e9ff76-47c7-407c-af61-1f3e3b6517ac">

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd" >
<properties>
<entry key="rankingListSQL">
select
..
..
</entry>
</properties>
```
사용할 쿼리를 넣고 알맞은 key 로 지정한다.
주의할 점은 xml이 `<` 해당 부호를 처리할 떄 ,`&lt;`로 처리해야 한다는 점이다.

```sql
current_date &lt;= m.date
```
위의 예시는 current_date <= m.date 와 같은 의미이다. 

## Spring Boot 3 수정
```Java
import org.springframework.beans.factory.annotation.Value;
@PropertySource("classpath:sql.xml")
public class MemberJdbcWebRepository implements MemberWebRepository {
  @Value("${rankingListSQL}")
  String rankingList;
  @Override
  public List<MyRankingInfoDto> findRankingInfoList(){
      List<MyRankingInfoDto> certRankingList = template.query(rankingList, rankingRowMapper());
      return certRankingList;
  }
}
```
### @PropertySource
사용할 클래스에 annotation을 붙혀준다. `classpath:` 는 src/resources/를 의미한다.  
그래서 아까 resources 아래에 `sql.xml`` 이라는 파일을 만들었으니 해당 파일 이름만 적어주었다.

### @Value 
사용하려는 Method 안에 넣으면 오류가 난다.  
꼭 밖에서 annotation을 붙혀주고, 아까 xml 파일에서 지정했던 key 값을 넣어준다.

### Reference
- [Spring JDBC를 사용할 때의 SQL 관리](https://madplay.github.io/post/sql-management-when-using-spring-jdbc#google_vignette)
- [[iBatis] iBatis 매핑구문에서 비교문(<>) 사용하기](http://theeye.pe.kr/archives/455)
- [[spring] Annotation 내 classpath의 기본경로](https://kkambi.tistory.com/14)