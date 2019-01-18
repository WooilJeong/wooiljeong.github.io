---
title: "퀀들(Quandl) 데이터 수집 파이썬 튜토리얼"
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


## 파이썬 라이브러리 임포트하기


```python
# 시각화 툴
%matplotlib inline
import matplotlib.pyplot as plt

plt.rcParams["figure.figsize"] = (14,4)
plt.rcParams['axes.grid'] = True

import pandas as pd
#import pandas_datareader as pdr
import quandl
quandl.ApiConfig.api_key="your_key"
```

## 국제 금 가격

퀀들에서 국제 금 가격 데이터를 불러오기 위해 퀀들에서 "gold"를 검색합니다. ```London Bullion Market Association``` 영역에서 ```LBMA/GOLD```이 퀀들 API에 사용할 ```ticker```값 입니다. 아래 코드와 같이 ```df_gold```에 퀀들 API를 이용해 ```2007-01-01``` 부터 ```2019-01-16``` 까지의 국제 금 가격 데이터를 할당합니다.



```python
df_gold = quandl.get("LBMA/GOLD", trim_start="2007-01-01", trim_end="2019-01-16")
```

이제 ```df_gold```에 할당된 국제 금 가격 데이터의 양 극단 값을 살펴보겠습니다.


```python
df_gold.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>USD (AM)</th>
      <th>USD (PM)</th>
      <th>GBP (AM)</th>
      <th>GBP (PM)</th>
      <th>EURO (AM)</th>
      <th>EURO (PM)</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2007-01-02</th>
      <td>640.75</td>
      <td>639.75</td>
      <td>324.710</td>
      <td>324.499</td>
      <td>482.347</td>
      <td>481.957</td>
    </tr>
    <tr>
      <th>2007-01-03</th>
      <td>636.75</td>
      <td>642.60</td>
      <td>324.277</td>
      <td>329.386</td>
      <td>481.511</td>
      <td>487.483</td>
    </tr>
    <tr>
      <th>2007-01-04</th>
      <td>625.25</td>
      <td>628.70</td>
      <td>321.796</td>
      <td>323.339</td>
      <td>477.035</td>
      <td>480.180</td>
    </tr>
    <tr>
      <th>2007-01-05</th>
      <td>626.00</td>
      <td>609.50</td>
      <td>322.464</td>
      <td>315.574</td>
      <td>477.936</td>
      <td>468.558</td>
    </tr>
    <tr>
      <th>2007-01-08</th>
      <td>608.30</td>
      <td>609.50</td>
      <td>314.904</td>
      <td>314.711</td>
      <td>467.492</td>
      <td>468.018</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_gold.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>USD (AM)</th>
      <th>USD (PM)</th>
      <th>GBP (AM)</th>
      <th>GBP (PM)</th>
      <th>EURO (AM)</th>
      <th>EURO (PM)</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-10</th>
      <td>1292.40</td>
      <td>1291.90</td>
      <td>1012.98</td>
      <td>1012.46</td>
      <td>1121.54</td>
      <td>1120.85</td>
    </tr>
    <tr>
      <th>2019-01-11</th>
      <td>1292.80</td>
      <td>1288.95</td>
      <td>1012.91</td>
      <td>1007.57</td>
      <td>1121.89</td>
      <td>1123.96</td>
    </tr>
    <tr>
      <th>2019-01-14</th>
      <td>1293.70</td>
      <td>1292.75</td>
      <td>1007.02</td>
      <td>1006.15</td>
      <td>1129.27</td>
      <td>1127.41</td>
    </tr>
    <tr>
      <th>2019-01-15</th>
      <td>1289.35</td>
      <td>1294.40</td>
      <td>1002.99</td>
      <td>1008.60</td>
      <td>1127.67</td>
      <td>1130.53</td>
    </tr>
    <tr>
      <th>2019-01-16</th>
      <td>1290.50</td>
      <td>1292.30</td>
      <td>1002.46</td>
      <td>1002.95</td>
      <td>1130.99</td>
      <td>1133.86</td>
    </tr>
  </tbody>
</table>
</div>



데이터의 컬럼 중 ```USD (AM)```의 시간에 따른 국제 금 가격 그래프를 그려보겠습니다.


```python
df_gold["USD (AM)"].plot(color="gold")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1c2f667b0f0>




![png](assets/img/post_img/2019-01-18-data_collecting_tutorial/output_9_1.png)


위와 같이 얻은 국제 금 가격 데이터를 ```CSV```파일로 저장하는 코드는 다음과 같습니다.


```python
df_gold.to_csv("<저장할 경로>/gold_070101_190116.csv", sep=',', index = True)
```

국제 금 가격 데이터를 수집, 시각화 및 저장한 방법을 그대로 응용하면 다음과 같이 국제 은 가격, 국제 구리 가격 및 국제 원유 가격 데이터 역시 쉽게 수집할 수 있습니다.

# 국제 은 가격


```python
df_silver = quandl.get("LBMA/SILVER", trim_start="2007-01-01", trim_end="2019-01-16")
df_silver["USD"].plot(color="silver")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1c2f693a390>




![png](assets/img/post_img/2019-01-18-data_collecting_tutorial/output_14_1.png)


# 국제 구리 가격



```python
df_copper = quandl.get("CHRIS/CME_HG10", trim_start="2007-01-01", trim_end="2019-01-16")
df_copper["Settle"].plot(color="darkred")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1c2f6a89c88>




![png](assets/img/post_img/2019-01-18-data_collecting_tutorial/output_16_1.png)


# 국제 원유 가격


```python
df_oil = quandl.get("OPEC/ORB", trim_start="2007-01-01", trim_end="2019-01-16")
df_oil.plot(color="black")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1c2f6b8cbe0>




![png](assets/img/post_img/2019-01-18-data_collecting_tutorial/output_18_1.png)


지금까지 퀀들(Quandl) API를 통해 금융 데이터 수집, 시각화 및 저장하는 과정을 소개해드렸습니다. 궁금하신 점이 있으시면 자유롭게 글로 남겨주시면 감사하겠습니다.
