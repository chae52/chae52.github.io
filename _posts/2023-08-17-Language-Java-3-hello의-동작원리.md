---
title: "[즐거운 자바] hello의 동작원리"
excerpt: "class, method, field, compile"

categories:
  - Language
tags:
  - [java, 즐거운 자바, inflearn, hello의 동작원리]

permalink: /Language/Java-hello의-동작원리/

toc: true
toc_sticky: true

date: 2023-08-17
last_modified_at: 2023-08-17
---
# 구성 요소
## Class
- field 와 method가 있을 수 있다
- 첫 번째 문자가 대분자면 메소드
## Method
- 함수 아님! java에서는 메소드라고 말함
ex) main method
- 소문자로 시작하고 뒤에 ()가 붙는다
### Main method
- java 프로그램이 실행되려면 무조건 main method가 있어야 한다
- 그래서 `시작점`이라고도 말한다

## Field 
- 소문자로 뒤에 ()가 없다

# System.out.println("Hello")
- System : 클래스
- out : 필드
- println("") : 메소드

[Java API 문서](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html)에 자세히 내용들이 적혀 있다

# 컴파일하기
- 컴파일 하면 .class 파일이 생성된다
- class 파일은 바이트 코드이다
  - 바이트 코드는 기계어여서 알아볼 수 없다
- java는 CPU 에 상관없이 실행되도록 되어 있음
  - 그래서 java는 기계어와 사람언어 중간 형태인 바이트 코드를 생성한다
  - JVM이 CPU별로 존재한다
### Interpreter 방식
1. 컴파일
2. 바이트 코드 만들어짐
3. JVM이 바이트 코드를 한 줄 씩 읽는다