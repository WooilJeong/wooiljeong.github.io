---
title: "macOS에 딥러닝을 위한 도커 환경 구축하기"
categories: Server
tags: Docker
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# macOS에 딥러닝을 위한 도커 환경 구축하기

![PNG](/assets/img/post_img/2019-12-27-docker_mac/img_logo.png){: .align-center height="300px" width="300px"}

![PNG](/assets/img/post_img/2019-12-27-docker_mac/img_tf.png){: .align-center height="300px" width="300px"}

딥러닝과 같은 데이터 분석 환경을 구축하려면 파이썬, 라이브러리 등의 버전을 확인해가며 설치해야하고, 주피터 노트북과 같은 에디터에 대한 설정을 해야합니다. 이렇게 매번 새로 환경을 구축하는 번거로움을 줄이기 위해서 Mac에 도커를 설치해봤습니다. Docker는 컨테이너 기반의 가상화 시스템이라고 합니다. 즉, 데이터 분석 환경이 미리 구축된 도커 이미지를 사용해 번거로운 환경 구축 작업을 최소화하는데 목적이 있습니다.

<br>

## Environment

- OS : macOS Mojave 10.14.6 ver

<br>

## 도커 설치하기

![PNG](/assets/img/post_img/2019-12-27-docker_mac/img_01.png){: .align-center}

[도커 홈페이지](https://www.docker.com)에 접속해 우측 상단의 `Get Started` 버튼을 누릅니다.

<br>

![PNG](/assets/img/post_img/2019-12-27-docker_mac/img_02.png){: .align-center}

중앙의 `Download Desktop and Take a Tutorial` 버튼을 누릅니다. 이후 로그인이 필요하므로 회원가입 후 작업을 진행합니다.

<br>

![PNG](/assets/img/post_img/2019-12-27-docker_mac/img_03.png){: .align-center}

중앙의 `Download Docker Desktop for Mac` 버튼을 누릅니다.

<br>

![PNG](/assets/img/post_img/2019-12-27-docker_mac/img_04.png){: .align-center}

다운로드된 도커 설치 파일을 실행하고, `Docker` 아이콘을 `Application` 디렉토리에 드래그 앤 드롭합니다.


<br>

![PNG](/assets/img/post_img/2019-12-27-docker_mac/img_05.png){: .align-center}

도커를 최초 실행하면 약간의 시간이 소요됩니다.

<br>

![PNG](/assets/img/post_img/2019-12-27-docker_mac/img_06.png){: .align-center}

터미널 실행 후 `docker run hello-world` 명령어를 입력해서 `hello from Docker!`가 출력되면 도커 설치가 완료된 것입니다.

<br>

## 도커 컨테이너 만들고 접속하기

```bash
docker pull ubuntu
```
도커 컨테이너를 만들기 위해 리눅스 OS 중에서 Ubuntu 이미지를 받아오겠습니다.

<br>

```bash
docker images
```
저장된 Ubuntu 도커 이미지를 목록에서 확인합니다. 도커 컨테이너 목록은 `docker ps`로 확인합니다.

<br>

```bash
docker run -t -i -p 8888:8888 ubuntu
```
`run` 명령어를 통해 도커 컨테이너를 `생성 - 실행 - 접속` 합니다.

-t : tty를 활성화하여 bash 쉘을 사용  
-i : 상호 입출력
-p 8888:8888 : 포트 설정

<br>

## 컨테이너에 주피터 노트북 설치하기

### 패키지 설치

```bash
apt-get update
apt-get install -y sudo net-tools nano wget build-essential
```
패키지들을 설치합니다.

### 패스워드 설정

```bash
passwd root
```
루트 비밀번호 : root

### 사용자 계정 생성

```bash
adduser ubuntu
```
ubuntu 비밀번호 : ubuntu

```bash
nano /etc/sudoers
```
`# User privilege specification` 항목의 root계정 아래에 다음을 입력합니다.

```
ubuntu    ALL=(ALL:ALL) ALL
```

입력 후 `CTRL + X` - `Y` - `ENTER` 를 통해 빠져나옵니다.

```bash
su - ubuntu
```
ubuntu 계정으로 접속합니다.


## 도커에 주피터 서버 구축하기

### 아나콘다 배포판 다운로드
```bash
wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh
```

도커 컨테이너에 아나콘다 배포판을 설치합니다.  
(**2019.10 python 3.7 for linux** 버전을 기준으로 합니다.)

<br>

### 아나콘다 배포판 설치

```bash
bash Anaconda3-2019.10-Linux-x86_64.sh
```

license term 에 동의(yes) 후 설치를 완료합니다.

<br>

### 아나콘다 path 설정

```bash
nano ~/.bashrc
```

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_14.png){: .align-center}

