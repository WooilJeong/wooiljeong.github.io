---
title: "PublicDataReader - 상가업소 데이터 수집하기"
categories: Python
tags: PublicDataReader
header:
  overlay_image: /assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 소상공인진흥공단 상가업소 데이터 수집하기

<br>

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}

<br>

### PublicDataReader 관련 글 목록

- [국토교통부 부동산 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [국토교통부 건축물대장 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_03/)
- [소상공인 진흥공단 상가업소 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_02/)
- [데이터 과학을 위한 파이썬 라이브러리 모음](https://wooiljeong.github.io/python/python_library/)

### PublicDataReader Github Repository

- [PublicDataReader](https://github.com/WooilJeong/PublicDataReader)  


## 소개

- **최신 버전**  
![](https://img.shields.io/badge/PublicDataReader-2021.4.12-blue.svg)  

    - 2021.4.12 Version (2021-04):
      - 국토교통부 건축물대장정보 서비스 추가
    - 2021.1.9 Version (2021-01): 
      - 소상공인 상가업소 정보 조회 기능 추가
      - 서울시 지하철호선별 역별 승하차 인원 정보 조회 기능 추가   
      - 서울시 버스노선별 정류장별 시간대별 승하차 인원 정보 조회 기능 추가
    - 0.1.2 Version (2020-12): 
      - 국토교통부 실거래가 정보 조회 기능 전면 수정


- **요구 사항**  
![](https://img.shields.io/badge/Python-3.7.4-yellow.svg) ![](https://img.shields.io/badge/Pandas-0.25.3-red.svg)

**PublicDataReader**는 [공공데이터포털](https://data.go.kr), [서울 열린데이터 광장](https://data.seoul.go.kr/) 등 에서 제공하는 OpenAPI 서비스를 Python으로 쉽게 이용할 수 있도록 도와주는 **데이터 수집 라이브러리**입니다. 

**2021년 04월** 현재 아래 Open API 서비스를 이용하여 데이터를 Pandas DataFrame 형태로 조회할 수 있습니다. 추후 수요가 높은 Open API 서비스에 대한 인터페이스도 지속적으로 업데이트할 예정입니다.

- [국토교통부 실거래가 정보](https://www.data.go.kr/dataset/3050988/openapi.do)
- [국토교통부 건축물대장정보 서비스](https://www.data.go.kr/data/15044713/openapi.do)
- [소상공인 상가업소 정보](https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15012005)
- [서울시 지하철호선별 역별 승하차 인원 정보](https://data.seoul.go.kr/dataList/OA-12914/S/1/datasetView.do)
- [서울시 버스노선별 정류장별 시간대별 승하차 인원 정보](https://data.seoul.go.kr/dataList/OA-12913/S/1/datasetView.do)


### 소상공인 상가업소 정보 조회 서비스

**메서드**              | **서비스 명**
---------------------- | --------------------
storeZoneOne | 지정 상권조회
storeZoneInRadius | 반경내 상권조회
storeZoneInRectangle | 사각형내 상권조회
storeZoneInAdmi | 행정구역 단위 상권조회
storeOne | 단일 상가업소 조회
storeListInBuilding | 건물단위 상가업소 조회
storeListInPnu | 지번단위 상가업소 조회
storeListInDong | 행정동 단위 상가업소 조회
storeListInArea | 상권내 상가업소 조회
storeListInRadius | 반경내 상가업소 조회
storeListInRectangle | 사각형내 상가업소 조회
storeListInPolygon | 다각형내 상가업소 조회
storeListInUpjong | 업종별 상가업소 조회
storeListByDate | 수정일자기준 상가업소 조회
reqStoreModify | 상가업소정보 변경요청
largeUpjongList | 상권정보 업종 대분류 조회
middleUpjongList | 상권정보 업종 중분류 조회
smallUpjongList | 상권정보 업종 소분류 조회


<br>

## 개발 배경

[공공 데이터 포털](https://www.data.go.kr/), [서울 열린데이터 광장](https://data.seoul.go.kr/) 등의 공공기관에서는 다양한 공공 데이터 조회를 위한 오픈 API 서비스를 제공하고 있습니다. 그러나 개발자가 아닌 데이터 분석가의 입장에서 웹 서비스를 이용해 데이터를 분석하기에는 약간의 어려움이 존재합니다. 데이터 수집을 위해서는 웹 서비스와 크롤링 기술에 대한 이해가 필요합니다. PublicDataReader는 웹 관련 사전지식이 부족한 데이터 분석가라도 손 쉽게 원하는 공공 데이터를 수집하고, 분석할 수 있도록 돕기 위해 개발되었습니다. 기관에서 발급받은 서비스키만 있다면 간단하게 원하는 데이터를 조회할 수 있습니다. 단 코드 3줄(라이브러리 불러오기, API 서비스키 등록하기, 데이터 수집하기)만으로 손쉽게 공공 데이터를 `Pandas`의 `DataFrame`형태로 불러올 수 있습니다.

<br>

'[소상공인 상가업소 정보](https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15012005)' Open API 서비스를 이용해 데이터를 수집하는 방법에 대해 알아보겠습니다.

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

## Open API 서비스 키 입력하기

공공 데이터 포털에서 발급받은 서비스 키를 복사하여 다음과 같이 `serviceKey`에 문자열로 할당해줍니다. 오픈 API 서비스 키 발급에 관해 모르시는 분들은 구글에 '**공공 데이터 포털 Open API 사용법**'을 검색해보시면 여러 문서를 참조하실 수 있습니다. 검색 후 가장 상단에 있는 [이 블로그](https://jeong-pro.tistory.com/143)를 참조하셔도 됩니다.


```python
import PublicDataReader as pdr
print(pdr.__version__)
```

    
    >>> PublicDataReader Version : 2021.1.9
    
    - Author : Wooil Jeong
    - E-mail : wooil@kakao.com
    - Github : https://github.com/WooilJeong/PublicDataReader
    - Blog : https://wooiljeong.github.io
    

<br>


## 서비스 키로 데이터 수집기 만들기

다음과 같이 소상공인 상가업소 정보를 수집할 `semas` 인스턴스를 만들어줍니다. 

```python
# 공공 데이터 포털에서 발급 받은 서비스 인증키
serviceKey="OPEN API SERVICE KEY HERE"

# 국토교통부 실거래가 Open API 인스턴스 생성
semas = pdr.StoreInfo(serviceKey)
```

<br>
    
## 소상공인 상가업소 관련 정보 수집하기

위에서 생성한 데이터 수집 인스턴스 `semas`의 여러 메서드를 이용해 아래와 같이 다양한 데이터를 조회할 수 있습니다.

```python
# 01 지정 상권조회
key = 1
df = semas.storeZoneOne(key=key)
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
      <th>trarNo</th>
      <th>mainTrarNm</th>
      <th>ctprvnCd</th>
      <th>ctprvnNm</th>
      <th>signguCd</th>
      <th>signguNm</th>
      <th>trarArea</th>
      <th>coordNum</th>
      <th>coords</th>
      <th>stdrDt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>제주 서귀포시 대정읍_1</td>
      <td>50</td>
      <td>제주특별자치도</td>
      <td>50130</td>
      <td>서귀포시</td>
      <td>330176.3</td>
      <td>61</td>
      <td>POLYGON ((126.253169 33.223738, 126.25313 33.2...</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 02 반경내 상권조회
radius = 500
cx = 127.03641615737838
cy = 37.50059843782878
df = semas.storeZoneInRadius(radius=radius, cx=cx, cy=cy)
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
      <th>trarNo</th>
      <th>mainTrarNm</th>
      <th>ctprvnCd</th>
      <th>ctprvnNm</th>
      <th>signguCd</th>
      <th>signguNm</th>
      <th>trarArea</th>
      <th>coordNum</th>
      <th>coords</th>
      <th>stdrDt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1910</td>
      <td>서울 강남구 역삼역_1</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>275398.4</td>
      <td>45</td>
      <td>POLYGON ((127.035609 37.496615, 127.036312 37....</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1916</td>
      <td>역삼역_2</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>63510.1</td>
      <td>18</td>
      <td>POLYGON ((127.042261 37.501568, 127.043012 37....</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1928</td>
      <td>서울 강남구 역삼역_2</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>161121.5</td>
      <td>17</td>
      <td>POLYGON ((127.041909 37.504139, 127.041256 37....</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 03 사각형내 상권조회
minx = 127.0327683531071
miny = 37.495967935149146
maxx = 127.04268179746694
maxy = 37.502402894207286
df = semas.storeZoneInRectangle(minx=minx, miny=miny, maxx=maxx, maxy=maxy)
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
      <th>trarNo</th>
      <th>mainTrarNm</th>
      <th>ctprvnCd</th>
      <th>ctprvnNm</th>
      <th>signguCd</th>
      <th>signguNm</th>
      <th>trarArea</th>
      <th>coordNum</th>
      <th>coords</th>
      <th>stdrDt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1910</td>
      <td>서울 강남구 역삼역_1</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>275398.4</td>
      <td>45</td>
      <td>POLYGON ((127.035609 37.496615, 127.036312 37....</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1916</td>
      <td>역삼역_2</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>63510.1</td>
      <td>18</td>
      <td>POLYGON ((127.042261 37.501568, 127.043012 37....</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 04 행정구역 단위 상권조회
divId = 'adongCd'
key = '1168058000'
df = semas.storeZoneInAdmi(divId=divId, key=key)
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
      <th>trarNo</th>
      <th>mainTrarNm</th>
      <th>ctprvnCd</th>
      <th>ctprvnNm</th>
      <th>signguCd</th>
      <th>signguNm</th>
      <th>trarArea</th>
      <th>coordNum</th>
      <th>coords</th>
      <th>stdrDt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1972</td>
      <td>포스코사거리_2</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>98268.5</td>
      <td>11</td>
      <td>POLYGON ((127.054383 37.513134, 127.053446 37....</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1976</td>
      <td>코엑스</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>218417.4</td>
      <td>6</td>
      <td>POLYGON ((127.060661 37.508194, 127.062988 37....</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1977</td>
      <td>삼성역_3</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>87017.6</td>
      <td>11</td>
      <td>POLYGON ((127.064546 37.509384, 127.066389 37....</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 05 단일 상가업소 조회
key = '19911027'
df = semas.storeOne(key=key)
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
      <th>bizesId</th>
      <th>bizesNm</th>
      <th>brchNm</th>
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>ksicCd</th>
      <th>...</th>
      <th>bldMngNo</th>
      <th>bldNm</th>
      <th>rdnmAdr</th>
      <th>oldZipcd</th>
      <th>newZipcd</th>
      <th>dongNo</th>
      <th>flrNo</th>
      <th>hoNo</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19911027</td>
      <td>서울떡집</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q08</td>
      <td>제과제빵떡케익</td>
      <td>Q08A03</td>
      <td>떡전문</td>
      <td>I56191</td>
      <td>...</td>
      <td>2720010100102040008028843</td>
      <td></td>
      <td>대구광역시 남구 명덕로64길 53</td>
      <td>705836</td>
      <td>42433</td>
      <td></td>
      <td></td>
      <td></td>
      <td>128.605390114157</td>
      <td>35.8527275873608</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 39 columns</p>
</div>




```python
# 06. 건물단위 상가업소 조회
key = '1168011000104940000004966'
pageNo = '1'
df = semas.storeListInBuilding(key=key, pageNo=1)
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
      <th>bizesId</th>
      <th>bizesNm</th>
      <th>brchNm</th>
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>ksicCd</th>
      <th>...</th>
      <th>bldMngNo</th>
      <th>bldNm</th>
      <th>rdnmAdr</th>
      <th>oldZipcd</th>
      <th>newZipcd</th>
      <th>dongNo</th>
      <th>flrNo</th>
      <th>hoNo</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20291153</td>
      <td>드림리더</td>
      <td></td>
      <td>D</td>
      <td>소매</td>
      <td>D03</td>
      <td>종합소매점</td>
      <td>D03B01</td>
      <td>백화점</td>
      <td>G47111</td>
      <td>...</td>
      <td>1168011000104940000004966</td>
      <td>갤러리아백화점</td>
      <td>서울특별시 강남구 압구정로 343</td>
      <td>135902</td>
      <td>06008</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.04008070443</td>
      <td>37.5284986430328</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22356602</td>
      <td>갤러리아백화점</td>
      <td>압구정점</td>
      <td>D</td>
      <td>소매</td>
      <td>D03</td>
      <td>종합소매점</td>
      <td>D03B01</td>
      <td>백화점</td>
      <td>G47111</td>
      <td>...</td>
      <td>1168011000104940000004966</td>
      <td>갤러리아백화점</td>
      <td>서울특별시 강남구 압구정로 343</td>
      <td>135902</td>
      <td>06008</td>
      <td></td>
      <td></td>
      <td></td>
      <td>127.04008070443</td>
      <td>37.5284986430328</td>
    </tr>
    <tr>
      <th>2</th>
      <td>22475029</td>
      <td>크리스챤디올꾸뛰르코리아</td>
      <td></td>
      <td>D</td>
      <td>소매</td>
      <td>D03</td>
      <td>종합소매점</td>
      <td>D03A06</td>
      <td>종합소매</td>
      <td>G47190</td>
      <td>...</td>
      <td>1168011000104940000004966</td>
      <td>갤러리아백화점</td>
      <td>서울특별시 강남구 압구정로 343</td>
      <td>135902</td>
      <td>06008</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.04008070443</td>
      <td>37.5284986430328</td>
    </tr>
    <tr>
      <th>3</th>
      <td>22365582</td>
      <td>구찌코리아</td>
      <td></td>
      <td>D</td>
      <td>소매</td>
      <td>D06</td>
      <td>가방/신발/액세서리</td>
      <td>D06A10</td>
      <td>가방/가죽제품소매</td>
      <td>G47430</td>
      <td>...</td>
      <td>1168011000104940000004966</td>
      <td>갤러리아백화점</td>
      <td>서울특별시 강남구 압구정로 343</td>
      <td>135902</td>
      <td>06008</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.04008070443</td>
      <td>37.5284986430328</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3128818</td>
      <td>빈스앤베리즈</td>
      <td>웨스트임시점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>1168011000104940000004966</td>
      <td>갤러리아백화점</td>
      <td>서울특별시 강남구 압구정로 343</td>
      <td>135902</td>
      <td>06008</td>
      <td></td>
      <td></td>
      <td></td>
      <td>127.04008070443</td>
      <td>37.5284986430328</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>




```python
# 07. 지번단위 상가업소 조회
key = '1168010600209380024'
pageNo = '1'
indsLclsCd = 'Q'
indsMclsCd = 'Q12'
indsSclsCd = 'Q12A01' 
df = semas.storeListInPnu(key=key, indsLclsCd_=indsLclsCd, pageNo=1)
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
      <th>bizesId</th>
      <th>bizesNm</th>
      <th>brchNm</th>
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>ksicCd</th>
      <th>...</th>
      <th>bldMngNo</th>
      <th>bldNm</th>
      <th>rdnmAdr</th>
      <th>oldZipcd</th>
      <th>newZipcd</th>
      <th>dongNo</th>
      <th>flrNo</th>
      <th>hoNo</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20019200</td>
      <td>쿠벅커피</td>
      <td>강남롯데점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>1168010600109380024014224</td>
      <td>삼환아르누보2</td>
      <td>서울특별시 강남구 도곡로 405</td>
      <td>135280</td>
      <td>06207</td>
      <td></td>
      <td>1</td>
      <td>103</td>
      <td>127.054649873493</td>
      <td>37.4973585734254</td>
    </tr>
    <tr>
      <th>1</th>
      <td>25579853</td>
      <td>시초동연가</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1168010600109380024014224</td>
      <td>삼환아르누보2</td>
      <td>서울특별시 강남구 도곡로 405</td>
      <td>135280</td>
      <td>06207</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.054649873493</td>
      <td>37.4973585734254</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2916182</td>
      <td>죽이야기</td>
      <td>대치롯데점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1168010600109380024014224</td>
      <td>삼환아르누보2</td>
      <td>서울특별시 강남구 도곡로 405</td>
      <td>135280</td>
      <td>06207</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.054649873493</td>
      <td>37.4973585734254</td>
    </tr>
    <tr>
      <th>3</th>
      <td>22563344</td>
      <td>쫄족브라더스</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1168010600109380024014224</td>
      <td>삼환아르누보2</td>
      <td>서울특별시 강남구 도곡로 405</td>
      <td>135280</td>
      <td>06207</td>
      <td></td>
      <td>1</td>
      <td>103</td>
      <td>127.054649873493</td>
      <td>37.4973585734254</td>
    </tr>
  </tbody>
</table>
<p>4 rows × 39 columns</p>
</div>




```python
# 08. 행정동 단위 상가업소 조회
divId = 'adongCd'
key = '1168064000'
indsLclsCd = 'Q'
pageNo = 1

df = semas.storeListInDong(divId = divId, key = key, indsLclsCd_=indsLclsCd, pageNo = pageNo)
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
      <th>bizesId</th>
      <th>bizesNm</th>
      <th>brchNm</th>
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>ksicCd</th>
      <th>...</th>
      <th>bldMngNo</th>
      <th>bldNm</th>
      <th>rdnmAdr</th>
      <th>oldZipcd</th>
      <th>newZipcd</th>
      <th>dongNo</th>
      <th>flrNo</th>
      <th>hoNo</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10117402</td>
      <td>청사이모떡도리탕</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q08</td>
      <td>제과제빵떡케익</td>
      <td>Q08A04</td>
      <td>떡/한과집</td>
      <td>I56191</td>
      <td>...</td>
      <td>1168010100108170014026207</td>
      <td></td>
      <td>서울특별시 강남구 테헤란로1길 28-4</td>
      <td>135080</td>
      <td>06129</td>
      <td></td>
      <td></td>
      <td></td>
      <td>127.027742937871</td>
      <td>37.5001587216431</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10159268</td>
      <td>쿠와</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>1168010100107510013025291</td>
      <td>우성빌딩</td>
      <td>서울특별시 강남구 역삼로 145</td>
      <td>135080</td>
      <td>06244</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.035197691487</td>
      <td>37.4947504340001</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10680644</td>
      <td>퍼니플러스멀티방</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>1168010100106190018023495</td>
      <td></td>
      <td>서울특별시 강남구 테헤란로1길 42</td>
      <td>135080</td>
      <td>06129</td>
      <td></td>
      <td>4</td>
      <td></td>
      <td>127.02713108529</td>
      <td>37.5011178128053</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10904829</td>
      <td>알라카르테</td>
      <td>강남점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>1168010100107970022025616</td>
      <td>정유빌딩</td>
      <td>서울특별시 강남구 논현로 319</td>
      <td>135080</td>
      <td>06257</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.039858038296</td>
      <td>37.4937792195489</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10931041</td>
      <td>리골레토시카고피자</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q07</td>
      <td>패스트푸드</td>
      <td>Q07A01</td>
      <td>피자전문</td>
      <td>I56192</td>
      <td>...</td>
      <td>1168010100108120018023186</td>
      <td></td>
      <td>서울특별시 강남구 봉은사로4길 37</td>
      <td>135080</td>
      <td>06123</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.027091400025</td>
      <td>37.5024330256564</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>




```python
# 09 상권내 상가업소 조회
key = '1819'
pageNo = '1'
indsLclsCd = 'Q'

df = semas.storeListInArea(key=key, pageNo=pageNo, indsLclsCd_=indsLclsCd)
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
      <th>bizesId</th>
      <th>bizesNm</th>
      <th>brchNm</th>
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>ksicCd</th>
      <th>...</th>
      <th>bldMngNo</th>
      <th>bldNm</th>
      <th>rdnmAdr</th>
      <th>oldZipcd</th>
      <th>newZipcd</th>
      <th>dongNo</th>
      <th>flrNo</th>
      <th>hoNo</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20107897</td>
      <td>롯데리아</td>
      <td>신림역점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q07</td>
      <td>패스트푸드</td>
      <td>Q07A04</td>
      <td>패스트푸드</td>
      <td>I56199</td>
      <td>...</td>
      <td>1162010200116400009021089</td>
      <td></td>
      <td>서울특별시 관악구 남부순환로 1610</td>
      <td>151930</td>
      <td>08776</td>
      <td></td>
      <td>2</td>
      <td></td>
      <td>126.929287904186</td>
      <td>37.4838994178</td>
    </tr>
    <tr>
      <th>1</th>
      <td>19997538</td>
      <td>할머니순대국밥</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A05</td>
      <td>해장국/감자탕</td>
      <td>I56111</td>
      <td>...</td>
      <td>1162010200115960006019401</td>
      <td></td>
      <td>서울특별시 관악구 신원로3길 28</td>
      <td>151869</td>
      <td>08774</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>126.927017092638</td>
      <td>37.4821848421044</td>
    </tr>
    <tr>
      <th>2</th>
      <td>25510560</td>
      <td>신사리즉석떡볶이</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q04</td>
      <td>분식</td>
      <td>Q04A01</td>
      <td>라면김밥분식</td>
      <td>I56194</td>
      <td>...</td>
      <td>1162010200116370020021523</td>
      <td></td>
      <td>서울특별시 관악구 신원로 40-12</td>
      <td>151930</td>
      <td>08776</td>
      <td></td>
      <td>2</td>
      <td></td>
      <td>126.929376957944</td>
      <td>37.4816205677277</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20867972</td>
      <td>전라도맛집</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1162010200116400038000003</td>
      <td>양지순대촌</td>
      <td>서울특별시 관악구 신림로59길 12</td>
      <td>151015</td>
      <td>08776</td>
      <td></td>
      <td>3</td>
      <td></td>
      <td>126.929182692016</td>
      <td>37.4833488236796</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22599910</td>
      <td>순천집</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q04</td>
      <td>분식</td>
      <td>Q04A01</td>
      <td>라면김밥분식</td>
      <td>I56194</td>
      <td>...</td>
      <td>1162010200116400038000003</td>
      <td>양지순대촌</td>
      <td>서울특별시 관악구 신림로59길 12</td>
      <td>151015</td>
      <td>08776</td>
      <td></td>
      <td></td>
      <td>209</td>
      <td>126.929182692016</td>
      <td>37.4833488236796</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>




```python
# 10. 반경내 상가업소 조회
radius = '500'
cx = 127.03641615737838
cy = 37.50059843782878
indsLclsCd = 'Q'
pageNo = '1'

df = semas.storeListInRadius(radius=radius, cx=cx, cy=cy, indsLclsCd_=indsLclsCd, pageNo=pageNo)
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
      <th>bizesId</th>
      <th>bizesNm</th>
      <th>brchNm</th>
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>ksicCd</th>
      <th>...</th>
      <th>bldMngNo</th>
      <th>bldNm</th>
      <th>rdnmAdr</th>
      <th>oldZipcd</th>
      <th>newZipcd</th>
      <th>dongNo</th>
      <th>flrNo</th>
      <th>hoNo</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>25693781</td>
      <td>카페드콜롬비아</td>
      <td>국기원점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>1168010100106350004023512</td>
      <td>과학기술회관</td>
      <td>서울특별시 강남구 테헤란로7길 22</td>
      <td>135703</td>
      <td>06130</td>
      <td></td>
      <td>2</td>
      <td></td>
      <td>127.030721483725</td>
      <td>37.5007148214433</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13323225</td>
      <td>현주네분식</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q04</td>
      <td>분식</td>
      <td>Q04A01</td>
      <td>라면김밥분식</td>
      <td>I56194</td>
      <td>...</td>
      <td>1168010100106350004023512</td>
      <td>과학기술회관</td>
      <td>서울특별시 강남구 테헤란로7길 22</td>
      <td>135703</td>
      <td>06130</td>
      <td></td>
      <td></td>
      <td></td>
      <td>127.030721483725</td>
      <td>37.5007148214433</td>
    </tr>
    <tr>
      <th>2</th>
      <td>16578662</td>
      <td>허수아비돈까스</td>
      <td>국기원점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q06</td>
      <td>양식</td>
      <td>Q06A02</td>
      <td>돈가스전문점</td>
      <td>I56114</td>
      <td>...</td>
      <td>1168010100106350004023512</td>
      <td>과학기술회관</td>
      <td>서울특별시 강남구 테헤란로7길 22</td>
      <td>135703</td>
      <td>06130</td>
      <td></td>
      <td></td>
      <td></td>
      <td>127.030721483725</td>
      <td>37.5007148214433</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20026069</td>
      <td>허바허바</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>1168010100106480000023703</td>
      <td></td>
      <td>서울특별시 강남구 테헤란로7길 12</td>
      <td>135080</td>
      <td>06133</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.030968160323</td>
      <td>37.5000050117406</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16210625</td>
      <td>투썸플레이스</td>
      <td>강남IBC점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>1168010100106480001023753</td>
      <td>비와이씨빌딩</td>
      <td>서울특별시 강남구 테헤란로7길 8</td>
      <td>135911</td>
      <td>06133</td>
      <td></td>
      <td>15</td>
      <td></td>
      <td>127.031106771288</td>
      <td>37.4997808849217</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>




```python
# 11. 사각형내 상가업소 조회
minx = 127.0327683531071
miny = 37.495967935149146
maxx = 127.04268179746694
maxy = 37.502402894207286
indsLclsCd = 'Q'
pageNo = 1

df = semas.storeListInRectangle(minx=minx, miny=miny, maxx=maxx, maxy=maxy, indsLclsCd_=indsLclsCd, pageNo=1)
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
      <th>bizesId</th>
      <th>bizesNm</th>
      <th>brchNm</th>
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>ksicCd</th>
      <th>...</th>
      <th>bldMngNo</th>
      <th>bldNm</th>
      <th>rdnmAdr</th>
      <th>oldZipcd</th>
      <th>newZipcd</th>
      <th>dongNo</th>
      <th>flrNo</th>
      <th>hoNo</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12561963</td>
      <td>돼지왕</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1168010100106370041026180</td>
      <td></td>
      <td>서울특별시 강남구 테헤란로19길 29</td>
      <td>135909</td>
      <td>06131</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.032798678569</td>
      <td>37.5018851976741</td>
    </tr>
    <tr>
      <th>1</th>
      <td>26392323</td>
      <td>제주꿀돼지</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1168010100106370023023548</td>
      <td></td>
      <td>서울특별시 강남구 강남대로94길 69</td>
      <td>135080</td>
      <td>06131</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.032788961767</td>
      <td>37.5008849470191</td>
    </tr>
    <tr>
      <th>2</th>
      <td>25982665</td>
      <td>엠케이크레딧</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q08</td>
      <td>제과제빵떡케익</td>
      <td>Q08A01</td>
      <td>제과점</td>
      <td>I56191</td>
      <td>...</td>
      <td>1168010100108230024023937</td>
      <td>남도빌딩</td>
      <td>서울특별시 강남구 테헤란로14길 6</td>
      <td>135080</td>
      <td>06234</td>
      <td></td>
      <td>7</td>
      <td>752</td>
      <td>127.032836099114</td>
      <td>37.4989841063617</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11978821</td>
      <td>스터번</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q06</td>
      <td>양식</td>
      <td>Q06A01</td>
      <td>정통양식/경양식</td>
      <td>I56114</td>
      <td>...</td>
      <td>1168010100106470002023600</td>
      <td></td>
      <td>서울특별시 강남구 강남대로94길 70</td>
      <td>135080</td>
      <td>06133</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.032826257509</td>
      <td>37.5006239463941</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20288291</td>
      <td>흑다돈</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1168010100108230035000001</td>
      <td></td>
      <td>서울특별시 강남구 테헤란로10길 21</td>
      <td>135080</td>
      <td>06234</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.032907048397</td>
      <td>37.497867378413</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>




```python
# 12. 다각형내 상가업소 조회
key = 'POLYGON((127.02355609555755 37.504264372557095, 127.02496157306963 37.50590702991155, 127.0270858825753 37.50486867039889, 127.02628121988377 37.503489842823114))'
pageNo = 1
indsLclsCd = 'Q'

df = semas.storeListInPolygon(key, indsLclsCd_=indsLclsCd, pageNo=pageNo)
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
      <th>bizesId</th>
      <th>bizesNm</th>
      <th>brchNm</th>
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>ksicCd</th>
      <th>...</th>
      <th>bldMngNo</th>
      <th>bldNm</th>
      <th>rdnmAdr</th>
      <th>oldZipcd</th>
      <th>newZipcd</th>
      <th>dongNo</th>
      <th>flrNo</th>
      <th>hoNo</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22596580</td>
      <td>대성</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q09</td>
      <td>유흥주점</td>
      <td>Q09A10</td>
      <td>룸살롱/단란주점</td>
      <td>I56211</td>
      <td>...</td>
      <td>1168010100106010001022455</td>
      <td>연우빌딩</td>
      <td>서울특별시 강남구 봉은사로 110</td>
      <td>135080</td>
      <td>06123</td>
      <td></td>
      <td></td>
      <td></td>
      <td>127.025796257769</td>
      <td>37.5046209238672</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20018685</td>
      <td>로얄분식</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q04</td>
      <td>분식</td>
      <td>Q04A01</td>
      <td>라면김밥분식</td>
      <td>I56194</td>
      <td>...</td>
      <td>1168010800102000006009377</td>
      <td>동양빌딩</td>
      <td>서울특별시 강남구 봉은사로 105</td>
      <td>135010</td>
      <td>06120</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.025011363289</td>
      <td>37.5049658590474</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20824421</td>
      <td>클로리스티롬즈</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q07</td>
      <td>패스트푸드</td>
      <td>Q07A04</td>
      <td>패스트푸드</td>
      <td>I56199</td>
      <td>...</td>
      <td>1168010100108090003022739</td>
      <td></td>
      <td>서울특별시 강남구 봉은사로2길 12</td>
      <td>135080</td>
      <td>06123</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.025523485327</td>
      <td>37.5038527779207</td>
    </tr>
    <tr>
      <th>3</th>
      <td>25595331</td>
      <td>광명수산</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q03</td>
      <td>일식/수산물</td>
      <td>Q03A01</td>
      <td>횟집</td>
      <td>I56111</td>
      <td>...</td>
      <td>1168010800101980022009360</td>
      <td></td>
      <td>서울특별시 강남구 봉은사로1길 8</td>
      <td>135010</td>
      <td>06120</td>
      <td></td>
      <td>2</td>
      <td></td>
      <td>127.025061113281</td>
      <td>37.5054888809511</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20578601</td>
      <td>을지로골뱅이</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1168010100106010007022605</td>
      <td></td>
      <td>서울특별시 강남구 봉은사로2길 7</td>
      <td>135080</td>
      <td>06123</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.025676892913</td>
      <td>37.5042324861086</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>




```python
# 13. 업종별 상가업소 조회
divId = 'indsLclsCd'
key = 'Q'
pageNo = 1

df = semas.storeListInUpjong(divId=divId, key=key, pageNo=pageNo)
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
      <th>bizesId</th>
      <th>bizesNm</th>
      <th>brchNm</th>
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>ksicCd</th>
      <th>...</th>
      <th>bldMngNo</th>
      <th>bldNm</th>
      <th>rdnmAdr</th>
      <th>oldZipcd</th>
      <th>newZipcd</th>
      <th>dongNo</th>
      <th>flrNo</th>
      <th>hoNo</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10000982</td>
      <td>이웃사촌</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>4511113700106950005025264</td>
      <td></td>
      <td>전라북도 전주시 완산구 하거마1길 32</td>
      <td>560812</td>
      <td>55088</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.117866794928</td>
      <td>35.7930153087836</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10001526</td>
      <td>이인영선산곱창</td>
      <td>봉곡점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q10</td>
      <td>별식/퓨전요리</td>
      <td>Q10A05</td>
      <td>순대전문점</td>
      <td>I56111</td>
      <td>...</td>
      <td>4719010400102250001023353</td>
      <td></td>
      <td>경상북도 구미시 봉곡동로 21</td>
      <td>730808</td>
      <td>39207</td>
      <td></td>
      <td></td>
      <td></td>
      <td>128.314616501412</td>
      <td>36.152346545776</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10001594</td>
      <td>이자까야류</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q09</td>
      <td>유흥주점</td>
      <td>Q09A01</td>
      <td>호프/맥주</td>
      <td>I56219</td>
      <td>...</td>
      <td>1168010800100040010006184</td>
      <td></td>
      <td>서울특별시 강남구 도산대로8길 9</td>
      <td>135010</td>
      <td>06039</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.022272218413</td>
      <td>37.5163956010583</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10002470</td>
      <td>이조숯불갈비</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A02</td>
      <td>갈비/삼겹살</td>
      <td>I56111</td>
      <td>...</td>
      <td>4884034022101110003003864</td>
      <td></td>
      <td>경상남도 남해군 미조면 미송로 40</td>
      <td>668832</td>
      <td>52442</td>
      <td></td>
      <td></td>
      <td></td>
      <td>128.044802106761</td>
      <td>34.7131927447173</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10002525</td>
      <td>이조식당</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>2723012400109240008000001</td>
      <td></td>
      <td>대구광역시 북구 팔거천동로34길 12-19</td>
      <td>702886</td>
      <td>41427</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>128.55903258871</td>
      <td>35.9374960132905</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>




```python
# 14. 수정일자기준 상가업소 조회
key = '20200101'
indsLclsCd = 'Q'
pageNo = '1'

df = semas.storeListByDate(key=key, pageNo=pageNo)
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
      <th>bizesId</th>
      <th>bizesNm</th>
      <th>brchNm</th>
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>ksicCd</th>
      <th>...</th>
      <th>bldMngNo</th>
      <th>bldNm</th>
      <th>rdnmAdr</th>
      <th>oldZipcd</th>
      <th>newZipcd</th>
      <th>dongNo</th>
      <th>flrNo</th>
      <th>hoNo</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>23036066</td>
      <td>부흥이발관</td>
      <td></td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F01</td>
      <td>이/미용/건강</td>
      <td>F01A04</td>
      <td>남성미용실</td>
      <td>S96111</td>
      <td>...</td>
      <td>2872036022102380072010216</td>
      <td>타잔원룸</td>
      <td>인천광역시 옹진군 영흥면 영흥로 418</td>
      <td>409871</td>
      <td>23119</td>
      <td></td>
      <td></td>
      <td></td>
      <td>126.469408145462</td>
      <td>37.2532290282315</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20520916</td>
      <td>영남냉면</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A19</td>
      <td>냉면집</td>
      <td>I56111</td>
      <td>...</td>
      <td>2614010200102680001002630</td>
      <td></td>
      <td>부산광역시 서구 보수대로201번길 7</td>
      <td>602811</td>
      <td>49216</td>
      <td></td>
      <td></td>
      <td></td>
      <td>129.020440520768</td>
      <td>35.1130940429127</td>
    </tr>
    <tr>
      <th>2</th>
      <td>23025282</td>
      <td>수피부관리</td>
      <td></td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F01</td>
      <td>이/미용/건강</td>
      <td>F01A03</td>
      <td>비만/피부관리</td>
      <td></td>
      <td>...</td>
      <td>3114010400116270032035113</td>
      <td></td>
      <td>울산광역시 남구 문수로369번길 63</td>
      <td>680010</td>
      <td>44649</td>
      <td></td>
      <td></td>
      <td></td>
      <td>129.297183796516</td>
      <td>35.5385410166224</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23173296</td>
      <td>제일미용실</td>
      <td></td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F01</td>
      <td>이/미용/건강</td>
      <td>F01A01</td>
      <td>여성미용실</td>
      <td>S96112</td>
      <td>...</td>
      <td>4790038021102980002025374</td>
      <td>아무데나식당</td>
      <td>경상북도 예천군 용궁면 용궁로 122</td>
      <td>757833</td>
      <td>36858</td>
      <td></td>
      <td></td>
      <td></td>
      <td>128.278426181695</td>
      <td>36.6074086392297</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23102398</td>
      <td>삼영탕이용실</td>
      <td></td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F01</td>
      <td>이/미용/건강</td>
      <td>F01A04</td>
      <td>남성미용실</td>
      <td>S96111</td>
      <td>...</td>
      <td>1129011400100090006046443</td>
      <td>삼영장여관</td>
      <td>서울특별시 성북구 동소문로8길 23</td>
      <td>136044</td>
      <td>02861</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.010307117488</td>
      <td>37.5889112299863</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>




```python
# 21. 상권정보 업종 대분류 조회
df = semas.largeUpjongList()
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
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>stdrDt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>1차산업</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>N</td>
      <td>관광/여가/오락</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>G</td>
      <td>교통/운송</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>V</td>
      <td>국가기관/단체</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>K</td>
      <td>금융</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 22. 상권정보 업종 중분류 조회
indsLclsCd = 'Q'
df = semas.middleUpjongList(indsLclsCd_=indsLclsCd)
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
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>stdrDt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q14</td>
      <td>기타음식업</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q05</td>
      <td>닭/오리요리</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q10</td>
      <td>별식/퓨전요리</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q15</td>
      <td>부페</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q04</td>
      <td>분식</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 23. 상권정보 업종 소분류 조회
indsLclsCd = 'Q'
indsMclsCd = 'Q01'
df = semas.smallUpjongList(indsLclsCd_=indsLclsCd)
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
      <th>indsLclsCd</th>
      <th>indsLclsNm</th>
      <th>indsMclsCd</th>
      <th>indsMclsNm</th>
      <th>indsSclsCd</th>
      <th>indsSclsNm</th>
      <th>stdrDt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q14</td>
      <td>기타음식업</td>
      <td>Q14A02</td>
      <td>고속도로휴게소</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q14</td>
      <td>기타음식업</td>
      <td>Q14A01</td>
      <td>구내식당/자급식음식점</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q14</td>
      <td>기타음식업</td>
      <td>Q14A03</td>
      <td>국도휴게소</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q05</td>
      <td>닭/오리요리</td>
      <td>Q05A02</td>
      <td>닭갈비전문</td>
      <td>2015-12-17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q05</td>
      <td>닭/오리요리</td>
      <td>Q05A10</td>
      <td>닭내장/닭발요리</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>

<br>

## Open Source Project

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}


`PublicDataReader`는 파이썬 오픈소스 프로젝트로 개발하고 있습니다. 크롤링 코드 작성과 같은 데이터 활용의 번거로움을 줄이고자 시작하게 되었습니다. 실제 활용시 발생하는 오류나 개선 사항 등 피드백을 남겨주시면 적극 반영하도록 하겠습니다. 또한, 이 프로젝트에 참여하고자 하시는 분들은 언제든지 아래 이메일 주소로 연락주시거나, [github repository](https://github.com/WooilJeong/PublicDataReader)에 Issue를 남겨주시면 감사하겠습니다.

**E-mail : wooil@kakao.com**  
**[카카오톡 오픈채팅방 링크](https://open.kakao.com/o/gFYXtP2c)**

<br>

### PublicDataReader 관련 글 목록

- [국토교통부 부동산 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [국토교통부 건축물대장 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_03/)
- [소상공인 진흥공단 상가업소 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_02/)
- [데이터 과학을 위한 파이썬 라이브러리 모음](https://wooiljeong.github.io/python/python_library/)

### PublicDataReader Github Repository

- [PublicDataReader](https://github.com/WooilJeong/PublicDataReader)  
