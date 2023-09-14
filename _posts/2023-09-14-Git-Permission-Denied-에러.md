---
title: "[Git] Git Flow 적용. Git Flow & Github Flow"
excerpt: "Git Flow 와 Github Flow"

categories:
  - Git
tags:
  - [Git Flow, Github Flow]

permalink: /Programming/Git/[Git] Permission denied, unable to access 에러 /

toc: true
toc_sticky: true

date: 2023-08-16
last_modified_at: 2023-09-14
---
# 문제 상황
처음 프로젝트를 생성하고 첫 푸시를 할 때 항상 생기는 문제가 되었다.  
Git access 접근 방식이 token으로 바뀌어서 매번 새 프로젝트를 팔 때마다 반복하는 일이 될 것이다.  

# 문제 해결
```shell
remote: Permission to user/repository-name.git denied to user.
fatal: unable to access 'https://github.com/user/repository-name.git/': The requested URL returned error: 403
```
1. developer setting에서 token 생성
2. 해당 프로젝트에 token 등록

순으로 진행하면 된다

# Token 생성
## 1. Profile > Settings > Developer Settings
<img width="309" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/b87ca2b0-2339-49d1-9ca4-9cf459af73bf">
<img width="558" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/2c641535-968f-4fec-904a-bd9573221161">

다음 personal access tokens를 누른 뒤  
<img width="1150" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/45af30a0-421e-4169-8a67-2fabb281c20b">  
이 화면에서 generate token을 해준다

## 2. 자격 선택
30개 정도 되는 접근 자격을 선택해야 한다.  
이 모든 것을 read and write 와 같은 것으로 선택해야한다.  
원하는 것들을 모두 선택해준다.   
<img width="1150" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/ca6321bb-ad21-4cb0-9728-67749d512f65">
그러면 토큰이 생성된다

<img width="1150" alt="스크린샷 2023-09-14 오후 6 28 34" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/28ff430d-2a1c-4af6-bfc3-5c1cc5707a7a">
바로 나오는 이 페이지에서밖에 토큰을 볼 수 없다. 해당 페이지를 나가게 되면 토큰을 복사할 수 없다.
그래서 빠르게 복사한다.


<img width="862" alt="스크린샷 2023-09-14 오후 6 33 31" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/4117
<img width="468" alt="스크린샷 2023-09-14 오후 6 33 56" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/9311b5d4-b1a5-4eab-b872-b31544bc1db2">
8045/3078196f-579e-48f8-a448-7964f393aaff">



<img width="886" alt="스크린샷 2023-09-14 오후 6 46 43" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/46c88490-52ee-46bc-81f0-e7f6db6631d2">


<img width="468" alt="스크린샷 2023-09-14 오후 6 46 27" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/09c586d4-b7e6-4e11-8b41-e8a6317838b2">
<img width="956" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/2ae8e38e-fc18-4728-ae88-3e309f36b508">
<img width="956" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/64ae1ced-6a8d-41ad-a6bd-4a1a19114d44">
<img width="956" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/a2560001-25be-46e4-a3c4-3ae066b9aaef">


<img width="521" alt="스크린샷 2023-09-14 오후 6 32 14" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/005ff9c6-4001-41bf-9462-75a1bad86a15">


### Reference
- [Git 작업시 personal access token 에러](https://jha-memo.tistory.com/149)
- [git permission denied (publickey) 에러 해결](https://madstorage.tistory.com/236)