---
title: "[즐거운 자바] 객체지향 문법"
excerpt: "객체, 인스턴스, 레퍼런스 변수"

categories:
  - Java
tags:
  - [자바, java, 객체지향, 객체, 인스턴스, 레퍼런스, 변수]

permalink: /Programming/Java/object-oriented/

toc: true
toc_sticky: true

date: 2023-12-19
last_modified_at: 2023-12-19
---
# 객체지향 프로그래밍
= Object-Oriented Programming = OOP
- 컴퓨터 프로그래밍의 패러다임 중 하나
- 객체들의 모임으로 컴퓨터 프로그램을 파악하고자 한다.

# 클래스 : Class
- 설계도면과 흡사
- 구성요소
  - 필드 : field
  - 메소드 : method

# 오브젝트 : Object
- 설계도면대로 만든 실제 물체

```java
Book b = new Book();
// Book : 레퍼런스 타입
// b : 참조형 변수
// new Book() : 인스턴스 생성 부분
// Book() : 생성자
```

# 인스턴스 : Instance
- 설계도면대로 만든 실제 물체

# 참조형 변수 : Reference Variable
> 기본형 타입 : int, short, byte, long ...  
- 인스턴스를 사용하려면 특별한 이름으로 참조해야 한다
- 그래서 참조형 변수를 선언한다
- 참조되지 않으면 사용할 수 없어서 쓰레기가 된다.
- 객체를 가리킨다(=참조한다)
- 자바에는 포인터처럼 주소로 가리키지 않는다.