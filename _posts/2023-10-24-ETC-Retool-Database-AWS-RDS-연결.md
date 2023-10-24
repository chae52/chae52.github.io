---
title: "[Retool] 리툴에서 AWS RDS(PostgreSQL) 연동하기"
excerpt: "Retool에서 AWS RDS 사용하기"

categories:
  - ETC
tags:
  - [rds, retool, aws, rds, connection]

permalink: /Programming/ETC/[ETC]-retool-db-aws-rds/

toc: true
toc_sticky: true

date: 2023-10-24
last_modified_at: 2023-10-24
---
인증 관리 페이지와 같은 관리자 페이를 만드는데 유용한 툴인 retool을 사용하는 방법을 정리하려 한다.  

## Retool 메인 화면
리툴 메인 화면에서 DB를 추가하는 방법이 있다.  
그렇게 하면 앱 내부에서 쿼리를 짤 때 DB를 사용할 수 있다.  
물론 앱 내에서 Create Resource 해서 추가할 수도 있는데 똑같은 것 같다. 


<img width="1376" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/1f24e3fc-1b35-41fb-a450-9c53b596fad7">  


## DB 연결
Create New를 클릭한다.  
<img width="1376" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/d2b0190a-f8d0-46b3-9386-bceca3f054bd">
<img width="493" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/8dfa1068-7e47-4323-ae74-f07482277ea2">

자신이 사용하고 있는 DB 종류를 골라준다.  

<img width="302" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/d340e299-eab4-4f22-9e0b-b9227708014a">
aws 에서 리전을 확인하여 선택해준다.  

여기는 잘 기억이 안나는데 AWS에서 IAM 만들어서 아래와 같이 등록해주면  
<img width="826" alt="스크린샷 2023-10-24 오후 1 01 53" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/a0dad7c0-b7dd-474d-9ddd-471ad5ace31d">
알아서 내 계정의 DB 인스턴스를 가져와준다.

<img width="715" alt="스크린샷 2023-10-24 오후 1 05 11" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/9b87eee1-3f7e-4d02-88a5-4d03b98b422f">  
<img width="715" alt="스크린샷 2023-10-24 오후 1 05 11" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/9b87eee1-3f7e-4d02-88a5-4d03b98b422f">  
위에는 db 이름
<img width="715" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/49c0bf76-092a-4420-8bf2-d6f73c78e2ba">
여기는 마스터 사용자 이름을 넣어준다. postgreSQL은 거의 postgres 이다
  

Authentication은 채우지 않아도 되었다.

<img width="1002" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/a987f9de-04bc-496d-bd95-e43f9c1c7708">  
Test Connection을 눌러 테스트도 통과!

<img width="391" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/c703adda-23d1-43ac-887a-35ee0e8ae783">  
등록 성공!

# 앱에서 사용

아까 등록한 DB를 사용하기 위해서 앱으로 이동한다

<img width="520" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/af8c1558-652b-4f87-b3a8-ee3695039174">

Code -> + -> Resource Query를 통해 쿼리를 생성한다

<img width="520" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/a5b622bf-95a0-4fe5-823a-a4283a14dcfd">  
여기에서 이렇게 아까 등록했던 DB를 찾아 사용하면 된다!