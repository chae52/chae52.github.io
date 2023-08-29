---
title: "[Dev] Git Flow 적용. Git Flow & Github Flow"
excerpt: "Git Flow 와 Github Flow"

categories:
  - Dev
tags:
  - [Git Flow, Github Flow]

permalink: /Dev/[Dev] Git Flow 적용. Git Flow & Github Flow/

toc: true
toc_sticky: true

date: 2023-08-16
last_modified_at: 2023-08-30
---
[우아한 기술 블로그 : 우린 Git-flow를 사용하고 있어요](https://techblog.woowahan.com/2553/)를 읽고
기존의 Github flow와 Git flow를 둘 다 팀에서 사용해보고 맞는 것을 고르기로 결정하였다.
나는 Git Flow를 경험해보기로 했다.

## Git flow 한 줄 요약
작업하는 분리된 공간을 branch가 아닌 fork한 repository로 두는 방법

# Git Repository 구성
- Upstream Remote Repository = Upstream Repository
  - 모든 개발자들이 공유하는 최신 소스코드가 저장된 원격 저장소
- Origin Remote Repository = Origin Repository
  - Upstream Repository를 fork 한 개인 원격 저장소
- Local Repository 
  - Origin Repository를 Clone 등 하여 내 컴퓨터에 저장되어 있는 개인 저장소

<img width="797" alt="image" src="https://github.com/choiiis/minimal-mistakes-choiiis-customized/assets/41178045/627cd303-76e6-4d75-9f5e-de2b311de9d4">

## 작업 Flow
1. Local Repository에서 작업을 완료한다
2. Origin Repository에 Push
3. Origin Repository -> Upstream Repository 로 Pull Request
![image](https://github.com/choiiis/minimal-mistakes-choiiis-customized/assets/41178045/1511230e-bf07-4e69-a495-d081268f9672)
4. Upstream Repository로 Merge
5. Upstream Repository와 Local Repository pull(fetch)

## Rebase
읽다보니 Rebase에 대한 개념이 부족해서 따로 공부했다.
### 브랜치에서 다른 브랜츠로 합치는 2가지 방법
1. Merge
  - 안전하다
  - 쉽다
2. Rebase
  -  잘 모르고 사용하면 위험
  - 히스토리가 깔끔해진다

두 방법의 실행결과는 같지만 히스토리가 달라진다

![image](https://github.com/choiiis/minimal-mistakes-choiiis-customized/assets/41178045/c9b1be3e-9d2f-4971-a70e-288ffba529d4)
  



# 적용한 팀 방식
- 해당 글을 따라가되, 여러 커밋이 나뉘어 있는 것을 squash 하지는 않았다
  더러운 commit 로그를 정리하지 않고, 왜 commit이 정돈되어야 하는지 필요성을 직접 느끼거나
  그 정돈되지 않은 commit 로그가 우리팀의 방식인지 알아보기로 했기 때문이다
- 태그 추가도 생략했다

![image](https://github.com/choiiis/minimal-mistakes-choiiis-customized/assets/41178045/1511230e-bf07-4e69-a495-d081268f9672)
Git Flow를 사용하며 3번의 Pull Request를 넣었다.

# 느낀 점
4주간 Git flow를 사용해보며 느낀 장단점

## 장점
### 1. Upstream Repository에서 브랜치를 따서 하지 않고도 안전하다
Origin Repository의 develop을 마치 Upstream Repository에서 feature 브랜치처럼
사용할 수 있다는 것이었다
물론 위의 블로그에서 제시한 정석의 방법은 아니지만 기능을 병렬적으로 만들고 있는 상황이 아니어서
이렇게 develop을 feature의 기능처럼 사용할 수 있어서 좋았다.

### 2. 브랜치 관리
위의 1번 장점과 연결되는데, Upstream Repository만 예쁘게 유지될 수 있도록 하고
Origin Repository는 어차피 fork한 내 ID아래의 Repository이기 때문에 
높은 강도의 관리에 노력을 쏟지 않았다. 예를 들어 브랜치 삭제나 네이밍 등에 대해 조금 관대해졌다.
어차피 예쁜 Pull Request를 만든 이후에 다시 fork 해도 되니 마음이 편했다


## 단점
### 1. Local repository에서 Upstream Repository의 clone본인지 Origin Repository의 clone 본인지 헷갈린다
![image](https://github.com/choiiis/minimal-mistakes-choiiis-customized/assets/41178045/6097736e-23fc-4def-8c64-443a89536141)
```bash
git remote -v
```
위와 같은 명령어로 upstream과 origin을 알아볼 수 있긴 하다

### 2. Github Action 반복 설정
아직 겪어보지 못 했지만, Github Flow를 사용했을 때는 브랜치마다 다른 것만 Secret 을 따로 만들고
나머지 공통으로 사용하는 것은 branch를 만든다고 다시 secret을 등록할 필요는 없었다.

하지만 Git Flow를 사용한다면 Secret이 생길 때마다 Upstream Repository와 Origin Repository에
반복적으로 등록해야 한다는 것이 불편할 것으로 예상된다.

# 마무리
팀원의 Github Flow 사용기를 듣고 우리 팀에 잘 맞는 방식을 선택해야겠다!



### Reference
- 실제 필요한 git 명령어
  - [git fetch upstream](https://my-codinglog.tistory.com/7)
  - https://wonit.tistory.com/368