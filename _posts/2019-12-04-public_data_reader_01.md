---
title: "PublicDataReader - 파이썬 공공데이터 수집 라이브러리 01"
categories: Python
tags: PublicDataReader
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 아파트매매 실거래자료 데이터 수집하기

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}

[공공 데이터 포털](https://www.data.go.kr/)에서는 2019년 12월 4일 현재 총 3,254건에 이르는 다양한 오픈 API 서비스를 제공하고 있습니다. 저도 부동산이나 상권 분석을 할 때, 오픈 API 서비스를 이용해 데이터를 수집하여 분석에 활용하고 있습니다. 다만, 서비스를 이용하려면 `Requests`, `BeautifulSoup` 등의 파이썬 라이브러리를 통해 크롤링하는 코드를 작성해야한다는 번거로움이 있습니다.  

<br>

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_01.png){: .align-center}

이런 수고로움을 줄이기 위해, 단 코드 3줄(라이브러리 불러오기, API 서비스키 등록하기, 데이터 수집하기)만으로 손쉽게 공공 데이터 포털의 데이터를 `Pandas`의 `DataFrame`형태로 가져올 수 있는 라이브러리 `PublicDataReader`를 만들어 보았습니다.  

<br>

현재는 '[국토교통부 실거래가 정보](https://www.data.go.kr/dataset/3050988/openapi.do)' 중 '**아파트매매 실거래자료**'의 데이터를 수집하는 기능까지 개발하였습니다. 추후 오픈 API 활용 신청 건수가 높은 서비스들을 대상으로 기능을 추가해나갈 예정입니다. 오픈소스 프로젝트이니 개발에 참여해주실 분들은 연락주시면 감사하겠습니다!

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_02.jpg){: .align-center}

<br>

## Python 라이브러리 PublicDataReader 설치하기

터미널에서 `pip`로 다음과 같이 `PublicDataReader` 를 설치합니다.
```bash
pip install PublicDataReader
```

`PublicDataReader`는 API기능 추가 등의 업데이트가 있을 수 있습니다. 다음과 같이 최신 버전으로 업그레이드합니다.
```
pip install PublicDataReader --upgrade
```

<br>

## PublicDataReader 임포트하기

주피터 노트북에서 `PublicDataReader`라이브러리를 불러와 **아파트매매 실거래자료**를 불러와 보겠습니다.


```python
import PublicDataReader as pdr
```

## Open API 서비스 키 입력하기

