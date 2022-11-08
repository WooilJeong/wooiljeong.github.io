---
title: "Python Captcha 인식 모델 만들기"
categories: python
tags: CaptchaCracker
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

> # CaptchaCracker 보안문자 인식 모델 만들기

CaptchaCracker는 Captcha Image 인식을 위한 딥 러닝 모델 생성 기능과 적용 기능을 제공하는 오픈소스 파이썬 라이브러리입니다. 아래와 같은 Captcha Image의 숫자를 인식해 숫자 문자열을 출력하는 딥 러닝 모델을 만들거나 모델을 직접 사용해볼 수 있습니다.

[GitHub Repository - CaptchaCracker](https://github.com/WooilJeong/CaptchaCracker)

<br>

### **입력 이미지**

![https://github.com/WooilJeong/CaptchaCracker/raw/main/assets/example01.png](https://github.com/WooilJeong/CaptchaCracker/raw/main/assets/example01.png)

### **출력 문자열**

```bash
023062
```

<br>

## **설치**

```bash
pip install CaptchaCracker
```

<br>

## **의존성**

```bash
pip install numpy==1.19.5 tensorflow==2.5.0
```

<br>

## **예제**

### **모델 학습 및 저장하기**

모델 학습 실행에 앞서 아래와 같이 파일명에 Captcha 이미지의 실제값이 표기된 학습 데이터 이미지 파일들이 준비되어 있어야 합니다.

- [샘플 데이터 다운로드](https://github.com/WooilJeong/CaptchaCracker/raw/main/sample.zip)

![https://github.com/WooilJeong/CaptchaCracker/raw/main/assets/example02.png](https://github.com/WooilJeong/CaptchaCracker/raw/main/assets/example02.png)

다음과 같이 파이썬 내장 모듈인 glob과 위에서 설치한 CaptchaCracker 라이브러리를 불러옵니다.

```python
import glob
import CaptchaCracker as cc
```

<br>

위 `샘플 데이터 다운로드` 링크를 통해 다운받은 이미지 파일 경로를 다음과 같이 지정하여 학습 이미지 파일 경로 리스트를 만들어줍니다.

```python
# 학습 이미지 데이터 경로
train_img_path_list = glob.glob("../data/train_numbers_only/*.png")
```

<br>

Captcha 이미지의 너비와 높이를 지정해줍니다.

```python
# 학습 이미지 데이터 크기
img_width = 200
img_height = 50
```

<br>

`CreateModel` 메서드의 인자로 학습 이미지 리스트, 이미지 너비 그리고 이미지 높이를 입력하여 `CM`이라는 모델 생성 인스턴스를 만들어줍니다.

```python
# 모델 생성 인스턴스
CM = cc.CreateModel(train_img_path_list, img_width, img_height)
```

<br>

`CM`의 `train_model`메서드의 인자로 `epochs`를 설정하여 모델 학습을 진행합니다.

```python
# 모델 학습
model = CM.train_model(epochs=100)
```

<br>

모델 학습이 끝나면 `model`의 `save_weights` 메서드의 인자로 가중치를 저장할 파일의 경로를 입력합니다.

```python
# 모델이 학습한 가중치 파일로 저장
model.save_weights("../model/weights.h5")
```

<br>

### **저장된 모델 불러와서 예측하기**

모델 적용의 경우도 학습 시와 마찬가지로 `CaptchaCracker` 라이브러리를 불러옵니다.

```python
import CaptchaCracker as cc
```

<br>

학습한 이미지 스펙과 동일한 타겟 이미지를 준비하고, 이미지 너비와 높이를 다음과 같이 지정합니다.

```python
# 타겟 이미지 크기
img_width = 200
img_height = 50
```

<br>

Captcha 이미지의 라벨의 자릿수와 라벨 각 자리가 가질 수 있는 숫자의 종류를 다음과 같이 지정해줍니다. (*학습 하지 않는 라벨 구성요소는 예측할 수 없습니다. 예를 들어, 학습 이미지들의 라벨에 숫자 )

```python
# 타겟 이미지 라벨 길이
max_length = 6
# 타겟 이미지 라벨 구성요소
characters = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'}
```

<br>

가중치 파일 경로를 지정합니다.

```python
# 모델 가중치 파일 경로
weights_path = "../model/weights.h5"
```

<br>

`ApplyModel` 메서드의 인자로 가중치 파일 경로, 이미지 너비, 이미지 높이, 라벨 길이 그리고 라벨 구성요소를 입력하여 `AM` 인스턴스를 만들어줍니다.

```python
# 모델 적용 인스턴스
AM = cc.ApplyModel(weights_path, img_width, img_height, max_length, characters)
```

<br>

예측 작업을 수행하기 위해 타겟 이미지의 경로를 입력합니다. `AM`의 `predict` 메서드를 호출하여 타겟 이미지의 라벨값을 예측합니다.

```python
# 타겟 이미지 경로
target_img_path = "../data/target.png"

# 예측값
pred = AM.predict(target_img_path)
```

<br>


## **참고**

- [GitHub Repository - CaptchaCracker](https://github.com/WooilJeong/CaptchaCracker)
- [https://keras.io/examples/vision/captcha_ocr/](https://keras.io/examples/vision/captcha_ocr/)