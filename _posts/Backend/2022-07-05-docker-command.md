---
title: "도커(Docker) 명령어 / 설치 셋팅"

categories:
  - Backend
tags:
  - [Docker]

toc: true
toc_sticky: true
---


# 명령어

## 도커 내부로 진입하기
1. ```docker ps -a``` 으로 진입하려고 하는 컨테이너의 ID를 확인한 후 복사한다
2. ```docker exec -it 컨테이너ID /bin/bash``` 를 입력해서 컨테이너에 진입한다
이후에는 일반 터미널 명령어로 동작
3. 컨테이너를 종료할 때는 ```exit``` 으로 빠져나오면 된다


---
---

# 설치 셋팅

## 도커 내부에 vim 설치 (vi로 파일 확인/수정 가능하게)
1. Dockerfile에 코드 두줄을 추가한다.

```javascript
RUN apt-get update 
RUN apt-get install -y vim

RUN yarn install //여기 윗쪽에 추가
```

vim을 설치하기전에 ```apt-get update``` 로 업데이트를 먼저 진행하고,  
```apt-get install -y vim``` 으로 설치한다. (가운데에 **-y** 를 붙어줘야 자동으로 설치 허가 단계를 진행한다)
