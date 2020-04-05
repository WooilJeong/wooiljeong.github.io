---
title: "PublicDataReader - 미세먼지 데이터 수집하기"
categories: Python
tags: PublicDataReader
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 한국환경공단 미세먼지 데이터 수집하기

<br>

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}

<br>

### PublicDataReader 관련 글 목록

- [01 국토교통부 부동산 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [02 한국환경공단 대기오염 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_02/)
- [데이터 과학을 위한 파이썬 라이브러리 모음](https://wooiljeong.github.io/python/python_library/)

<br>

[공공 데이터 포털](https://www.data.go.kr/)에서는 2019년 12월 4일 현재 총 3,254건에 이르는 다양한 오픈 API 서비스를 제공하고 있습니다. 저도 부동산이나 상권 분석을 할 때, 오픈 API 서비스를 이용해 데이터를 수집하여 분석에 활용하고 있습니다. 다만, 서비스를 이용하려면 `Requests`, `BeautifulSoup` 등의 파이썬 라이브러리를 통해 크롤링하는 코드를 작성해야한다는 번거로움이 있습니다.  

<br>

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_01.png){: .align-center}

이런 수고로움을 줄이기 위해, 단 코드 3줄(라이브러리 불러오기, API 서비스키 등록하기, 데이터 수집하기)만으로 손쉽게 공공 데이터 포털의 데이터를 `Pandas`의 `DataFrame`형태로 가져올 수 있는 라이브러리 `PublicDataReader`를 만들어 보았습니다.  


그럼 `PublicDataReader`를 활용해 한국환경공단에서 제공하는 다음 서비스들을 이용해 보겠습니다.

- [한국환경공단 측정소정보](https://www.data.go.kr/dataset/15000660/openapi.do)
- [한국환경공단 대기오염정보](https://www.data.go.kr/dataset/15000581/openapi.do)
- [한국환경공단 대기오염통계 현황](https://www.data.go.kr/dataset/15000583/openapi.do)


![PNG](/assets/img/post_img/2019-12-04-public_data_reader_02/img_01.jpg){: .align-center}

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

주피터 노트북에서 `PublicDataReader`라이브러리를 불러옵니다.

```python
import PublicDataReader as pdr
```

<br>

## Open API 서비스 키 입력하기

공공 데이터 포털에서 발급받은 서비스 키를 복사하여 다음과 같이 `serviceKey`에 문자열로 할당해줍니다. 오픈 API 서비스 키 발급에 관해 모르시는 분들은 구글에 '**공공 데이터 포털 Open API 사용법**'을 검색해보시면 여러 문서를 참조하실 수 있습니다. 검색 후 가장 상단에 있는 [이 블로그](https://jeong-pro.tistory.com/143)를 참조하셔도 됩니다!

<br>

## 서비스 1 : 측정소정보 조회 서비스

측정소정보 조회 서비스의 오퍼레이션을 사용하기 위해 `serviceKey`를 초기화하여, 인스턴스 `AirStn`을 생성합니다.

```python
serviceKey = "<< OPEN API SERVICE KEY HERE >>"
AirStn = pdr.AirStation(serviceKey)
```

<br>

### 오퍼레이션 1 : 근접측정소 목록 조회 (getNearbyMsrstnList)

- TM 좌표를 이용하여 좌표 주변 측정소 정보와 측정정소와 좌표 간의 거리 정보를 제공하는 서비스

```python
# 오퍼레이션 1 - 입력 데이터 예시
tmX = "202447.342016"
tmY = "447610.908303"
ver = '1.0'

# 데이터 조회
df_1 = AirStn.GetNearby(tmX, tmY, ver)
df_1.head()
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
      <th>stationName</th>
      <th>addr</th>
      <th>tm</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>대정읍</td>
      <td>제주 서귀포시 대정읍 동일하모로149번길 21-8대정청소년 수련관</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>부산북항</td>
      <td>부산 동구 충장대로 314자성대부두(관공선부두) 내 (좌천동)</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>고산리</td>
      <td>제주 제주시 한경면 고산리3762</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>

<br>


### 오퍼레이션 2 : 측정소 목록 조회 (getMsrstnList)

- 측정소 주소 또는 측정소 명칭으로 측정소 목록 또는 단 건의 측정소 상세 정보를 제공하는 서비스


```python
# 오퍼레이션 2 - 입력 데이터 예시
numOfRows = '10'
pageNo = '1'
addr = '서울'
stationName = '강남구'

# 데이터 조회
df_2 = AirStn.GetList(addr, stationName, pageNo, numOfRows)
df_2.head()
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
      <th>stationName</th>
      <th>addr</th>
      <th>year</th>
      <th>oper</th>
      <th>photo</th>
      <th>vrml</th>
      <th>map</th>
      <th>mangName</th>
      <th>item</th>
      <th>dmX</th>
      <th>dmY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강남구</td>
      <td>서울</td>
      <td>1978</td>
      <td>서울특별시보건환경연구원</td>
      <td>http://www.airkorea.or.kr/airkorea/station_pho...</td>
      <td>http://www.airkorea.or.kr/airkorea/vrml/111261...</td>
      <td>http://www.airkorea.or.kr/airkorea/station_map...</td>
      <td>도시대기</td>
      <td>SO2, CO, O3, NO2, PM10, PM2.5</td>
      <td>37.517562</td>
      <td>127.047289</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강남구</td>
      <td>서울</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


<br>

### 오퍼레이션 3 : TM 기준좌표 조회 (getTMStdrCrdnt)

- 검색서비스를 사용하여 읍면동 이름을 검색조건으로 기준좌표(TM좌표) 정보를 제공하는 서비스


```python
# 오퍼레이션 3 - 입력 데이터 예시
numOfRows = '10'
pageNo = '1'
umdName = '압구정동'

# 데이터 조회
df_3 = AirStn.GetTM(umdName,pageNo,numOfRows)
df_3.head()
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
      <th>sidoName</th>
      <th>sggName</th>
      <th>umdName</th>
      <th>tmX</th>
      <th>tmY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>압구정동</td>
      <td>202447.342016</td>
      <td>447610.908303</td>
    </tr>
  </tbody>
</table>
</div>

<br>


## 서비스 2 : 대기오염정보 조회 서비스

대기오염정보 조회 서비스의 오퍼레이션을 사용하기 위해 `serviceKey`를 초기화하여, 인스턴스 `AirRT`를 생성합니다.


```python
serviceKey = "<< OPEN API SERVICE KEY HERE >>"
AirRT = pdr.AirDataRT(serviceKey)
```
<br>

### 오퍼레이션 1 : 측정소별 실시간 측정정보 조회 (getMsrstnAcctoRltmMesureDnsty)


```python
# 오퍼레이션 1 - 입력 데이터 예시
stationName = '서초구'
dataTerm = 'DAILY'
pageNo = '1'
numOfRows = '25'
ver = '1.3'

# 데이터 조회
df = AirRT.DataReader(stationName, dataTerm, pageNo, numOfRows, ver)
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
      <th>dataTime</th>
      <th>mangName</th>
      <th>so2Value</th>
      <th>coValue</th>
      <th>o3Value</th>
      <th>no2Value</th>
      <th>pm10Value</th>
      <th>pm10Value24</th>
      <th>pm25Value</th>
      <th>pm25Value24</th>
      <th>khaiValue</th>
      <th>khaiGrade</th>
      <th>so2Grade</th>
      <th>coGrade</th>
      <th>o3Grade</th>
      <th>no2Grade</th>
      <th>pm10Grade</th>
      <th>pm25Grade</th>
      <th>pm10Grade1h</th>
      <th>pm25Grade1h</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-12-13 17:00</td>
      <td>도시대기</td>
      <td>0.004</td>
      <td>0.4</td>
      <td>0.013</td>
      <td>0.044</td>
      <td>30</td>
      <td>31</td>
      <td>19</td>
      <td>18</td>
      <td>73</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-12-13 16:00</td>
      <td>도시대기</td>
      <td>0.004</td>
      <td>0.4</td>
      <td>0.014</td>
      <td>0.044</td>
      <td>30</td>
      <td>31</td>
      <td>20</td>
      <td>18</td>
      <td>73</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-12-13 15:00</td>
      <td>도시대기</td>
      <td>0.003</td>
      <td>0.3</td>
      <td>0.019</td>
      <td>0.037</td>
      <td>34</td>
      <td>31</td>
      <td>14</td>
      <td>18</td>
      <td>61</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-12-13 14:00</td>
      <td>도시대기</td>
      <td>0.003</td>
      <td>0.3</td>
      <td>0.016</td>
      <td>0.039</td>
      <td>21</td>
      <td>31</td>
      <td>17</td>
      <td>19</td>
      <td>65</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-12-13 13:00</td>
      <td>도시대기</td>
      <td>0.004</td>
      <td>0.4</td>
      <td>0.011</td>
      <td>0.048</td>
      <td>32</td>
      <td>33</td>
      <td>21</td>
      <td>19</td>
      <td>80</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>

<br>


## 서비스 3 : 대기오염통계 현황 조회 서비스

대기오염통계 현황 조회 서비스의 오퍼레이션을 사용하기 위해 `serviceKey`를 초기화하여, 인스턴스 `Air`를 생성합니다.


```python
serviceKey = '<< OPEN API SERVICE KEY HERE >>'
Air = pdr.AirData(serviceKey)
```
<br>

### 오퍼레이션 1 : 측정소별 최종확정 농도 조회 (getMsrstnAcctoLastDcsnDnsty)


```python
# 오퍼레이션 1 - 입력 데이터 예시
stationName = '종로구'
searchCondition='DAILY'
pageNo='1'
numOfRows='5000'

# 데이터 조회
df = Air.StnDataReader(stationName, searchCondition, pageNo, numOfRows)
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
      <th>dataTime</th>
      <th>so2Avg</th>
      <th>coAvg</th>
      <th>o3Avg</th>
      <th>no2Avg</th>
      <th>pm10Avg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2006-01-01</td>
      <td>0.008</td>
      <td>0.8</td>
      <td>0.002</td>
      <td>0.039</td>
      <td>59</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2006-01-02</td>
      <td>0.008</td>
      <td>0.7</td>
      <td>0.010</td>
      <td>0.028</td>
      <td>65</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2006-01-03</td>
      <td>0.007</td>
      <td>0.5</td>
      <td>0.015</td>
      <td>0.022</td>
      <td>30</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2006-01-04</td>
      <td>0.006</td>
      <td>0.4</td>
      <td>0.015</td>
      <td>0.018</td>
      <td>27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2006-01-05</td>
      <td>0.005</td>
      <td>0.5</td>
      <td>0.012</td>
      <td>0.025</td>
      <td>33</td>
    </tr>
  </tbody>
</table>
</div>

<br>


### 오퍼레이션 2 : 기간별 오염통계 정보 조회 (getDatePollutnStatInfo)


```python
# 오퍼레이션 2 - 입력 데이터 예시
searchDataTime='2017-10'
statArticleCondition='도시대기'
pageNo='1'
numOfRows='10'

# 데이터 조회
df = Air.PeriodDataReader(searchDataTime,statArticleCondition,pageNo,numOfRows)
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
      <th>sidoName</th>
      <th>dataTime</th>
      <th>so2Avg</th>
      <th>coAvg</th>
      <th>o3Avg</th>
      <th>no2Avg</th>
      <th>pm10Avg</th>
      <th>so2Max</th>
      <th>coMax</th>
      <th>o3Max</th>
      <th>no2Max</th>
      <th>pm10Max</th>
      <th>so2Min</th>
      <th>coMin</th>
      <th>o3Min</th>
      <th>no2Min</th>
      <th>pm10Min</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>서울</td>
      <td>2017-10</td>
      <td>0.006</td>
      <td>0.7</td>
      <td>0.027</td>
      <td>0.032</td>
      <td>36</td>
      <td>0.012</td>
      <td>1.5</td>
      <td>0.080</td>
      <td>0.113</td>
      <td>126</td>
      <td>0.005</td>
      <td>0.3</td>
      <td>0.004</td>
      <td>0.014</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>부산</td>
      <td>2017-10</td>
      <td>0.006</td>
      <td>0.4</td>
      <td>0.037</td>
      <td>0.023</td>
      <td>39</td>
      <td>0.030</td>
      <td>1.2</td>
      <td>0.111</td>
      <td>0.093</td>
      <td>122</td>
      <td>0.002</td>
      <td>0.3</td>
      <td>0.006</td>
      <td>0.004</td>
      <td>15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>대구</td>
      <td>2017-10</td>
      <td>0.003</td>
      <td>0.4</td>
      <td>0.025</td>
      <td>0.022</td>
      <td>39</td>
      <td>0.010</td>
      <td>0.8</td>
      <td>0.078</td>
      <td>0.074</td>
      <td>135</td>
      <td>0.001</td>
      <td>0.2</td>
      <td>0.002</td>
      <td>0.004</td>
      <td>10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>인천</td>
      <td>2017-10</td>
      <td>0.007</td>
      <td>0.7</td>
      <td>0.035</td>
      <td>0.031</td>
      <td>46</td>
      <td>0.027</td>
      <td>1.6</td>
      <td>0.079</td>
      <td>0.100</td>
      <td>128</td>
      <td>0.004</td>
      <td>0.3</td>
      <td>0.004</td>
      <td>0.007</td>
      <td>12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>광주</td>
      <td>2017-10</td>
      <td>0.003</td>
      <td>0.5</td>
      <td>0.026</td>
      <td>0.022</td>
      <td>37</td>
      <td>0.008</td>
      <td>1.1</td>
      <td>0.082</td>
      <td>0.077</td>
      <td>143</td>
      <td>0.002</td>
      <td>0.2</td>
      <td>0.002</td>
      <td>0.005</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>

<br>


## Open Source Project

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}


`PublicDataReader`는 파이썬 오픈소스 프로젝트로 개발하고 있습니다. 크롤링 코드 작성과 같은 데이터 활용의 번거로움을 줄이고자 시작하게 되었습니다. 실제 활용시 발생하는 오류나 개선 사항 등 피드백을 남겨주시면 적극 반영하도록 하겠습니다. 또한, 이 프로젝트에 참여하고자 하시는 분들은 언제든지 연락주시면 감사하겠습니다.

<br>

## Github Repo에 Star를 눌러주세요.

`PublicDataReader`의 [Github Repository](https://github.com/WooilJeong/PublicDataReader)에 Star를 눌러주세요. 고독한 오픈소스 개발의 원동력이 됩니다!

**Email : wooil@kakao.com**

<br>

### PublicDataReader 관련 글 목록

- [01 국토교통부 부동산 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [02 한국환경공단 대기오염 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_02/)
- [데이터 과학을 위한 파이썬 라이브러리 모음](https://wooiljeong.github.io/python/python_library/)
