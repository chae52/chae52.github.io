---
title: "[Sentry-알림"
excerpt: "Sentry-알림"

categories:
  - Spring
tags:
  - [Sentry,알림]

permalink: /Programming/Spring/Sentry-알림/

toc: true
toc_sticky: true

date: 2023-09-11
last_modified_at: 2023-09-11
---
# 적용하기
[Sentry Spring Boot Docs](https://docs.sentry.io/platforms/java/guides/spring-boot/)
docs가 2가지가 있는데 위에 있는 독스가 더 정확했다  

[Logging Framework Integrations](https://docs.sentry.io/platforms/java/guides/spring-boot/logging-frameworks/)


# 알림
## Integration
Sentry상에서 지원하는 슬랙 통합 알림은 developer plan에서는 불가하다  
<img width="1119" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/09d7a0c2-4cf1-4a8c-a88d-3b00c0e0e7b0">  
[Sentry Pricing](https://sentry.io/pricing/)에 들어가면 다른 항목도 확인할 수 있다

[Slack Notification Docs](https://docs.sentry.io/product/integrations/notification-incidents/slack/?original_referrer=https%3A%2F%2Fsentry.io%2F&utm_campaign=Google_Search_Brand_ROW_Beta&utm_content=g&utm_id=%7B20066198574%7D&utm_medium=cpc&utm_source=google&utm_term=sentry#linking-your-slack-and-sentry-accounts)

[slack 연동 docs](https://develop.sentry.dev/integrations/slack/)
## Webhook
<img width="958" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/80795bc7-0ad8-4b37-ab33-2d4240cbca0c">


<img width="958" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/d5e2fcc9-72f7-4acd-9cf8-ca87dbb14dde">

해당 페이지로 슬랙에서의 webhook url등 필요한 것들을 받아볼 수 있었다  
특정 채널과 연동도 했다.

<img width="659" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/573792bc-9fda-4dbc-930b-23a43e077016">

설정에서 webhook넣는 란도 찾았다.  
<img width="1265" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/51e3f44a-e271-469e-9ee3-c0f07a3b3231">  
[들어갈 수 있는 주소](https://savable.sentry.io/settings/developer-settings/slackcustomintegration-5a096b/)

## 설정
<img width="1487" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/e49d82ab-e436-4149-8fe3-b891ea0bf47b">

[설정주소](https://sentry.io/settings/account/notifications/)

## 알림 주기
이전에는 [[Sentry] 설정 및 spring boot application 연동](https://blog.naver.com/PostView.naver?blogId=cutesboy3&logNo=221933485820&parentCategoryNo=1&categoryNo=&viewDate=&isShowPopularPosts=true&from=search)을 참고하면  
<img width="486" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/e25e44db-e68a-4292-b595-c3fe8bf99078">  

간격에서의 발생횟수를 확인했던 것 같다

[Sentry 공식 블로그의 알림 수 글](https://blog.sentry.io/proactive-alert-rules/)


하지만 최근 docs를 확인하면
["When" Conditions: Triggers](https://docs.sentry.io/product/alerts/create-alerts/issue-alert-config/)  
<img width="763" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/0423955f-d7e7-4169-b35d-40768d5ca5f4">  

몇 분에 몇 번 이상 발생시로 바뀌었다

[Alerts](https://docs.sentry.io/product/alerts/notifications/notification-settings/?original_referrer=https%3A%2F%2Fwww.google.com%2F#alerts)에 이메일과 슬랙 등 알림에 대한 모든 것이 잘 정리되어 있었다

<img width="768" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/03baa06d-8e66-4b75-9556-ac74bf42750b">




## Email 알림
그래서 그냥 이메일로 받기로 했다
<img width="779" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/e8a37ade-3adc-4541-821f-32a33aa6108d">  
[Suggested Assignees](https://docs.sentry.io/product/alerts/create-alerts/issue-alert-config/#then-conditions-actions)


# Github action
[Sentry Release](https://github.com/marketplace/actions/sentry-release?version=v1.4.1#create-a-sentry-internal-integration)라는 github도 참고하면 할 수 있겠지만 나는 그냥 application.yml 파일 전체를  
secret key로 넣었다
[sentry github action app](https://github.com/getsentry/sentry-github-actions-app/blob/main/.github/workflows/ci.yml)의 yml파일도 살펴봤었다

[Configuration Docs](https://develop.sentry.dev/config/)

# 분석 및 추가 기능
## User Feedback

<img width="254" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/449b64ed-3aeb-4385-85a9-2be6b3f272c9">
왼쪽 사이드바에 User Feedback 메뉴가 따로 있다
<img width="1091" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/0cc4714e-3a62-48ba-8ce8-27d5833a5b02">
어떠한 설정을 해야지만 보이는 것 같다

[User feedback 관련 docs](https://docs.sentry.io/api/projects/submit-user-feedback/)가 있지만 구현하진 못했다

[https://skyseven73.tistory.com/16](https://skyseven73.tistory.com/16)

## Crash Free Sessions/Users
[Crash Free Sessions/Users](https://docs.sentry.io/product/releases/health/#session)

## Environment
[Environment Docs](https://docs.sentry.io/product/alerts/create-alerts/issue-alert-config/#environment)



### Reference
- [[JPA] @ManyToMany, 다대다[N:M] 관계](https://ict-nroo.tistory.com/127)
- [JPA 연관 관계 한방에 정리 (단방향/양방향, 연관 관계의 주인, 일대일, 다대일, 일대다, 다대다)](https://jeong-pro.tistory.com/231)