---
title: "[Git] Permission denied, unable to access, publickey 에러"
excerpt: "Token으로 Git config 설정시 마주치는 (모든) 에러와 과정"

categories:
  - Git
tags:
  - [Git, permission, denied, access, unable, key, ssh, token]

permalink: /Programming/Git/[Git]-Permission-denied-unable-to-access-publickey-에러/

toc: true
toc_sticky: true

date: 2023-09-14
last_modified_at: 2023-09-14
---
# 문제 상황
처음 프로젝트를 생성하고 첫 푸시를 할 때 항상 생기는 문제가 되었다.  
Git access 접근 방식이 token으로 바뀌어서 매번 새 프로젝트를 팔 때마다 반복하는 일이 될 것이다.  

# 문제 해결
작업을 하고 처음 git push를 했더니 아래의 에러가 떴다.
```shell
remote: Permission to user/repository-name.git denied to user.
fatal: unable to access 'https://github.com/user/repository-name.git/': The requested URL returned error: 403
```
1. developer setting에서 token 생성
2. 해당 프로젝트에 token 등록
3. ssh key 등록

순으로 진행으로 해결했다.

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
그러면 토큰이 생성된다. 나의 경우는 모든 자격에서 다 허락했다.  

<img width="1150" alt="스크린샷 2023-09-14 오후 6 28 34" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/28ff430d-2a1c-4af6-bfc3-5c1cc5707a7a">
바로 나오는 이 페이지에서만 토큰을 볼 수 없다. 해당 페이지를 나가게 되면 토큰을 복사할 수 없다.
그래서 빠르게 복사한다.

## 3. (실패) 키 체인 등록
맥의 경우에는 키체인에 등록하는 방법이 있다고 해서 따라해보았다.
`키체인` -> github 검색을 해서  

<img width="862" alt="스크린샷 2023-09-14 오후 6 33 31" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/3078196f-579e-48f8-a448-7964f393aaff">
위를 클릭해준다.

<img width="468" alt="스크린샷 2023-09-14 오후 6 33 56" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/9311b5d4-b1a5-4eab-b872-b31544bc1db2">  
`암호 보기`를 하면 이렇게 암호를 입력하라고 하는데 맥에 로그인 시에 사용하는 암호를 입력하면 된다.  

<img width="521" alt="스크린샷 2023-09-14 오후 6 32 14" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/0b7f17b1-2d34-4fdc-9073-a6f0e2075ef8">  
위에서 발급한 토큰을 여기에 입력하고 저장했다.  
하지만 push는 되지 않았다

## 4. origin 등록시 token 올리기

```shell
git config --global user.email
git config --global user.name 
```
로 나의 이메일과 아이디가 맞는지 확인한 후에  

```shell
git remote -v // origin 확인
git remote remove origin // origin 삭제

// 1번
git remote set-url origin https://{아까 발급받은 토큰}@github.com/{user이름이나 orginization 이름}/{repo 이름}.git
// 2번
git remote add origin https://{아까 발급받은 토큰}@github.com/{user이름이나 orginization 이름}/{repo 이름}.git
```
위의 명령어들로 origin 삭제, 확인, 등록 후 push 를 반복했다.  
1번의 등록이 맞는지 2번의 등록 둘 다 되지 않았던 것 같다.

```shell
git remote add origin git@github.com:{user이름이나 orginization 이름}/{repo 이름}.git
```
을 해주니 된 것 같았지만 또 다른 에러가 떴다.  

```shell
The authenticity of host 'github.com (---)' can't be established.
ED25519 key fingerprint is SHA256:+DiY
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

```shell
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

## 5. organization token 설정
나의 경우에는 organization에서 만든 repository여서 아래의 설정이 필요했다.  

<img width="886" alt="스크린샷 2023-09-14 오후 6 46 43" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/46c88490-52ee-46bc-81f0-e7f6db6631d2">
organization 프로필에서 세팅을 들어간다.  

<img width="468" alt="스크린샷 2023-09-14 오후 6 46 27" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/09c586d4-b7e6-4e11-8b41-e8a6317838b2">  
token 설정에 들어간다.  

<img width="956" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/2ae8e38e-fc18-4728-ae88-3e309f36b508">  
처음 설정한다면 이런 화면이 뜬다.  

<img width="956" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/64ae1ced-6a8d-41ad-a6bd-4a1a19114d44">
<img width="956" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/a2560001-25be-46e4-a3c4-3ae066b9aaef">

정리하자면 fine-grained personal access token 이든, personal access token(classic)이든 상관없이  
organization 접근을 허용한다고 설정해준다.  

## 6. ssh key gen
```shell
ssh-keygen -t rsa -C "github에서 사용하는 email"
```
이후에 뜨는 파일 저장 위치나 비밀번호는 그냥 `enter`로 지나쳐도 되고 설정해도 된다.

<img width="259" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/02e55d48-23a1-45f5-8fb0-af137c82a26e">
<img width="259" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/2f5019a6-e275-4dc9-9e98-98512d9b7272">
<img width="811" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/5732776e-6fd2-4f9d-8570-68fb76885413">


해당 메뉴를 통해 ssh key를 등록하는 메뉴로 이동한다.  

터미널에서 ssh key를 저장한 위치로 이동한 후
```shell
vim id_rsa.pub
```
를 통해 생성된 키를 확인한 후 복사하고
<img width="811" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/a8f87d1f-ef6e-4e54-86a5-8937381fdf2a">  
여기에 붙혀넣어 등록한다.

<img width="559" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/c7a079d5-bcc1-4beb-b768-393f2b39af16">
이렇게 되면 등록이 잘 된 것이다.  

이제 push를 하니 겨우 된다..

### Reference
- [Git 작업시 personal access token 에러](https://jha-memo.tistory.com/149)
- [git permission denied (publickey) 에러 해결](https://madstorage.tistory.com/236)
- [Permission denied when pushing to remote repository: Error 403](https://stackoverflow.com/questions/76554123/permission-denied-when-pushing-to-remote-repository-error-403)