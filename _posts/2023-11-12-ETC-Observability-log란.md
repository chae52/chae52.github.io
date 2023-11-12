---
title: "[Observability] Log 란, Log 레벨, log & metric & trace"
excerpt: "Log를 남길 목적을 분명히 해보자"

categories:
  - ETC
tags:
  - [log, level, trace, metric,Observability]

permalink: /Programming/ETC/[LOG]-log-level-metric-trace/

toc: true
toc_sticky: true

date: 2023-11-12
last_modified_at: 2023-11-12
---
# 배경 상황
서버가 터졌다  
CPU 사용률이 100%를 찍긴 했으나, 트래픽이 절대 많지 않을 새벽 2-3시 경이었기 때문에 이유를 알 수 없었다.  
해커가 여러 번 접속을 시도했다거나 이런 기록도 없었다.  
그래서 메모리가 부족해져서일 것이다라는 추정으로 끝냈지만, 한 번 더 터졌을 때 이유를 알아야 하기 떄문에  
로그를 남기고자 한다.  

# 로그 레벨
평소에 spring boot 서버를 개발하면서 log level에 대해 자주 접했기에 이에 대한 것부터 알아보았다.
현재 우리는 Spring Boot로 서버를 구성하고 있다.  
[Spring Boot docs](https://docs.spring.io/spring-boot/docs/2.1.13.RELEASE/reference/html/boot-features-logging.html)에 따르면 내부 로깅은 [Apache commons logging](https://commons.apache.org/proper/commons-logging/guide.html#Best_Practices_General)을 따르고 있다고 했기에 이 곳을 살펴보았다.  

<img width="1205" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/cf68b23c-d611-4d15-a025-fdc0a64dabd4">  


해석해보았다  
그러고 다른 블로그들을 참고하여 정보를 추가했다.  
- fatal : 서버가 조기 종료되어버리는 에러.
  - 동작을 중단시키는 치명적인 에러
- error : 예측 못한 상황에서의 다른 runtime 에러. 
  - 오류. 심각한 예외 상황
- warn : 거의 에러인 것
  - 경고성
- info : 흥미있을 이벤트 - 시작 및 종료 같은 것. 
  - 정보성
- debug : 시스템 flow를 통한 자세한 정보
  - 내부 동작을 이해하고 싶을 때
- trace : 더 자세한 정보
  - 애플리케이션의 실행 흐름과 디버깅 정보

여기에서 debug와 trace는 직접 코드로 적어야 보이는 로그이다.

이렇게 구성되어 있으며 아래로 갈수록 더 양이 많고 중요도가 작아진다. 원하는 레벨을 적으면 그 레벨을 포함한 상위 레벨을 포함하여 로깅된다.

# 우리에게 필요한 것은?
[Logs vs Metrics vs Traces : Microsoft Code with Engineering Playbook](https://microsoft.github.io/code-with-engineering-playbook/observability/log-vs-metric-vs-trace/)를 읽어보고 현 상황에서 필요한 것이 어떤 것인지 살펴보자  
<img width="751" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/c22a77b9-eecc-40d7-b948-1960d7de6ebb">  


## Metrics
- compact size : 대규모 시스템에서도 효율적으로 수집할 수 있다.
- health data로 자주 쓰인다  
  - 자원의 현재 상태를 reporting 하는데 자주 쓰인다 ex. CPU, Memory

## Logs
- 이벤트에 대한 상세 정보
  - rich data
- metric 보다 더 사이즈가 크다
-   광범위한 시스템의 상태를 알아보는데는 부적잘하다. 차라리 metric을 쓴다.  
  - metric에 의해서도 모니터링 되지만, error, warning 같이 좀 더 예외적인 상황에 대한 것임
- 이산적이다(<-> 연속적)

## Traces
- log 보다 더 넓고 연속적인 상황을 파악할 수 있게 한다
- 프로그램의 flow와 진행 상황을 파악할 수 있게 해준다
- 유저 1명의 사용 journey를 따라갈 수 있다.
- microservice 환경이나 multiple service에서 요청이 어떻게 처리되는지 알 수 있다.
- 각각 유니크한 식별자가 필요하다.
- HTTP request나 DB call, 큐로부터 받은 메세지 등이 있을 수 있다
- 어떤 함수, 몇 시, 어떤 파라미터 등에 대한 정보를 받을 수 있다

# 결론
평소에 '로그를 남긴다', '로그 추적' 같은 단어를 많이 들어서 로깅이 이벤트에 대한 기록을 남기는 것에 대한 대표단어?인줄 알았는데,  
observability라는 더 넒은 개념의 단어가 있었다.  
현재 우리 상황에서는 서버에 대한 health check가 필요하기 때문에 metric를 주기적으로 남길 필요가 있고, 
문제가 생겼을 때 log를 보고싶을 것이기 떄문에 서버가 죽기 직전의 log를 남기고 죽을 수 있게 조치가 필요해 보인다.  
또한 복잡하고 큰 서비스도 아니고, microservice도 아니기 때문에 trace까지는 용도로도 필요없고 너무 많은 데이터를 쓸모없이 저장해야할 것 같다.  


### Reference
- https://www.atatus.com/blog/logging-traces-metrics-observability/