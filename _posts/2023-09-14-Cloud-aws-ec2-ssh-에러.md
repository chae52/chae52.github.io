---
title: "[AWS] MAC에서 EC2 ubuntu ssh 'Add correct host key' 에러 해결"
excerpt: "EC2 ssh key 에러 해결 : config, known_hosts"

categories:
  - Cloud
tags:
  - [AWS, EC2, ssh, key, known,hosts, add, correct]

permalink: /Programming/Cloud/EC2-SSH-add-host-error/

toc: true
toc_sticky: true

date: 2023-09-14
last_modified_at: 2023-09-14
---
# 문제 발단
ec2에 연결된 vpc를 바꾸기 위해 기존 ec2를 삭제하고 새로 팠다.  
키를 기존 ec2꺼를 재활용했더니 아래와 같은 에러가 발생했다.

# 해결 시도 1
<img width="565" alt="스크린샷 2023-09-14 오전 12 45 38" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/835c0a1f-a1e4-4bf6-8805-309db811d1ef">
> Add correct host key in ~/.ssh/known_hosts to get rid of this message.  
가 주요 문제인 것 같았다.  

전에 이런 비슷한 상황에서 chmod로 권한 변경으로 해결한 적이 있어서 시도했지만 실패했었다.  
그래서 [팀원 블로그](https://da-y-0522.tistory.com/88)를 보고 config 파일을 수정했지만 되지 않았다.


# 해결 시도 2
<img width="565" alt="스크린샷 2023-09-14 오전 12 51 41" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/002fcf17-a2ed-4c01-85ff-76b0c78cbe4c">
> Add correct host key in /Users//.ssh/known_hosts to get rid of this message.
  Offending ECDSA key in /Users//.ssh/known_hosts:30

라는 또 다른 에러가 떴다.

[이 글](https://serverfault.com/questions/321167/add-correct-host-key-in-known-hosts-multiple-ssh-host-keys-per-hostname)을 보고 따라했다.  

```bash
ssh-keyscan -t rsa {ec2 public IPv4}
```
<img width="565" alt="스크린샷 2023-09-14 오전 12 58 37" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/11a6b2d0-e74d-4c7f-8c71-aafce179ee7b">
로 키를 만들고, ~/.

```bash
vim known_hosts
```
로 들어가서 현재 접속하려는 IP({ec2 public IPv4}) 로 시작하는 모든 키를 지우고

```
{ec2 public IPv4} ssh-rsa {위에서 ssh-keyscan으로 알아낸 암호}
```

만 두었더니 잘 접속된다!!