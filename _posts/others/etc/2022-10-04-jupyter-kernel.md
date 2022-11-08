---
title: "Jupyter Notebook 가상환경 커널 추가 및 삭제하기"
categories: etc
tags: jupyter
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

conda를 통해 생성한 Python 가상환경(venv)을 주피터 노트북 커널로 추가하거나 삭제하는 방법을 알아보자.

<br>

## 가상환경 생성하기

conda 명령으로 venv라는 이름의 Python 3.8 버전을 사용하는 가상환경을 생성한다.

```bash
conda create -n venv python=3.8
```

<br>

## 가상환경 활성화하기

위에서 생성한 가상환경인 venv를 활성화한다.

```bash
conda activate venv
```

<br>

## ipykernel 설치하기

`ipykernel`을 설치한다.

```bash
pip install ipykernel
```

<br>

## 커널 추가하기

위에서 설치한 `ipykernel`을 이용해 가상환경 venv를 주피터 노트북의 커널로 추가한다.

```bash
python -m ipykernel install --user --name=venv
```

<br>

## 커널 목록 확인하기

아래 명령을 통해 주피터 노트북에 추가된 커널 목록을 확인할 수 있다.

```bash
jupyter kernelspec list
```

<br>

## 커널 삭제하기

주피터 노트북에 추가된 커널인 venv를 삭제하려면 아래 명령어를 실행하면 된다.

```bash
jupyter kernelspec uninstall venv
```