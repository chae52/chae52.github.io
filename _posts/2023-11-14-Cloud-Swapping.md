---
title: "[Ubuntu] Swapping으로 디스크를 메모리로 관리하기"
excerpt: "메모리 사용률 넘치지 않게 예방하기"

categories:
  - Cloud
tags:
  - [linux, swap, file, space, partition, 스왑, 디스크, 메모리, 사용률,swapping]

permalink: /Cloud/swapping/

toc: true
toc_sticky: true

date: 2023-11-14
last_modified_at: 2023-11-14
---
# 배경 상황
서버가 터진 이후로 메모리가 부족할 때를 대응하기 위해서 여러 방법을 강구하고 있었다.

## CHAT GPT 답변
'''
Amazon EC2 서버의 메모리 관리는 성능 및 안정성을 유지하기 위해 중요한 부분입니다. 아래는 EC2 서버에서 메모리를 관리하는 몇 가지 기본적인 방법입니다:

1. **모니터링 및 알림 설정:**
   - Amazon CloudWatch나 다른 모니터링 도구를 사용하여 EC2 인스턴스의 메모리 사용률을 지속적으로 모니터링하세요.
   - 메모리 부족 상태에 대한 알림을 설정하여 문제를 빠르게 감지하고 조치할 수 있도록 합니다.

2. **자원 최적화를 위한 인스턴스 유형 선택:**
   - EC2 인스턴스 유형을 선택할 때 애플리케이션의 메모리 요구 사항에 맞는 인스턴스 유형을 고려하세요. 다양한 유형의 인스턴스가 다양한 메모리 용량을 제공합니다.

3. **인스턴스 리사이징:**
   - 메모리 부족 현상이 지속되는 경우, 인스턴스를 리사이징하여 더 많은 메모리를 가진 인스턴스로 변경할 수 있습니다.

4. **Swap Space 활용:**
   - Swap space는 메모리 부족 시에 디스크 공간을 일시적으로 메모리로 사용하는 용도로 활용됩니다. 그러나 Swap을 사용하는 것은 성능에 영향을 미칠 수 있으므로 주의가 필요합니다.

5. **JVM 메모리 관리:**
   - Java 애플리케이션을 실행할 경우, JVM의 힙 크기 및 기타 메모리 관련 설정을 최적화하세요. `-Xmx` 및 `-Xms`와 같은 JVM 옵션을 사용하여 힙 크기를 조절할 수 있습니다.

6. **불필요한 프로세스 정리:**
   - 불필요한 백그라운드 프로세스나 서비스를 중지하고, 부하를 줄일 수 있는 방법을 찾아 최적화하세요.

7. **고가용성 및 Auto Scaling 구성:**
   - 고가용성을 유지하고, Auto Scaling을 활용하여 메모리 요구 사항에 따라 EC2 인스턴스 수를 동적으로 조절할 수 있도록 구성하세요.

8. **적절한 파일 시스템 및 캐시 설정:**
   - 파일 시스템 및 캐시 설정을 최적화하여 메모리 사용을 효율적으로 관리하세요.

9. **보안 및 업데이트:**
   - OS 및 관련 패키지를 최신 상태로 유지하고, 보안 업데이트를 정기적으로 적용하세요.

메모리 관리는 애플리케이션의 특성에 따라 다를 수 있으므로, 실제로 발생하는 문제 및 요구 사항에 따라 조치를 취해야 합니다. 실시간으로 모니터링하고 필요할 때 적절한 대응을 하는 것이 중요합니다.
'''

그래서 4번에 대한 하드 디스크의 일부를 RAM으로 사용하는 swapping 적용해보기로 했다.  

# Swapping 이란?
리눅스는 RAM을 페이지라는 단위로 나눈다. 스와핑(Swapping)은 메모리 페이지를 하드 디스크의 미리 구성된 공간인 스왑 공간에 복사하여 해당 메모리 페이지를 확보하는 프로세스이다.  
그러니까 스와핑을 통해 하드디스크 공간을 확보해두고 필요할 때 메모리로 사용할 수 있다.  

하지만 단점도 있다. 메모리는 nano second 단위이지만 디스크는 millisecond 단위이기 때문에 속도차이가 크다. 그래서 스와핑이 일어나면 시스템이 느려질 수 있다. 그래서 극단적으로 애플리케이션을 실행하면서 스와핑을 해야해서 더 느려질 수도 있다.  