공공 데이터 포털에서 발급받은 서비스 키를 복사하여 다음과 같이 `serviceKey`에 문자열로 할당해줍니다. 오픈 API 서비스 키 발급에 관해 모르시는 분들은 구글에 '**공공 데이터 포털 Open API 사용법**'을 검색해보시면 여러 문서를 참조하실 수 있습니다. 검색 후 가장 상단에 있는 [이 블로그](https://jeong-pro.tistory.com/143)를 참조하셔도 됩니다!


```python
serviceKey = "<< OPEN API SERVICE KEY HERE >>"
```

<br>

## 서비스 키로 아파트매매 실거래 자료 수집기 만들기

다음과 같이 아파트매매 실거래 자료를 수집할 `Apt` 수집기를 만들어줍니다. 위에서 정의한 `serviceKey`를 다음과 같이 입력해줍니다.


```python
Apt = pdr.AptTransactionReader(serviceKey)
```

<br>

## 주소 검색을 통해 지역코드 검색하기

아파트매매 실거래 자료 Open API 기술 문서에 따르면 조회하고자 하는 지역의 코드는 본래 법정동 코드 10자리 중 앞 5자리로 한다고 정의되어 있습니다. 이 부분도 직접 크롤링을 할 때 번거로운 부분이었는데, 지역명을 문자열로 입력받아 지역코드를 반환하는 메소드를 만들었습니다. 즉, 조회하고자 하는 지역의 이름을 검색하고, 출력된 '법정구코드'를 아래 데이터 수집 시 활용하시면 됩니다.


```python
df_code = Apt.CodeFinder("분당구")
df_code.head()
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
      <th>법정동명</th>
      <th>법정구코드</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>경기도 성남시 분당구</td>
      <td>41135</td>
    </tr>
    <tr>
      <th>1</th>
      <td>경기도 성남시 분당구 분당동</td>
      <td>41135</td>
    </tr>
    <tr>
      <th>2</th>
      <td>경기도 성남시 분당구 수내동</td>
      <td>41135</td>
    </tr>
    <tr>
      <th>3</th>
      <td>경기도 성남시 분당구 정자동</td>
      <td>41135</td>
    </tr>
    <tr>
      <th>4</th>
      <td>경기도 성남시 분당구 율동</td>
      <td>41135</td>
    </tr>
  </tbody>
</table>
</div>

<br>

## 특정 월의 아파트매매 실거래 데이터 불러오기

다음과 같이 `Apt.DataReader()`에 `지역코드`와 `계약월`을 입력해줍니다. 결과는 `Pandas`의 `DataFrame` 형태로 반환됩니다.


```python
df = Apt.DataReader("41135", "201911")
df.head()
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
      <th>지역코드</th>
      <th>법정동</th>
      <th>거래일</th>
      <th>아파트</th>
      <th>지번</th>
      <th>전용면적</th>
      <th>층</th>
      <th>건축년도</th>
      <th>거래금액</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2019-11-01</td>
      <td>무지개(1단지)(대림)</td>
      <td>211</td>
      <td>47.52</td>
      <td>14</td>
      <td>1995</td>
      <td>39000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2019-11-01</td>
      <td>무지개(3단지)(건영)</td>
      <td>201</td>
      <td>53.83</td>
      <td>7</td>
      <td>1996</td>
      <td>47000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2019-11-01</td>
      <td>까치마을(1단지)(대우롯데선경)</td>
      <td>77</td>
      <td>84.79</td>
      <td>21</td>
      <td>1995</td>
      <td>94650</td>
    </tr>
    <tr>
      <th>3</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2019-11-02</td>
      <td>무지개(5단지)(청구)</td>
      <td>221</td>
      <td>85</td>
      <td>3</td>
      <td>1995</td>
      <td>67900</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2019-11-02</td>
      <td>까치마을(2단지)(주공)</td>
      <td>88</td>
      <td>49.36</td>
      <td>1</td>
      <td>1995</td>
      <td>46000</td>
    </tr>
  </tbody>
</table>
</div>

<br>

## 특정 기간 동안의 아파트매매 실거래 데이터 불러오기

특정 월 뿐 아니라 `Apt.DataCollector()`를 사용하면, 특정 기간 동안의 데이터를 한 번에 수집할 수 있습니다. `지역코드`, `시작월`, `종료월` 순으로 입력하면 됩니다. 위의 `Apt.DataReader()`와 같이 결과는 `Pandas`의 `DataFrame` 형태로 반환됩니다.


```python
df_sum = Apt.DataCollector("41135", "2019-01", "2019-11")
df_sum.head()
```

    >>> LAWD_CD : 41135 DEAL_YMD : 201901
    >>> LAWD_CD : 41135 DEAL_YMD : 201902
    >>> LAWD_CD : 41135 DEAL_YMD : 201903
    >>> LAWD_CD : 41135 DEAL_YMD : 201904
    >>> LAWD_CD : 41135 DEAL_YMD : 201905
    >>> LAWD_CD : 41135 DEAL_YMD : 201906
    >>> LAWD_CD : 41135 DEAL_YMD : 201907
    >>> LAWD_CD : 41135 DEAL_YMD : 201908
    >>> LAWD_CD : 41135 DEAL_YMD : 201909
    >>> LAWD_CD : 41135 DEAL_YMD : 201910
    >>> LAWD_CD : 41135 DEAL_YMD : 201911





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
      <th>지역코드</th>
      <th>법정동</th>
      <th>거래일</th>
      <th>아파트</th>
      <th>지번</th>
      <th>전용면적</th>
      <th>층</th>
      <th>건축년도</th>
      <th>거래금액</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2019-01-02</td>
      <td>무지개(5단지)(청구)</td>
      <td>221</td>
      <td>58.49</td>
      <td>7</td>
      <td>1995</td>
      <td>48500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2019-01-12</td>
      <td>까치마을(3단지)(신원)</td>
      <td>66</td>
      <td>130.83</td>
      <td>13</td>
      <td>1995</td>
      <td>96500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2019-01-14</td>
      <td>무지개(4단지)(주공)</td>
      <td>220</td>
      <td>59.98</td>
      <td>24</td>
      <td>1995</td>
      <td>50000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2019-01-16</td>
      <td>무지개(5단지)(청구)</td>
      <td>221</td>
      <td>85</td>
      <td>4</td>
      <td>1995</td>
      <td>65000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2019-01-19</td>
      <td>까치마을(1단지)(대우롯데선경)</td>
      <td>77</td>
      <td>51.32</td>
      <td>3</td>
      <td>1995</td>
      <td>47500</td>
    </tr>
  </tbody>
</table>
</div>

<br>

## (부가 기능) 간단한 요약 통계량 확인하기

수집된 데이터를 동 별로 집계할 수 있는 기능이 있습니다. 법정동 별 중앙값, 평균값, 최솟값, 최댓값, 표준편차, 거래량를 확인할 수 있습니다. `Apt.Agg()`에 수집한 데이터 `df`를 입력하면 결과를 `Pandas`의 `DataFrame`형태로 반환합니다.

<br>

### 특정 월(2019년 11월) 분당구 법정동 별 요약 통계량


```python
df_agg = Apt.Agg(df)
df_agg
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
      <th>법정동</th>
      <th>중앙값</th>
      <th>평균값</th>
      <th>최솟값</th>
      <th>최댓값</th>
      <th>표준편차</th>
      <th>거래량</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>구미동</td>
      <td>61000</td>
      <td>65285</td>
      <td>39000</td>
      <td>120000</td>
      <td>22422</td>
      <td>23</td>
    </tr>
    <tr>
      <th>1</th>
      <td>금곡동</td>
      <td>80750</td>
      <td>74834</td>
      <td>41950</td>
      <td>100700</td>
      <td>18532</td>
      <td>16</td>
    </tr>
    <tr>
      <th>2</th>
      <td>백현동</td>
      <td>169000</td>
      <td>159071</td>
      <td>129500</td>
      <td>185000</td>
      <td>24249</td>
      <td>7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>분당동</td>
      <td>69500</td>
      <td>70945</td>
      <td>55000</td>
      <td>85000</td>
      <td>9340</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>삼평동</td>
      <td>114700</td>
      <td>138585</td>
      <td>91500</td>
      <td>203000</td>
      <td>38030</td>
      <td>13</td>
    </tr>
    <tr>
      <th>5</th>
      <td>서현동</td>
      <td>75900</td>
      <td>76634</td>
      <td>47000</td>
      <td>106000</td>
      <td>18291</td>
      <td>16</td>
    </tr>
    <tr>
      <th>6</th>
      <td>수내동</td>
      <td>102500</td>
      <td>94690</td>
      <td>41700</td>
      <td>162000</td>
      <td>35587</td>
      <td>20</td>
    </tr>
    <tr>
      <th>7</th>
      <td>야탑동</td>
      <td>56000</td>
      <td>56838</td>
      <td>34000</td>
      <td>104500</td>
      <td>20116</td>
      <td>16</td>
    </tr>
    <tr>
      <th>8</th>
      <td>운중동</td>
      <td>82000</td>
      <td>79395</td>
      <td>57643</td>
      <td>101500</td>
      <td>12228</td>
      <td>15</td>
    </tr>
    <tr>
      <th>9</th>
      <td>이매동</td>
      <td>88500</td>
      <td>87250</td>
      <td>65500</td>
      <td>111000</td>
      <td>19385</td>
      <td>6</td>
    </tr>
    <tr>
      <th>10</th>
      <td>정자동</td>
      <td>94500</td>
      <td>94957</td>
      <td>37000</td>
      <td>169000</td>
      <td>41433</td>
      <td>38</td>
    </tr>
    <tr>
      <th>11</th>
      <td>판교동</td>
      <td>94000</td>
      <td>84772</td>
      <td>60860</td>
      <td>102000</td>
      <td>17475</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>

<br>

### 특정 기간(2019년 1월 ~ 2019년 11월) 분당구 법정동 별 요약 통계량


```python
df_sum_agg = Apt.Agg(df_sum)
df_sum_agg
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
      <th>법정동</th>
      <th>중앙값</th>
      <th>평균값</th>
      <th>최솟값</th>
      <th>최댓값</th>
      <th>표준편차</th>
      <th>거래량</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>구미동</td>
      <td>60000</td>
      <td>62178</td>
      <td>24500</td>
      <td>120000</td>
      <td>19399</td>
      <td>479</td>
    </tr>
    <tr>
      <th>1</th>
      <td>금곡동</td>
      <td>74000</td>
      <td>72730</td>
      <td>37000</td>
      <td>112000</td>
      <td>18577</td>
      <td>234</td>
    </tr>
    <tr>
      <th>2</th>
      <td>백현동</td>
      <td>139000</td>
      <td>149865</td>
      <td>110000</td>
      <td>262000</td>
      <td>31896</td>
      <td>141</td>
    </tr>
    <tr>
      <th>3</th>
      <td>분당동</td>
      <td>72000</td>
      <td>71763</td>
      <td>30000</td>
      <td>105900</td>
      <td>15138</td>
      <td>141</td>
    </tr>
    <tr>
      <th>4</th>
      <td>삼평동</td>
      <td>111250</td>
      <td>126773</td>
      <td>80000</td>
      <td>203000</td>
      <td>30174</td>
      <td>168</td>
    </tr>
    <tr>
      <th>5</th>
      <td>서현동</td>
      <td>85000</td>
      <td>83804</td>
      <td>32000</td>
      <td>140000</td>
      <td>23423</td>
      <td>410</td>
    </tr>
    <tr>
      <th>6</th>
      <td>수내동</td>
      <td>101500</td>
      <td>98569</td>
      <td>33500</td>
      <td>170000</td>
      <td>26039</td>
      <td>452</td>
    </tr>
    <tr>
      <th>7</th>
      <td>야탑동</td>
      <td>64000</td>
      <td>63335</td>
      <td>28800</td>
      <td>117000</td>
      <td>20625</td>
      <td>508</td>
    </tr>
    <tr>
      <th>8</th>
      <td>운중동</td>
      <td>89500</td>
      <td>89804</td>
      <td>51638</td>
      <td>169700</td>
      <td>15496</td>
      <td>118</td>
    </tr>
    <tr>
      <th>9</th>
      <td>이매동</td>
      <td>90000</td>
      <td>90364</td>
      <td>50000</td>
      <td>155000</td>
      <td>19528</td>
      <td>337</td>
    </tr>
    <tr>
      <th>10</th>
      <td>정자동</td>
      <td>84500</td>
      <td>87273</td>
      <td>33000</td>
      <td>350000</td>
      <td>35267</td>
      <td>881</td>
    </tr>
    <tr>
      <th>11</th>
      <td>판교동</td>
      <td>99600</td>
      <td>100175</td>
      <td>59000</td>
      <td>185000</td>
      <td>22361</td>
      <td>134</td>
    </tr>
  </tbody>
</table>
</div>

<br>


## 전체 코드 정리

```python
import PublicDataReader as pdr

# Open API 서비스 키 초기화
serviceKey = "<< OPEN API SERVICE KEY HERE >>"
Apt = pdr.AptTransactionReader(serviceKey)

# 지역코드 검색기
df_code = Apt.CodeFinder("분당구")

# 특정 월 아파트매매 실거래 자료 수집기
df = Apt.DataReader("41135", "201911")

# 특정 기간 아파트매매 실거래 자료 수집기
df_sum = Apt.DataCollector("41135", "2019-01", "2019-11")

# 법정동 별 아파트매매 실거래 자료 요약 통계량 확인하기
df_agg = Apt.Agg(df)
df_sum_agg = Apt.Agg(df_sum)
```

<br>

## Open Source Project

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}


`PublicDataReader`는 파이썬 오픈소스 프로젝트로 개발하고 있습니다. 크롤링 코드 작성과 같은 데이터 활용의 번거로움을 줄이고자 시작하게 되었습니다. 실제 활용시 발생하는 오류나 개선 사항 등 피드백을 남겨주시면 적극 반영하도록 하겠습니다. 또한, 이 프로젝트에 참여하고자 하시는 분들은 언제든지 연락주시면 감사하겠습니다.

**Email : wooil@kakao.com**
