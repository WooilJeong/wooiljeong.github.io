---
title: "도커(Docker)로 MongoDB 서버 구축하기"
categories: Server
tags: Docker
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 도커(Docker)로 MongoDB 서버 구축하기

## Docker 기본 명령어

**옵션**  

옵션 | 내용
-----|-----
-d | detached mode
-p | 포트 포워딩 HOST:CONTAINER
-v | 디렉토리 마운팅 HOST:CONTAINER
-e | 컨테이너에서 사용할 환경변수 설정
-name | 컨테이너 이름
-rm | 컨테이너 제거
-rmi | 이미지 제거
-it | 터미널 입력

<br>

- 이미지 Pull
```bash
docker pull [이미지 이름]
```
- 이미지 확인
```bash
docker images
```

- 컨테이너 bash 접속
```bash
docker exec -it [컨테이너 이름] bash
```
> -it: interactive terminal 모드

- 컨테이너 무중단으로 빠져나오기

CTRL + P, Q

- 실행중인 컨테이너 확인
```bash
docker ps
```

- 종료된 컨테이너까지 확인
```bash
docker ps -a
```

- 컨테이너 삭제
```bash
docker rm [컨테이너 ID]
```

- 이미지 삭제
```bash
docker rmi [이미지 ID]
```

<br>

## MongoDB 서버 구축

### Dcker Hub에서 mongo 도커 이미지 pull 받기

- 터미널을 실행하여 도커 허브에서 mongo 이미지를 pull

```bash
docker pull mongo
```

<br>

### mongodb 컨테이너 생성

- 'docker-compose.yml' 파일 생성
- yml 파일 내부의 `사용할 이미지` - `image`는 도커 허브에서 pull 받은 mongo 이미지 ID를 입력
  - 터미널에서 `docker images` 입력하여 image id를 입력
- 컨테이너 이름, 계정, 비밀번호 옵션도 임의로 지정
- 마운트할 디렉토리도 임의 지정

```yml
# 파일 규격 버전
version: "3"
# 이 항목 밑에 실행하려는 컨테이너 들을 정의
services: 
  # 서비스 명
  mongodb:
    # 사용할 이미지
    image: mongo
    # 컨테이너 실행 시 재시작
    restart: always
    # 컨테이너 이름 설정
    container_name: jason-mongo
    # 접근 포트 설정 (컨테이너 외부:컨테이너 내부)
    ports:
      - "27017:27017"
    # -e 옵션
    environment: 
      # MongoDB 계정 및 패스워드 설정 옵션
      MONGO_INITDB_ROOT_USERNAME: jason
      MONGO_INITDB_ROOT_PASSWORD: password
    volumes:
      # -v 옵션 (다렉토리 마운트 설정)
      - /data/mongodb/data/db:/data/db
```

- yml 파일을 생성한 디렉토리에서 docker-compose 명령어 실행

```bash
docker-compose up -d
```

<br>

*도커 컨테이너 빌드 명령어를 실행했지만, 컨테이너가 실행되지 않고 exit되는 오류가 있는 경우에는 다음 명령어를 통해 로그를 확인해 볼 수 있다.

```bash
# docker container build 오류 로그 확인
docker logs: [컨테이너 명]
```

<br>

### 컨테이너 bash 접속

```bash
docker exec -it jason-mongo bash
```
> it: interactive terminal 모드

<br>

### DB Server 외부 접속 테스트

- Robo3T GUI 환경에서 접속

```
IP: public IP 혹은 localhost
PORT: 27017
USER: jason
PASSWORD: password
```