```
export PATH=/home/ubuntu/anaconda3/bin:$PATH
```
위 내용을 가장 하단부에 입력 후  
`ctrl`+`x`입력 - `y`입력 - `Enter`입력 하여 빠져나옵니다.

```bash
source .bashrc
conda info --envs
```
콘다 정보가 정상적으로 출력된다면 아나콘다 배포판 설치가 완료된 것입니다.

### 주피터 서버 구축

```bash
jupyter notebook --generate-config
```

`.jupyter`디렉토리와 `.jupyter/jupyter_notebook_config.py`설쩡 파일이 생성됩니다.


```bash
cd .jupyter
ipython
```
디렉토리 이동 후 `ipython`을 실행합니다.

```bash
from notebook.auth import passwd
passwd()
```

패스워드에 `jupyter`를 입력하고, 결과로 나온 암호를 복사해둡니다.

```
'sha1:0b94bc192464:c83224b882342aa109f2e78ffe9096cae0ac38e6'
```

```bash
exit()
```

`ipython`을 빠져나옵니다.


```bash
nano jupyter_notebook_config.py
```
아래와 같이 설정할 파라미터를 찾고 주석(#)을 제거한 뒤 설정값을 입력합니다. 설정을 마친 후 nano를 빠져나옵니다. 단, ip주소는 `ifconfig` 명령을 틍해 확인합니다.
```
c.NotebookApp.password = '복사한 sha1 암호'
c.NotebookApp.ip = '도커 컨테이너 IP 주소'
c.NotebookApp.notebook_dir = '/home/ubuntu/project'
c.NotebookApp.allow_root = True
c.NotebookApp.open_browser = False
```

<br>

```bash
cd ~
mkdir project
jupyter notebook
```

호스트에서 `localhost:8888`로 접속하면 주피터 노트북이 활성화됩니다.

### 주피터 익스텐션 설치

```bash
conda install -c conda-forge jupyter_contrib_nbextensions
```
주피터 노트북 확장 프로그램을 설치합니다.

### Tensorflow 설치

```bash
conda install tensorflow
```
텐서플로우를 설치합니다. (현재 2.0 버전)

<br>

## 도커 내부 쉘에서 나가기

도커 내부 쉘에서 나가는 방법은 두 가지가 있습니다. 도커 컨테이너를 종료하고 나가는 방법과 컨테이너를 종료하지 않고 나가는 방법이 있습니다.

### 컨테이너 종료 후 나가기

- `exit`
- `CRTL` + `D`

### 컨테이너 종료하지 않고 나가기

- `CTRL` + `P`, `Q`
- 재접속 시 `docker attach "컨테이너 ID"`혹은 "컨테이너 NAMES"

<br>

## 도커 이미지 생성하기

```bash
docker commit <옵션> <컨테이너 이름> <이미지 이름>:<태그>
```

-a : 이미지를 작성한 작성자 이름을 지정함
-m : 이미지 생성과 관련된 로그 메세지를 지정함

```bash
docker commit -a "Wooil Jeong <wooil@kakao.com>" -m "Jupyter Server" great_swartz jupyter:0.1
```
도커 이미지 `jupyter:0.1`를 생성했습니다.

<br>

## 도커 이미지 배포하기

```bash
docker login
```
도커에 로그인을 합니다.

```bash
docker tag [push할 image ID or name] [docker hub ID]/[image name]:[version]
```

```bash
docker tag 038c7f815911 mcwooil/jupyter:0.1
docker push mcwooil/jupyter
```

<br>

## net-tools 설치 없이 외부에서 도커 컨테이너 IP주소 확인하기

```bash
docker inspect "컨테이너 ID" 혹은 "컨테이너 NAMES" | grep IPAddress
```

```bash
# 실행 결과

"SecondaryIPAddresses": null,
"IPAddress": "172.17.0.2",
"IPAddress": "172.17.0.2",
```

`docker ps` 명령으로 컨테이너 ID를 확인한 후 IP주소를 찾으면 됩니다.
