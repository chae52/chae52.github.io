---
title: "[CI/CD] YAML 파일 ParserException 에러 해결"
excerpt: "yaml 문법으로 인한 도커 에러 해결"

categories:
  - ETC
tags:
  - [yaml, github, action, parser, exception, 에러, block]

permalink: /Programming/ETC/[ETC]-YAML-Parser-Exception/

toc: true
toc_sticky: true

date: 2023-10-03
last_modified_at: 2023-10-03
---
# 문제 상황
로그인 로직을 추가하며 application-oauth.yml 파일을 추가적으로 Github Action에 넣는 코드를 추가했고
`application-oauth.yml` 자체를 secret으로 넣었다.

하지만

<img width="1050" alt="무제" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/8ec4abfc-c962-4d0a-a624-44021b9e2806">


``` shell
org.yaml.snakeyaml.parser.ParserException: while parsing a block mapping
expected <block end>, but found '<scalar>'
```

과 같은 에러가 나며 실행되지 않았다

# 시도한 것
- build.gradle에 yaml 신 버전으로 추가
  ``` shell
  implementation 'org.yaml:snakeyaml:2.0'
  ```
- 주석 뺴보기
- chat GPT에게 문법 정리해달라고 요청하기
- / 를 \로 바꿔보기
등등 ..   

baseUrl을 넣는 부분에서 문제가 있어서 {} 대신 직접 넣을까도 생각했지만  
BaseUrl과 action이 Spring에서 자동 대입을 해주는 것 같고, 로그인 하는 서비스에 따라 registrationId는 변경되기에 불가능 했다.

또한, `application-oauth.yml` 을 spring project에 넣은 후에 각각을 secret으로 불러오는 방법도 생각했었다

# 해결방법
``` yaml
"{baseUrl}/{action}/oauth2/code/{registrationId}"
```
위의 코드를 아래로 바꿨다.
``` yaml
'{baseUrl}/{action}/oauth2/code/{registrationId}'
```
즉 " 를 '로 바꾸니 해결되었다..

<img width="199" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/b8c20901-5fa0-4e7d-9e55-54658d10f5de">