리눅스는 스왑 공간이 없어도 돌아가지만 메모리가 없으면 시스템이 돌아가지 않기 때문에 상대적으로 더 싼 디스크를 스왑 공간으로 지정해두는 것이 좋다.  

>  얼마나 공간을 확보해야 될까?
  Linux에서는 데스크탑에서는 메모리의 2배를 추천하고, 서버는 메모리의 1/2배를 추천한다. 서버의 경우 스왑된 공간을 얼마나 사용하는지 모니터링 해서 필요하다면 RAM을 더 추가하는 것이 나을 수 있다.  

  ubuntu 에서는 1GB의 이하의 메모리를 사용중이라면 메모리와 같은 양으로 확보하는 것을 추천한다.
  1GB 이상으로 쓰고 있다면 최소 같은 양으로 사용하고, 최대로는 2배까지 추천한다.  

# Swap Space의 두 가지 형태
swap space를 만드는 방법은 2가지가 있다.  

1. Swap partition
하드 디스크에서 오로지 스왑을 위해서 독립적으로 분리해둔 파티션 공간이다. 다른 파일은 적재될 수 없다.  
[하드 드라이브의 파티션을 사용하여 Amazon EC2 인스턴스에서 스왑 공간으로 사용할 메모리를 할당하려면 어떻게 해야 하나요?](https://repost.aws/ko/knowledge-center/ec2-memory-partition-hard-drive)를 보면 스왑 파티션을 사용하는 방법을 볼 수 있다.
2. Swap file
파일 시스템에서도 특별한 파일이다. swap partition과 비슷하지만 swap space를 위한 공간을 repartiton할 필요가 없어서 좋다.  
[스왑 파일을 사용하여 Amazon EC2 인스턴스에서 스왑 공간으로 사용할 메모리를 할당하는 방법은 무엇입니까?](https://repost.aws/ko/knowledge-center/ec2-memory-swap-file)를 보면 스왑 파일을 사용하는 방법을 볼 수 있다.
  
그래서 swap file 방식을 선택하기로 했다.

# Swap File 로 스와핑 적용하기
## 서버 스펙 확인하기
<img width="554" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/0c180193-05e7-4035-b078-7659412a8148">  
현재 사용하고 있는 서버는 `t2.micro`로 메모리는 1GB이다.  
위의 내용에 따라 1GB를 스왑 공간으로 사용할 것이다.  

## 스왑 파일 만들기
```shell
sudo dd if=/dev/zero of=/swapfile bs=128M count=8
```
128MB * 8 = 1024MB = 1GB
<img width="572" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/73176ba1-f6ec-4e2f-b1c9-fcc78edc7066">

## 권한 업데이트 및 스왑 영역 설정
여기에서 부터는 [이 글](https://repost.aws/ko/knowledge-center/ec2-memory-swap-file)을 따라가면 된다.
```
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo swapon -s
```
<img width="572" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/7f600041-fd4b-4108-b751-61ae96860593">

## 부팅 시 스왑 파일 시작하도록
```
sudo vi /etc/fstab
```
<img width="572" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/745ec52f-321e-4898-854a-f7829dde7c1c">  
여기에서 vi 편집기를 사용해서  
<img width="572" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/e2693f4a-dacd-4afc-8b30-0889bcded281">

```
/swapfile swap swap defaults 0 0
```
를 추가해준다

# 전 후 비교
<img width="572" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/d947d9d0-5b59-401b-a655-888fb302d1e7">  
전에는 swap 0 이었다.  

<img width="572" alt="image" src="https://github.com/Sejong-Kaggle-Challengers-2nd/chaewon/assets/41178045/e68308ee-8136-4c08-a9d4-3cf23671df45">  
위의 과정을 거치니 1GB가 스왑 공간으로 적용되었다!

### Reference
- [All about Linux swap space](https://www.linux.com/news/all-about-linux-swap-space/)
- https://repost.aws/ko/knowledge-center/ec2-memory-swap-file 
- [How much swap do I need?](https://help.ubuntu.com/community/SwapFaq#How_much_swap_do_I_need.3F)