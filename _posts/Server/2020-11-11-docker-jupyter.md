---
title: "Dockerfile로 주피터 서버 구축하기"
categories: Server
tags: Docker
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# Dockerfile로 주피터 서버 구축하기

Dockerfile을 작성하면 쉽게 주피터 서버를 도커 환경에 구축하여 배포할 수 있다. Dockerfile에는 Ubuntu 18.04 도커 이미지를 바탕으로 아나콘다 배포판 설치 과정과 몇 가지 주피터 서버 설정 내용이 스크립트로 작성되어 있다. 

<br>

## 도커파일(Dockerfile) 생성

Dockerfile 이라는 파일을 하나 생성한 다음 아래와 같이 주피터 서버 구축 과정을 스크립트로 작성하여 저장하면 된다. 주피터 서버 설정 내용 중 패스워드가 암호화되어 있는데, 이는 숫자 1230을 암호화한 것이다.

다음을 실행하면 암호화된 패스워드 문자열을 구할 수 있다.

```bash
from notebook.auth import passwd
passwd(algorithm="sha1")
```

```
sha1:70b72fef724c:2b1518d4d7db2512f7832e4ba5041bedad68726d
```

```bash
#===========================================
# 주피터 서버 도커 파일
#===========================================
# 우분투OS 이미지
FROM ubuntu:18.04

# /home 디렉토리
WORKDIR /home
RUN mkdir /soft
RUN mkdir /workspace
RUN apt-get update
RUN apt-get install -y net-tools wget nano lsof

# Anaconda 디렉토리
WORKDIR /home/soft
RUN wget https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-x86_64.sh
RUN bash Anaconda3-2020.07-Linux-x86_64.sh -b -p /home/soft/anaconda
RUN rm Anaconda3-2020.07-Linux-x86_64.sh
ENV PATH /home/soft/anaconda/bin:$PATH

WORKDIR /root/.jupyter
RUN jupyter notebook --generate-config

# 주피터 서버 설정
RUN echo "c.NotebookApp.password = u'sha1:70b72fef724c:2b1518d4d7db2512f7832e4ba5041bedad68726d'" >> /root/.jupyter/jupyter_notebook_config.py
RUN echo "c.NotebookApp.ip = '0.0.0.0'" >> /root/.jupyter/jupyter_notebook_config.py
RUN echo "c.NotebookApp.notebook_dir = '/home/workspace'" >> /root/.jupyter/jupyter_notebook_config.py
RUN echo "c.NotebookApp.allow_root = True" >> /root/.jupyter/jupyter_notebook_config.py
RUN echo "c.NotebookApp.open_browser = False" >> /root/.jupyter/jupyter_notebook_config.py
```

<br>

## 도커 이미지 빌드하기

Dockerfile이 위치한 경로에서 다음 명령어를 실행하면 도커 이미지가 빌드된다. --tag 옵션 뒤에는 생성할 이미지의 이름과 일종의 버전을 의미하는 tag를 지정한다. 명령어 마지막에 있는 점(.)은 현재 디렉토리를 의미한다.

```bash
docker build --tag jupyter:0.1 .
```

<br>

## 도커 컨테이너 생성하기

--name 옵션 뒤에는 생성할 컨테이너의 이름을 입력하고, -p 옵션 뒤에는 포트포워딩을 설정한다. -v 옵션으로는 호스트와 컨테이너의 디렉토리를 마운트할 수 있다. 명령어 마지막에 있는 값(7cdbf14e584f)은 위에서 생성한 도커 이미지의 ID값을 의미한다. docker images 명령어로 이미지 ID를 확인할 수 있다.

```bash
docker run -it -d --name jason-jupyter -p 8888:8888 -v D:/docker/jupyter/workspace:/home/workspace 7cdbf14e584f
```

<br>

## 도커 컨테이너 실행하기

도커 컨테이너에 터미널 모드로 접속한다.

```bash
docker exec -it jason-jupyter bash
```

<br>

## 주피터 서버 실행하기

nohup &를 이용해 주피터 서버를 백그라운드 모드로 실행한다.

```bash
nohup jupyter notebook &
```

<br>

## 주피터 서버 로그 확인하기

다음 명령어로 주피터 서버 로그를 확인할 수 있다.

```bash
tail -f nohup.out
```

<br>

## 프로세스 Kill하기

실행 중인 주피터 서버를 종료하기 위해서는 lsof 명령어로 PID값을 확인할 수 있고, kill 명령어로 실행중인 주피터 서버 프로세스를 종료할 수 있다.

```bash
lsof -i :8888
```

```bash
kill -9 [PID 번호]
```

<br>

## 도커 이미지 Docker Hub에 배포하기

다음 명령어를 통해 Docker Hub에 주피터 서버 이미지를 배포할 수 있다.

```bash
docker login
docker tag [이미지 ID] [Docker Hub ID]/[이미지 이름]:[태그]
docker push [Docker Hub ID]/[이미지 이름]
```