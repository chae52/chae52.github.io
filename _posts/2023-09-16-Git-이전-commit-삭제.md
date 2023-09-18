---
title: "[Git] commit 삭제하기"
excerpt: "Github 상에서의 commit log까지 삭제하는 방법"

categories:
  - Git
tags:
  - [Git, permission, denied, access, unable, key, ssh, token]

permalink: /Programming/Git/[Git]-이전-commit-삭제/

toc: true
toc_sticky: true

date: 2023-09-16
last_modified_at: 2023-09-16
---
# 문제 상황
commit한 이후에 올라가지 말아야할 내용이 들어갔거나 아예 commit log 자체를 삭제하고 싶은 경우에 쓸 수 있다.

# 방법
## 1. reset
```shell
git reset --hard HEAD~1
```
- hard와 soft 가 있는데 hard를 사용하면, 로컬과 origin에서도 모두 사라진다
  soft는 stage 영역에 남는다.
- HEAD~n은 n개 뒤의 commit까지 삭제한다는 의미이다.  
  그러므로 위의 코드는 한 commit만 삭제한다
```shell
git reset {commit id}
```
특정 commit까지 하고 싶다면 위와 같이 commit id를 활용해도 된다

## 2. origin에 push
```shell
git push --force origin {repository 명}
```
강제로 올려줘야 올라간다.


### Reference
- [[Git] Commit 취소 및 되돌리기 - reset & revert](https://tekken5953.tistory.com/4#article-1--reset)
- [[git] 이전 commit으로 돌아가기](https://medium.com/@kwoncharles/git-%EC%9D%B4%EC%A0%84-commit%EC%9C%BC%EB%A1%9C-%EB%8F%8C%EC%95%84%EA%B0%80%EA%B8%B0-cf6caed43ed5)