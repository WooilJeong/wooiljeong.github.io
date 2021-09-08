---
title: "PublicDataReader - 건축물대장 데이터 조회하기"
categories: Python
tags: PublicDataReader
header:
  overlay_image: /assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 건축물대장정보 데이터 조회하기

<br>

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}

<br>

### PublicDataReader 관련 글 목록

- [국토교통부 부동산 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [국토교통부 건축물대장 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_02/)
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


### 국토교통부 건축물대장정보 서비스

| **메서드**               | **서비스 명**                |
| ------------------------ | ---------------------------- |
| getBrBasisOulnInfo       | 건축물대장 기본개요 조회     |
| getBrRecapTitleInfo      | 건축물대장 총괄표제부 조회   |
| getBrTitleInfo           | 건축물대장 표제부 조회       |
| getBrFlrOulnInfo         | 건축물대장 층별개요 조회     |
| getBrAtchJibunInfo       | 건축물대장 부속지번 조회     |
| getBrExposPubuseAreaInfo | 건축물대장 전유공용면적 조회 |
| getBrWclfInfo            | 건축물대장 오수정화시설 조회 |
| getBrHsprcInfo           | 건축물대장 주택가격 조회     |
| getBrExposInfo           | 건축물대장 전유부 조회       |
| getBrJijiguInfo          | 건축물대장 지역지구구역 조회 |


<br>

## 개발 배경

[공공 데이터 포털](https://www.data.go.kr/), [서울 열린데이터 광장](https://data.seoul.go.kr/) 등의 공공기관에서는 다양한 공공 데이터 조회를 위한 오픈 API 서비스를 제공하고 있습니다. 그러나 개발자가 아닌 데이터 분석가의 입장에서 웹 서비스를 이용해 데이터를 분석하기에는 약간의 어려움이 존재합니다. 데이터 수집을 위해서는 웹 서비스와 크롤링 기술에 대한 이해가 필요합니다. PublicDataReader는 웹 관련 사전지식이 부족한 데이터 분석가라도 손 쉽게 원하는 공공 데이터를 수집하고, 분석할 수 있도록 돕기 위해 개발되었습니다. 기관에서 발급받은 서비스키만 있다면 간단하게 원하는 데이터를 조회할 수 있습니다. 단 코드 3줄(라이브러리 불러오기, API 서비스키 등록하기, 데이터 수집하기)만으로 손쉽게 공공 데이터를 `Pandas`의 `DataFrame`형태로 불러올 수 있습니다.

<br>

'[건축물대장정보](https://www.data.go.kr/data/15044713/openapi.do)' Open API 서비스를 이용해 데이터를 조회하는 방법에 대해 알아보겠습니다.

<br>

## 테이블 관계도

![PNG](/assets/img/common/building_table_erd.png){: .align-center}

<br>

## 테이블 설명

구분 | 설명
:-----|:-------
기본개요|건축물대장 대장 종류별 관리
총괄표제부|총괄표제부 관리
표제부|건축물의 표제부를 관리
층별개요|건축물의 층별개요
전유부|건축물의 호별개요
전유공용면적|호별개요의 전유/공용부분
오수정화시설|건축물의 오수정화시설
지역지구구역|건축물의 총괄/동 단위 지역지구구역
부속지번|건축물의 관련지번(대표지번 외의 지번)
주택가격|건축물대장 공동주택가격 정보

※ 건축물 상위 : 총괄표제부 > 표제부 > 전유부

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
# 라이브러리 임포트 및 버전 확인하기
import PublicDataReader as pdr
print(pdr.__version__)
```

    
    >>> PublicDataReader Version : 2021.4.12
    
    - Author : Wooil Jeong
    - E-mail : wooil@kakao.com
    - Github : https://github.com/WooilJeong/PublicDataReader
    - Blog : https://wooiljeong.github.io
    

<br>

## 서비스 키로 데이터 수집기 만들기

다음과 같이 소상공인 상가업소 정보를 수집할 `molit` 인스턴스를 만들어줍니다. 



```python
# 공공 데이터 포털 Open API 서비스 인증키 입력하기
serviceKey = "OPEN API SERVICE KEY HERE"

# 국토교통부(molit) 건축물대장정보 서비스 Open API 인스턴스 생성하기
molit = pdr.Building(serviceKey)
```

    >>> 건축물대장 기본개요 조회 서비스가 정상 작동합니다.
    >>> 건축물대장 총괄표제부 조회 서비스가 정상 작동합니다.
    >>> 건축물대장 표제부 조회 서비스가 정상 작동합니다.
    >>> 건축물대장 층별개요 조회 서비스가 정상 작동합니다.
    >>> 건축물대장 부속지번 조회 서비스가 정상 작동합니다.
    >>> 건축물대장 전유공용면적 조회 서비스가 정상 작동합니다.
    >>> 건축물대장 오수정화시설 조회 서비스가 정상 작동합니다.
    >>> 건축물대장 주택가격 조회 서비스가 정상 작동합니다.
    >>> 건축물대장 전유부 조회 서비스가 정상 작동합니다.
    >>> 건축물대장 지역지구구역 조회 서비스가 정상 작동합니다.
    
<br>

## 건축물대장 조회 파라미터 입력하기

[이곳](https://www.mois.go.kr/frt/bbs/type001/commonSelectBoardArticle.do?bbsId=BBSMSTR_000000000052&nttId=85215)에서 첨부파일 중 `(수정)jscode20210705.zip`을 다운로드 받아 압축 해제 후 `KIKcd_B.20210705.xlsx`을 열면 법정동코드를 확인할 수 있습니다. 법정동코드는 10자리 숫자로 이루어져있고, 앞에서부터 5자리는 `시군구코드`를 이후 5자리는 `읍면동코드`를 의미합니다. 건축물대장 데이터 조회 시 활용할 수 있습니다. `본번`과 `부번`은 조회하고자 하는 건축물의 지번을 의미합니다. 각각 아래와 같이 제로패딩을 해주어야합니다. 예를 들어, `성남시 분당구 백현동 530번지`에 해당하는 건축물에 대한 정보를 조회하려면 다음과 같이 입력하면 됩니다.


```python
# 시군구코드(5)
sigunguCd = '41135'
# 읍면동코드(5)
bjdongCd = '11000'
# 본번(4)
bun = '530'.zfill(4) 
# 부번(4)
ji = ''.zfill(4)

print(f"""
입력 파라미터
- 시군구코드: {sigunguCd}
- 읍면동코드: {bjdongCd}
- 본번: {bun}
- 부번: {ji}
""")
```

    
    입력 파라미터
    - 시군구코드: 41135
    - 읍면동코드: 11000
    - 본번: 0530
    - 부번: 0000
    
<br>

## Operation 1 - 기본개요


```python
df1 = molit.getBrBasisOulnInfo(
    sigunguCd_=sigunguCd,
    bjdongCd_=bjdongCd,
    platGbCd_="0",
    bun_=bun,
    ji_=ji,
    startDate_="",
    endDate_=""
)
df1 = molit.ChangeCols(df1, "getBrBasisOulnInfo")
df1.head(2)
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
      <th>법정동코드</th>
      <th>건물명</th>
      <th>블록</th>
      <th>번</th>
      <th>외필지수</th>
      <th>생성일자</th>
      <th>구역코드</th>
      <th>구역코드명</th>
      <th>지</th>
      <th>지구코드</th>
      <th>...</th>
      <th>도로명대지위치</th>
      <th>대지구분코드</th>
      <th>대지위치</th>
      <th>대장구분코드</th>
      <th>대장구분코드명</th>
      <th>대장종류코드</th>
      <th>대장종류코드명</th>
      <th>순번</th>
      <th>시군구코드</th>
      <th>특수지명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>0</td>
      <td>20171205</td>
      <td></td>
      <td></td>
      <td>0000</td>
      <td></td>
      <td>...</td>
      <td>경기도 성남시 분당구 판교역로 145</td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>2</td>
      <td>집합</td>
      <td>4</td>
      <td>전유부</td>
      <td>1</td>
      <td>41135</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>0</td>
      <td>20171205</td>
      <td></td>
      <td></td>
      <td>0000</td>
      <td></td>
      <td>...</td>
      <td>경기도 성남시 분당구 판교역로 145</td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>2</td>
      <td>집합</td>
      <td>4</td>
      <td>전유부</td>
      <td>2</td>
      <td>41135</td>
      <td></td>
    </tr>
  </tbody>
</table>
<p>2 rows × 31 columns</p>
</div>

<br>


##  Operation 2 - 총괄표제부


```python
df2 = molit.getBrRecapTitleInfo(
    sigunguCd_=sigunguCd,
    bjdongCd_=bjdongCd,
    platGbCd_="0",
    bun_=bun,
    ji_=ji,
    startDate_="",
    endDate_=""
)
df2 = molit.ChangeCols(df2, "getBrRecapTitleInfo")
df2.head(2)
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
      <th>건축면적</th>
      <th>부속건축물면적</th>
      <th>부속건축물수</th>
      <th>건폐율</th>
      <th>법정동코드</th>
      <th>건물명</th>
      <th>블록</th>
      <th>번</th>
      <th>외필지수</th>
      <th>생성일자</th>
      <th>...</th>
      <th>대장종류코드명</th>
      <th>순번</th>
      <th>시군구코드</th>
      <th>특수지명</th>
      <th>착공일</th>
      <th>연면적</th>
      <th>총주차수</th>
      <th>사용승인일</th>
      <th>용적률</th>
      <th>용적률산정연면적</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12010.77</td>
      <td>39960.514</td>
      <td>2</td>
      <td>35.29</td>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>0</td>
      <td>20210723</td>
      <td>...</td>
      <td>총괄표제부</td>
      <td>1</td>
      <td>41135</td>
      <td></td>
      <td>20130423</td>
      <td>191006.75</td>
      <td>1448</td>
      <td>20151124</td>
      <td>349.61</td>
      <td>118980.331</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 65 columns</p>
</div>

<br>

##  Operation 3 - 표제부


```python
df3 = molit.getBrTitleInfo(
    sigunguCd_=sigunguCd,
    bjdongCd_=bjdongCd,
    platGbCd_="0",
    bun_=bun,
    ji_=ji,
    startDate_="",
    endDate_=""
)
df3 = molit.ChangeCols(df3, "getBrTitleInfo")
df3.head(2)
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
      <th>건축면적</th>
      <th>부속건축물면적</th>
      <th>부속건축물수</th>
      <th>건폐율</th>
      <th>법정동코드</th>
      <th>건물명</th>
      <th>블록</th>
      <th>번</th>
      <th>외필지수</th>
      <th>생성일자</th>
      <th>...</th>
      <th>특수지명</th>
      <th>착공일</th>
      <th>구조코드</th>
      <th>구조코드명</th>
      <th>연면적</th>
      <th>총동연면적</th>
      <th>지하층수</th>
      <th>사용승인일</th>
      <th>용적률</th>
      <th>용적률산정연면적</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1159.22</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>0</td>
      <td>20180831</td>
      <td>...</td>
      <td></td>
      <td>20130423</td>
      <td>21</td>
      <td>철근콘크리트구조</td>
      <td>17290.407</td>
      <td>17290.407</td>
      <td>0</td>
      <td>20151124</td>
      <td>0</td>
      <td>17290.407</td>
    </tr>
    <tr>
      <th>1</th>
      <td>214.44</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>0</td>
      <td>20200427</td>
      <td>...</td>
      <td></td>
      <td>20130423</td>
      <td>21</td>
      <td>철근콘크리트구조</td>
      <td>201.817</td>
      <td>201.817</td>
      <td>0</td>
      <td>20151124</td>
      <td>0</td>
      <td>201.817</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 78 columns</p>
</div>

<br>


##  Operation 4 - 층별개요


```python
df4 = molit.getBrFlrOulnInfo(
    sigunguCd_=sigunguCd,
    bjdongCd_=bjdongCd,
    platGbCd_="0",
    bun_=bun,
    ji_=ji,
    startDate_="",
    endDate_=""
)
df4 = molit.ChangeCols(df4, "getBrFlrOulnInfo")
df4.head(2)
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
      <th>면적</th>
      <th>면적제외여부</th>
      <th>법정동코드</th>
      <th>건물명</th>
      <th>블록</th>
      <th>번</th>
      <th>생성일자</th>
      <th>동명칭</th>
      <th>기타용도</th>
      <th>기타구조</th>
      <th>...</th>
      <th>새주소부번</th>
      <th>새주소지상지하코드</th>
      <th>도로명대지위치</th>
      <th>대지구분코드</th>
      <th>대지위치</th>
      <th>순번</th>
      <th>시군구코드</th>
      <th>특수지명</th>
      <th>구조코드</th>
      <th>구조코드명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>915.354</td>
      <td>0</td>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>20180831</td>
      <td>205동</td>
      <td>아파트</td>
      <td>철근콘크리트구조</td>
      <td>...</td>
      <td>0.0</td>
      <td>0</td>
      <td>경기도 성남시 분당구 판교역로 145</td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>1</td>
      <td>41135</td>
      <td></td>
      <td>21</td>
      <td>철근콘크리트구조</td>
    </tr>
    <tr>
      <th>1</th>
      <td>915.354</td>
      <td>0</td>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>20180831</td>
      <td>205동</td>
      <td>아파트</td>
      <td>철근콘크리트구조</td>
      <td>...</td>
      <td>0.0</td>
      <td>0</td>
      <td>경기도 성남시 분당구 판교역로 145</td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>2</td>
      <td>41135</td>
      <td></td>
      <td>21</td>
      <td>철근콘크리트구조</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 34 columns</p>
</div>

<br>

##  Operation 5 - 부속지번


```python
df5 = molit.getBrAtchJibunInfo(
    sigunguCd_=sigunguCd,
    bjdongCd_=bjdongCd,
    platGbCd_="0",
    bun_=bun,
    ji_=ji,
    startDate_="",
    endDate_=""
)
df5 = molit.ChangeCols(df5, "getBrAtchJibunInfo")
df5.head(2)
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
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>

<br>


##  Operation 6 - 전유공용면적


```python
df6 = molit.getBrExposPubuseAreaInfo(
    sigunguCd_ = sigunguCd, 
    bjdongCd_ = bjdongCd, 
    platGbCd_ = "0", 
    bun_ = bun, 
    ji_ = ji, 
    startDate_ = "", 
    endDate_ = "", 
    dongNm_ = "", 
    hoNm_ = ""
)
df6 = molit.ChangeCols(df6, "getBrExposPubuseAreaInfo")
df6.head(2)
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
      <th>면적</th>
      <th>법정동코드</th>
      <th>건물명</th>
      <th>블록</th>
      <th>번</th>
      <th>생성일자</th>
      <th>동명칭</th>
      <th>기타용도</th>
      <th>기타구조</th>
      <th>전유공용구분코드</th>
      <th>...</th>
      <th>대지위치</th>
      <th>대장구분코드</th>
      <th>대장구분코드명</th>
      <th>대장종류코드</th>
      <th>대장종류코드명</th>
      <th>순번</th>
      <th>시군구코드</th>
      <th>특수지명</th>
      <th>구조코드</th>
      <th>구조코드명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>110.255</td>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>20181001</td>
      <td>203동</td>
      <td>아파트</td>
      <td>철근콘크리트구조</td>
      <td>1</td>
      <td>...</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>2</td>
      <td>집합</td>
      <td>4</td>
      <td>전유부</td>
      <td>1</td>
      <td>41135</td>
      <td></td>
      <td>21</td>
      <td>철근콘크리트구조</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0406</td>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>20181001</td>
      <td>203동</td>
      <td>경비실</td>
      <td>철근콘크리트구조</td>
      <td>2</td>
      <td>...</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>2</td>
      <td>집합</td>
      <td>4</td>
      <td>전유부</td>
      <td>2</td>
      <td>41135</td>
      <td></td>
      <td>21</td>
      <td>철근콘크리트구조</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 40 columns</p>
</div>

<br>

##  Operation 7 - 오수정화시설


```python
df7 = molit.getBrWclfInfo(
    sigunguCd_=sigunguCd,
    bjdongCd_=bjdongCd,
    platGbCd_="0",
    bun_=bun,
    ji_=ji,
    startDate_="",
    endDate_=""
)
df7 = molit.ChangeCols(df7, "getBrWclfInfo")
df7.head(2)
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
      <th>법정동코드</th>
      <th>건물명</th>
      <th>블록</th>
      <th>번</th>
      <th>용량(루베)</th>
      <th>용량(인용)</th>
      <th>생성일자</th>
      <th>기타형식</th>
      <th>지</th>
      <th>로트</th>
      <th>...</th>
      <th>대지위치</th>
      <th>대장구분코드</th>
      <th>대장구분코드명</th>
      <th>대장종류코드</th>
      <th>대장종류코드명</th>
      <th>순번</th>
      <th>시군구코드</th>
      <th>특수지명</th>
      <th>단위구분코드</th>
      <th>단위구분코드명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>20210723</td>
      <td>하수종말처리장연결</td>
      <td>0000</td>
      <td></td>
      <td>...</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>2</td>
      <td>집합</td>
      <td>1</td>
      <td>총괄표제부</td>
      <td>1</td>
      <td>41135</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
<p>1 rows × 30 columns</p>
</div>

<br>

##  Operation 8 - 주택가격


```python
df8 = molit.getBrHsprcInfo(
    sigunguCd_=sigunguCd,
    bjdongCd_=bjdongCd,
    platGbCd_="0",
    bun_=bun,
    ji_=ji,
    startDate_="",
    endDate_=""
)
df8 = molit.ChangeCols(df8, "getBrHsprcInfo")
df8.head(2)
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
      <th>법정동코드</th>
      <th>건물명</th>
      <th>블록</th>
      <th>번</th>
      <th>외필지수</th>
      <th>생성일자</th>
      <th>주택가격</th>
      <th>지</th>
      <th>로트</th>
      <th>관리건축물대장PK</th>
      <th>...</th>
      <th>도로명대지위치</th>
      <th>대지구분코드</th>
      <th>대지위치</th>
      <th>대장구분코드</th>
      <th>대장구분코드명</th>
      <th>대장종류코드</th>
      <th>대장종류코드명</th>
      <th>순번</th>
      <th>시군구코드</th>
      <th>특수지명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>0</td>
      <td>20171205</td>
      <td>864000000</td>
      <td>0000</td>
      <td></td>
      <td>41135-100263634</td>
      <td>...</td>
      <td>경기도 성남시 분당구 판교역로 145</td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>2</td>
      <td>집합</td>
      <td>4</td>
      <td>전유부</td>
      <td>1</td>
      <td>41135</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>0</td>
      <td>20171205</td>
      <td>896000000</td>
      <td>0000</td>
      <td></td>
      <td>41135-100263661</td>
      <td>...</td>
      <td>경기도 성남시 분당구 판교역로 145</td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>2</td>
      <td>집합</td>
      <td>4</td>
      <td>전유부</td>
      <td>2</td>
      <td>41135</td>
      <td></td>
    </tr>
  </tbody>
</table>
<p>2 rows × 25 columns</p>
</div>

<br>

##  Operation 9 - 전유부


```python
df9 = molit.getBrExposInfo(
    sigunguCd_=sigunguCd,
    bjdongCd_=bjdongCd,
    platGbCd_="0",
    bun_=bun,
    ji_=ji,
    startDate_="",
    endDate_=""
)
df9 = molit.ChangeCols(df9, "getBrExposInfo")
df9.head(2)
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
      <th>법정동코드</th>
      <th>건물명</th>
      <th>블록</th>
      <th>번</th>
      <th>생성일자</th>
      <th>동명칭</th>
      <th>층구분코드</th>
      <th>층구분코드명</th>
      <th>층번호</th>
      <th>호명칭</th>
      <th>...</th>
      <th>도로명대지위치</th>
      <th>대지구분코드</th>
      <th>대지위치</th>
      <th>대장구분코드</th>
      <th>대장구분코드명</th>
      <th>대장종류코드</th>
      <th>대장종류코드명</th>
      <th>순번</th>
      <th>시군구코드</th>
      <th>특수지명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>20171205</td>
      <td>203동</td>
      <td>20</td>
      <td>지상</td>
      <td>4.0</td>
      <td>404</td>
      <td>...</td>
      <td>경기도 성남시 분당구 판교역로 145</td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>2</td>
      <td>집합</td>
      <td>4</td>
      <td>전유부</td>
      <td>1</td>
      <td>41135</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>11000</td>
      <td>알파리움</td>
      <td></td>
      <td>0530</td>
      <td>20171205</td>
      <td>204동</td>
      <td>20</td>
      <td>지상</td>
      <td>14.0</td>
      <td>1402</td>
      <td>...</td>
      <td>경기도 성남시 분당구 판교역로 145</td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>2</td>
      <td>집합</td>
      <td>4</td>
      <td>전유부</td>
      <td>2</td>
      <td>41135</td>
      <td></td>
    </tr>
  </tbody>
</table>
<p>2 rows × 28 columns</p>
</div>

<br>

##  Operation 10 - 지역지구구역


```python
df10 = molit.getBrJijiguInfo(
    sigunguCd_=sigunguCd,
    bjdongCd_=bjdongCd,
    platGbCd_="0",
    bun_=bun,
    ji_=ji,
    startDate_="",
    endDate_=""
)
df10 = molit.ChangeCols(df10, "getBrJijiguInfo")
df10.head(2)
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
      <th>법정동코드</th>
      <th>블록</th>
      <th>번</th>
      <th>생성일자</th>
      <th>기타지역지구구역</th>
      <th>지</th>
      <th>지역지구구역코드</th>
      <th>지역지구구역코드명</th>
      <th>지역지구구역구분코드</th>
      <th>지역지구구역구분코드명</th>
      <th>로트</th>
      <th>관리건축물대장PK</th>
      <th>도로명대지위치</th>
      <th>대지구분코드</th>
      <th>대지위치</th>
      <th>대표여부</th>
      <th>순번</th>
      <th>시군구코드</th>
      <th>특수지명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11000</td>
      <td></td>
      <td>0530</td>
      <td>20210723</td>
      <td>제1종지구단위계획구역</td>
      <td>0000</td>
      <td>UQQ310</td>
      <td>제1종지구단위계획구역</td>
      <td>3</td>
      <td>용도구역코드</td>
      <td></td>
      <td>41135-100263367</td>
      <td></td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>1</td>
      <td>1</td>
      <td>41135</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>11000</td>
      <td></td>
      <td>0530</td>
      <td>20210723</td>
      <td>택지개발지구</td>
      <td>0000</td>
      <td>160</td>
      <td>택지개발지구</td>
      <td>2</td>
      <td>용도지구코드</td>
      <td></td>
      <td>41135-100263367</td>
      <td></td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 530번지</td>
      <td>1</td>
      <td>2</td>
      <td>41135</td>
      <td></td>
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
- [국토교통부 건축물대장 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_02/)
- [소상공인 진흥공단 상가업소 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_02/)
- [데이터 과학을 위한 파이썬 라이브러리 모음](https://wooiljeong.github.io/python/python_library/)

### PublicDataReader Github Repository

- [PublicDataReader](https://github.com/WooilJeong/PublicDataReader)  
