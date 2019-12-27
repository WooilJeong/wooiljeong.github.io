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

## Docker 설치

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
