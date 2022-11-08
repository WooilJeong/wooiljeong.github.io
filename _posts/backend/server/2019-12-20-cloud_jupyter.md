---
title: "AWS EC2 Ubuntu에 주피터 서버 구축하기"
categories: server
tags: Jupyter
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# AWS EC2 Ubuntu에 주피터 서버 구축하기

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_01/img_logo.PNG){: .align-center height="300px" width="300px"}

AWS EC2 우분투 프리티어 인스턴스에 주피터 서버를 구축하여, 언제 어디서나 접속가능한 데이터 분석 환경을 만들어보겠습니다.

<br>

## Environment

- OS : macOS Mojave 10.14.6 ver

<br>

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

## 아나콘다 배포판 다운로드

```bash
wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh
```

우분투 EC2에 접속한 상태에서 아나콘다 배포판을 설치합니다.  
(**2019.10 python 3.7 for linux** 버전을 기준으로 합니다.)

<br>

## 아나콘다 배포판 설치

```bash
sudo bash Anaconda3-2019.10-Linux-x86_64.sh
```

license term 에 동의(yes) 후 설치를 완료합니다.

<br>

## 아나콘다 path 설정

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

```bash
sudo chown -R ubuntu:ubuntu anaconda3
```
ubuntu 유저에게 `anaconda3` 디렉토리에 대한 권한을 넘겨줍니다.

<br>

## 가상환경 생성

```bash
conda create -n venv python=3.7 anaconda
```
가상환경 `venv`를 생성합니다.

```bash
source activate venv
```
가상환경 `venv`를 활성화합니다.

<br>

## 주피터 서버 구축

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

패스워드를 입력하고, 결과로 나온 암호를 복사해둡니다.

```
'sha1:390d90....'
```

```bash
exit()
```

`ipython`을 빠져나옵니다.


```bash
nano jupyter_notebook_config.py
```
아래와 같이 설정할 파라미터를 찾고 주석(#)을 제거한 뒤 설정값을 입력합니다. 설정을 마친 후 nano를 빠져나옵니다. 단, ip는 퍼블릭이 아닌 프라이빗 ip를 입력합니다.

```
c.NotebookApp.password = '복사한 sha1 암호'
c.NotebookApp.ip = 'EC2 Private IP Address'
c.NotebookApp.port = '8888'
c.NotebookApp.notebook_dir = '/home/ubuntu/project'
```

<br>

## AWS EC2 포트 개방

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_15.png){: .align-center}

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_16.png){: .align-center}

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_17.png){: .align-center}

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_18.png){: .align-center}

<br>

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_19.png){: .align-center}

<br>


## 주피터 서버 실행

```bash
cd ~
mkdir project
jupyter notebook
```

![PNG](/assets/img/post_img/2019-12-20-cloud_jupyter/img_20.png){: .align-center}

주피터 서버 구축이 완료되었습니다.
