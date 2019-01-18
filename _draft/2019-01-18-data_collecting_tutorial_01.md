---
title: "퀀들(Quandl) 데이터 수집 파이썬 튜토리얼 01"
date: 2019-01-18 20:00:00 -0400
categories: Tutorial
tags: Python Quandl
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
---

## 퀀들(Quandl) API를 이용한 금융 데이터 수집 튜토리얼

이번 튜토리얼에서는 파이썬(Python) 환경에서 ```퀀들(Quandl) API```를 이용해 다양한 금융 데이터를 수집하는 방법에 대해 다뤄보겠습니다. 데이터를 수집한 뒤에는 간단한 시각화도 진행해보겠습니다. 끝으로 수집한 데이터를 CSV파일 형태로 저장하는 방법에 대해서도 소개해드리겠습니다.


## 퀀들(Quandl)이란?

퀀들(Quandl)은 캐나다 토론토의 데이터 공유 플랫폼 회사입니다. 특히 금융 데이터를 퀀들이 제공하는 API를 통해 손쉽게 제공받아 분석에 활용할 수 있습니다. 퀀들 API는 [퀀들](http://https://www.quandl.com/) 웹 사이트에 접속 후 가입 절차를 완료한 후 ```api_key```를 발급받아 이용할 수 있습니다. 그럼 이제 퀀들 API를 이용하여 파이썬에서 금융 데이터를 수집해보도록 하겠습니다.


##
