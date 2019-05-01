---
title: "캐글(Kaggle) 튜토리얼 00"
date: 2018-12-19 00:00:00 -0000
categories: Tutorial
tags: Kaggle
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---
## Step 0 : 캐글(Kaggle) 관련 배경지식

**1) 캐글(Kaggle)**

<center>
![Kaggle img](https://www.kaggle.com/static/images/site-logo.png)
</center>

[캐글(Kaggle)](https://www.kaggle.com/)은 2010년 설립된 예측모델 및 분석 대회 플랫폼이다. 기업 및 단체에서 데이터와 해결과제를 등록하면, 데이터 과학자들이 이를 해결하는 모델을 개발하고 경쟁한다. 2017년 3월 구글에 인수되었다.


<br>

**2) 타이타닉 대회(Titinic Competition)**

![Titinic img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fd/RMS_Titanic_3.jpg/270px-RMS_Titanic_3.jpg)

[타이타닉(Titinic) 대회](https://www.kaggle.com/c/titanic)는 캐글에 소개된 대회 중 하나이다. 데이터 사이언스 혹은 머신러닝을 처음 시작하거나 캐글 예측 대회에 대한 쉬운 이해가 필요할 때 시작하기 좋은 대회라고 소개되어 있다.

이 대회에 대해 간략히 소개하자면, 영화로도 널리 알려진 바 있는 '타이타닉 호 침몰 사건''과 관련한 데이터 분석 대회이다. 타이타닉 호는 1912년 4월 15일 빙산과 충돌해 침몰했고, 이로 인해 2224명의 승객과 승무원 중에서 무려 1502명이 사망했다.

난파 사고로 인해 여러 사람이 죽게 된 이유 중 하나는 승객과 승무원을 위한 구명정이 충분하지 않았기 때문이라고 알려져있다. 침몰에서 살아남는 데는 운의 요소가 있긴 하지만, 일부 그룹의 사람들은 여성, 어린이 및 상류층과 같은 다른 이들 보다 생존 가능성이 더 높을 수 있었다.

이 대회에서는 과연 어떤 부류의 사람들이 생존할 가능성이 더 높았는지에 대한 분석을 하도록 요구하고 있다. 특히, 머신러닝 기법을 적용해 승객이 비극에서 살아남는데 도움을 주고자 하고 있다.

이 대회의 문제를 풀기 위해 요구되는 능력으로는 크게 다음과 같은 두 가지가 있다.

- 이진 분류 (Binary Classification)
- 파이썬과 R 기초 (Python and R basics)


<iframe width="560" height="315" src="https://www.youtube.com/embed/9xoqXVjBEF8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


<br>

**3) 파이썬(Python)**

![Python img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Python_logo_and_wordmark.svg/240px-Python_logo_and_wordmark.svg.png)

파이썬(Python)은 1991년 프로그래머인 귀도 반 로섬(Guido van Rossum)이 발표한 고급 프로그래밍 언어로, 플랫폼 독립적이며 인터프리터식, 객체지향적, 동적 타이핑(dynamically typed) 대화형 언어이다. 파이썬이라는 이름은 귀도가 좋아하는 코미디 〈Monty Python's Flying Circus〉에서 따온 것이다.

타이타닉 대회에서 예측 분석 문제를 풀기 위해 필요한 능력으로 '파이썬과 R 기초'를 제시했던 것처럼, 파이썬은 실제 데이터 분석가들 사이에서 가장 널리 사용되는 프로그래밍 언어이다. 또, 앞으로 실습해 볼 여러 머신러닝 기법들 역시 파이썬 환경에서 쉽게 사용할 수 있도록 패키지화 되어 있다.


<br>

**4) 머신러닝(Machine Learning)**

기계학습 또는 머신러닝(Machine Learning)은 인공지능의 한 분야로, 컴퓨터가 학습할 수 있도록 하는 알고리즘과 기술을 개발하는 분야를 말한다. 가령, 머신러닝을 통해서 수신한 이메일이 스팸인지 아닌지를 구분할 수 있도록 학습할 수 있다.

머신러닝의 핵심은 표현(representation)과 일반화(generalization)에 있다. 표현이란 데이터의 평가이며, 일반화란 아직 알 수 없는 데이터에 대한 처리이다. 이는 전산 학습 이론 분야이기도 하다. 다양한 머신러닝의 응용이 존재한다. 문자 인식은 이를 이용한 가장 잘 알려진 사례이다.

앞으로 진행할 튜토리얼에서 여러 머신러닝 알고리즘들 중에서 의사결정나무(Decision Trees)와 랜덤 포레스트(Random Forest) 알고리즘을 이용한 예측 방법을 적용해 타이타닉 대회의 생존율 예측 문제를 간단한 수준에서 풀어 볼 것이다.
