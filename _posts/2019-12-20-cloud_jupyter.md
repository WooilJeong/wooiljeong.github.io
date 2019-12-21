---
title: "AWS EC2 Ubuntu에 주피터 서버 구축하기"
categories: Server
tags: Jupyter
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# AWS EC2 Ubuntu에 주피터 서버 구축하기

AWS EC2 우분투 프리티어 인스턴스에 주피터 서버를 구축하여, 언제 어디서나 접속가능한 데이터 분석 환경을 만들어보겠습니다.


## Environment

- OS : macOS Mojave 10.14.6 ver


## AWS 프리티어 인스턴스 생성

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_01.png){: .align-center}

[AWS Console](https://console.aws.amazon.com) 에 접속 및 로그인을 합니다.

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_02.png){: .align-center}

인스턴스를 눌러줍니다.

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_03.png){: .align-center}

인스턴스 시작 버튼을 눌러줍니다.

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_04.png){: .align-center}

프리티어 사용가능한 인스턴스 중 Ubuntu 18.04를 선택합니다.

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_05.png){: .align-center}

검토 및 시작 버튼을 눌러줍니다.

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_06.png){: .align-center}

시작하기 버튼을 눌러줍니다.

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_07.png){: .align-center}

새 키 페어 생성을 선택하고, 키 페어 이름을 입력합니다.


<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_08.png){: .align-center}

키 페어 다운로드 버튼을 눌러줍니다. 우분투 인스턴스에 접속 시 필히 필요하므로 안전한 곳에 저장하여 유실되지 않도록 주의합니다.

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_09.png){: .align-center}

다운로드 받은 키 페어 파일입니다.

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_10.png){: .align-center}

인스턴스 시작 버튼을 눌러줍니다.

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_11.png){: .align-center}

인스턴스 보기 버튼을 눌러줍니다.

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_12.png){: .align-center}

하단 부분에 인스턴스의 퍼블릭 IP가 나타납니다.

<br>



## SSH 접속

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_13.png){: .align-center}

```bash
sudo chmod 600 jupyter.pem
ssh -i jupyter.pem ubuntu@퍼블릭ip주소
```

터미널 실행 후 위 명령어를 입력하여 생성한 우분투 인스턴스에 접속합니다.  

최초 접속 시 종종 Permission 관련 에러가 발생하기도 합니다. 따라서 `sudo chmod 600 jupyter.pem`을 통해 키페어의 권한을 올려준 뒤 접속을 시도한 것입니다.

<br>


## 아나콘다 배포판 설치

우분투 EC2에 접속한 상태에서 아나콘다 배포판을 설치합니다.  
(**2019.10 python 3.7 for linux** 버전을 기준으로 합니다.)

```bash
wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh
```

## 아나콘다 설치

```bash
sudo bash Anaconda3-2019.10-Linux-x86_64.sh
```

## 아나콘다 path 설정

nano ~/.bashrc
export PATH=/home/ubuntu/anaconda3/bin:$PATH
source .bashrc
conda list


## 주피터 서버 구축
https://wooiljeong.github.io/etc/coding_on_ipad_01/


## 콘다 가상환경 생성
conda create -n wooil python=3.7 anaconda

에러시 다음 코드 실행
sudo chown -R username /path/to/anaconda3

## 주피터 익스텐션 설치
conda install -c conda-forge jupyter_contrib_nbextensions

## autopep8 설치
pip install autopep8
