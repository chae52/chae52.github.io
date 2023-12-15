---
title: "[즐거운 자바] 문자(char) 타입"
excerpt: "char, 유니코드, 문자->정수 변환"

categories:
  - Java
tags:
  - [char, 문자, 타입, 자바, java]

permalink: /Programming/Java/char-type/

toc: true
toc_sticky: true

date: 2023-12-15
last_modified_at: 2023-12-15
---

# 문자 타입
> "a" = 문자열 = string
  'a' = 문자 = char

- 2byte
- 유니코드 값을 가진다
  - 유니코드란? 0000 ~ 0FFF 2byte로 표현한 문자 체계


# 문자 타입은 정수타입이기도 하다
- 97이라는 숫자를 소문자 영어 a로 대입해둔 것이다 
- 문자 타입은 0 ~ 65535까지 저장할 수 있는 정수타입이기도 하다

## 문자 -> 정수, 정수 -> 문자 변환
```java
char c1= 'a';
System.out.println((int)c1);

char c2= (char)65;
System.out.println(c2);
```
97  
A  
가 출력된다.

## 'a' ~ 'z'까지 출력하기
```java
char c1= 'a';
while (c1<='z'){
    System.out.println(c1);
    c1++;
}
```  
문자는 숫자이기도 하기에 증가시킬 수 있다.