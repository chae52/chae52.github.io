---
title: "[Tableau] RDS 데이터베이스 연결 시 보안그룹 설정"
excerpt: "보안그룹에 추가해야 하는 Tableau IP"

categories:
  - Analysis
tags:
  - [Tableau, rds, database, 연결, 보안그룹]

permalink: /Data/Analysis/Tableau-RDS-보안그룹/

toc: true
toc_sticky: true

date: 2023-09-09
last_modified_at: 2023-09-09
---
태블로에서 AWS의 Database 서비스를 사용할 수 있을지 궁금했었다  
결론은 된다.

하지만 RDS는 보안그룹으로 인바운드가 막혀있기 떄문에 어떤 IP를 열어 주어야    
태블로에서도 RDS의 데이터베이스에 접근할 수 있을지 알아보고 적용해보자


# 1. RDS region 확인
먼저 RDS의 region을 확인한다  
<img width="333" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/3004d914-eb28-468c-bcfd-6b5f3c4d5983">
northeast임을 확인할 수 있다

# 2. RDS 리전에 대한 IP 주소 확인
<img width="948" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/1566bf96-7eb8-4535-98c3-8162fbe2d8d9">
[태블로 공식 사이트](https://help.tableau.com/current/pro/desktop/ko-kr/publish_tableau_online_ip_authorization.htm)에 가서 확인하면 서울 region에 대한 IP 주소를 확인할 수 있다

# 3. 보안그룹에 IP 추가
<img width="1219" alt="스크린샷 2023-09-11 오후 5 45 38" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/6cc583ec-2e16-4730-9a4e-cbe03933f65d">
rds의 보안그룹으로 가서 인바운드규칙 편집을 눌러 변경할 수 있다

<img width="1151" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/cb5522bc-58e2-488e-b17c-a4e17075765f">
본인의 DB를 선택하고 아까 확인한 IP를 넣어준다. 나중에 식별할 수 있도록 별명도 넣으면 편하다

이렇게 설정을 하면 태블로에서 접근해서 사용할 수 있다!