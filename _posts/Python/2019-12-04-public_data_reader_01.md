---
title: "PublicDataReader - 부동산 데이터 수집하기"
categories: Python
tags: PublicDataReader
header:
  overlay_image: /assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 국토교통부 아파트매매 실거래 데이터 수집하기

<br>

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}

<br>

### PublicDataReader 관련 글 목록

- [국토교통부 부동산 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [데이터 과학을 위한 파이썬 라이브러리 모음](https://wooiljeong.github.io/python/python_library/)

### PublicDataReader Github Repository

- [PublicDataReader](https://github.com/WooilJeong/PublicDataReader)  


## 소개

**PublicDataReader**는 [공공데이터포털](https://data.go.kr)에서 제공하는 OpenAPI 서비스를 Python으로 쉽게 이용할 수 있도록 도와주는 **데이터 수집 라이브러리**입니다. 2020년 12월 현재 [국토교통부 실거래가 정보](https://www.data.go.kr/dataset/3050988/openapi.do) 조회 서비스에 대한 인터페이스를 제공하고 있습니다. 추후 수요가 높은 Open API 서비스에 대한 인터페이스도 지속적으로 업데이트할 예정입니다.

## 사용 가능한 서비스(0.1.2 Version 기준)

![](https://img.shields.io/badge/PublicDataReader-0.1.2-blue.svg)  

**메서드**              | **서비스 명**
---------------------- | --------------------
CondeFinder | 지역코드 조회
DataCollector | 서비스/기간 별 데이터 조회
AptTrade | 아파트매매 실거래자료 조회
AptTradeDetail | 아파트매매 실거래 상세 자료 조회
AptRent | 아파트 전월세 자료 조회
AptOwnership | 아파트 분양권전매 신고 자료 조회
OffiTrade | 오피스텔 매매 신고 조회
OffiRent | 오피스텔 전월세 신고 조회
RHTrade | 연립다세대 매매 실거래자료 조회
RHRent | 연립다세대 전월세 실거래자료 조회
DHTrade | 단독/다가구 매매 실거래 조회
DHRent | 단독/다가구 전월세 자료 조회
LandTrade | 토지 매매 신고 조회
BizTrade | 상업업무용 부동산 매매 신고 자료 조회


<br>

## 배경

[공공 데이터 포털](https://www.data.go.kr/)에서는 2019년 12월 4일 현재 총 3,254건에 이르는 다양한 오픈 API 서비스를 제공하고 있습니다. 저도 부동산이나 상권 분석을 할 때, 오픈 API 서비스를 이용해 데이터를 수집하여 분석에 활용하고 있습니다. 다만, 서비스를 이용하려면 `Requests`, `BeautifulSoup` 등의 파이썬 라이브러리를 통해 크롤링하는 코드를 작성해야한다는 번거로움이 있습니다.  

<br>

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_01.png){: .align-center}

이런 수고로움을 줄이기 위해, 단 코드 3줄(라이브러리 불러오기, API 서비스키 등록하기, 데이터 수집하기)만으로 손쉽게 공공 데이터 포털의 데이터를 `Pandas`의 `DataFrame`형태로 가져올 수 있는 라이브러리인 `PublicDataReader`를 만들어 보았습니다.  

<br>

그럼 지금부터 '[국토교통부 실거래가 정보](https://www.data.go.kr/dataset/3050988/openapi.do)' 중 '**아파트매매 실거래자료**' 데이터를 쉽게 수집하는 방법에 대해 소개해드리겠습니다. 아파트 전월세 자료, 오피스텔 매매 신고 조회 등 위에 소개된 서비스를 이용하는 방식도 이와 동일합니다.

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_02.jpg){: .align-center}

<br>

## Python 라이브러리 PublicDataReader 설치하기

터미널에서 `pip`로 다음과 같이 `PublicDataReader` 를 설치합니다.
```bash
pip install PublicDataReader
```

`PublicDataReader`는 API기능 추가 등의 업데이트가 있을 수 있습니다. 다음과 같이 최신 버전으로 업그레이드합니다. **(글 작성 시점 기준 최신 버전은 0.1.2 입니다.)**
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

    
    >>> PublicDataReader Version : 0.1.2
    
    - Author : Wooil Jeong
    - E-mail : wooil@kakao.com
    - Github : https://github.com/WooilJeong/PublicDataReader
    - Blog : https://wooiljeong.github.io
    

<br>


## 서비스 키로 아파트매매 실거래 자료 수집기 만들기

다음과 같이 아파트매매 실거래 자료를 수집할 `AptTrade` 수집기를 만들어줍니다. 위에서 정의한 `serviceKey`를 다음과 같이 입력해줍니다.


```python
serviceKey="OPEN API SERVICE KEY HERE"
```

<br>

## 서비스 키로 아파트매매 실거래 자료 수집기 만들기

다음과 같이 아파트매매 실거래 자료를 수집할 `molit` 인스턴스를 만들어줍니다. 위에서 정의한 `serviceKey`를 다음과 같이 입력해줍니다. 국토부에서 제공하는 실거래가 조회 서비스들을 모두 정상적으로 신청 완료한 상태라면 아래와 같은 메시지를 확인할 수 있습니다.

```python
# 국토교통부 실거래가 Open API 인스턴스 생성
molit = pdr.Transaction(serviceKey)
```
    >>> 아파트매매 실거래자료 조회 서비스가 정상 작동합니다.
    >>> 아파트매매 실거래 상세 자료 조회 서비스가 정상 작동합니다.
    >>> 아파트 전월세 자료 조회 서비스가 정상 작동합니다.
    >>> 아파트 분양권전매 신고 자료 조회 서비스가 정상 작동합니다.
    >>> 오피스텔 매매 신고 조회 서비스가 정상 작동합니다.
    >>> 오피스텔 전월세 신고 조회 서비스가 정상 작동합니다.
    >>> 연립다세대 매매 실거래자료 조회 서비스가 정상 작동합니다.
    >>> 연립다세대 전월세 실거래자료 조회 서비스가 정상 작동합니다.
    >>> 단독/다가구 매매 실거래 조회 서비스가 정상 작동합니다.
    >>> 단독/다가구 전월세 자료 조회 서비스가 정상 작동합니다.
    >>> 토지 매매 신고 조회 서비스가 정상 작동합니다.
    >>> 상업업무용 부동산 매매 신고 자료 조회 서비스가 정상 작동합니다.



<br>

## 주소 검색을 통해 지역코드 검색하기

아파트매매 실거래 자료 Open API 기술 문서에 따르면 조회하고자 하는 지역의 코드는 본래 법정동 코드 10자리 중 앞 5자리를 사용한다고 정의되어 있습니다. 이 부분도 직접 크롤링 코드를 작성할 때 꽤 번거로운 부분이었는데, 지역명을 문자열로 입력받아 지역코드를 반환하는 함수를 만들었습니다. 즉, 조회하고자 하는 지역의 이름을 검색하고, 출력된 '법정구코드'를 아래 데이터 수집 과정에서 활용하시면 됩니다.


```python
# 지역코드 조회
bdongName = '분당구'
codeResult = molit.CodeFinder(bdongName)
codeResult.head(1)
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
  </tbody>
</table>
</div>


<br>


## 특정 월의 아파트매매 실거래 데이터 불러오기

다음과 같이 `molit.AptTrade()`에 `지역코드`와 `계약월`을 입력해줍니다. 결과는 `Pandas`의 `DataFrame` 형태로 반환됩니다.


```python
df = molit.AptTrade(41135, 202004)
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
      <td>2020-04-04</td>
      <td>무지개(5단지)(청구)</td>
      <td>221</td>
      <td>58.49</td>
      <td>10</td>
      <td>1995</td>
      <td>59600</td>
    </tr>
    <tr>
      <th>1</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2020-04-06</td>
      <td>무지개(3단지)(신한)</td>
      <td>201</td>
      <td>53.46</td>
      <td>1</td>
      <td>1996</td>
      <td>38000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>41135</td>
      <td>금곡동</td>
      <td>2020-04-06</td>
      <td>청솔마을(대원)</td>
      <td>141</td>
      <td>50.4</td>
      <td>1</td>
      <td>1994</td>
      <td>54700</td>
    </tr>
    <tr>
      <th>3</th>
      <td>41135</td>
      <td>분당동</td>
      <td>2020-04-04</td>
      <td>샛별마을(우방)</td>
      <td>38</td>
      <td>57.28</td>
      <td>3</td>
      <td>1994</td>
      <td>64000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41135</td>
      <td>분당동</td>
      <td>2020-04-08</td>
      <td>샛별마을(동성)</td>
      <td>35</td>
      <td>69.39</td>
      <td>3</td>
      <td>1992</td>
      <td>73900</td>
    </tr>
  </tbody>
</table>
</div>

<br>

## 특정 기간 동안의 아파트매매 실거래 데이터 불러오기

`DataCollector` 메서드를 사용하면, 특정 월 뿐 아니라 특정 기간 동안의 데이터를 한 번에 조회할 수 있습니다. `서비스 메서드`,`지역코드`, `시작월`, `종료월` 순으로 입력하면 됩니다. 위의 `AptTrade` 메서드 호출 결과와 같이 `Pandas`의 `DataFrame` 형태로 반환됩니다.



```python
df_sum = molit.DataCollector(molit.AptTrade, 41135, 202001, 202003)
df_sum.head()
```

    >>> LAWD_CD : 41135 DEAL_YMD : 202001
    >>> LAWD_CD : 41135 DEAL_YMD : 202002
    >>> LAWD_CD : 41135 DEAL_YMD : 202003
    

<br>


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
      <td>2020-01-03</td>
      <td>까치마을(3단지)(신원)</td>
      <td>66</td>
      <td>162.87</td>
      <td>17</td>
      <td>1995</td>
      <td>130000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2020-01-03</td>
      <td>무지개(10단지)(삼성)</td>
      <td>222</td>
      <td>101.82</td>
      <td>15</td>
      <td>1996</td>
      <td>74000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2020-01-03</td>
      <td>까치마을(2단지)(주공)</td>
      <td>88</td>
      <td>49.36</td>
      <td>8</td>
      <td>1995</td>
      <td>53200</td>
    </tr>
    <tr>
      <th>3</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2020-01-04</td>
      <td>무지개(10단지)(건영)</td>
      <td>222</td>
      <td>134.82</td>
      <td>14</td>
      <td>1996</td>
      <td>83500</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41135</td>
      <td>구미동</td>
      <td>2020-01-04</td>
      <td>무지개(8단지)(제일)</td>
      <td>243</td>
      <td>101.91</td>
      <td>10</td>
      <td>1995</td>
      <td>71000</td>
    </tr>
  </tbody>
</table>
</div>

<br>

## 전체 코드 정리


```python
import PublicDataReader as pdr

# Open API 서비스 키 설정
serviceKey = "OPEN API SERVICE KEY HERE"

# 국토교통부 실거래가 Open API 인스턴스 생성
molit = pdr.Transaction(serviceKey)

# 지역코드 조회
bdongName = '분당구'
codeResult = molit.CodeFinder(bdongName)
codeResult.head(1)

# 특정 월 아파트매매 실거래 자료 조회
df = molit.AptTrade(41135, 202004)

# 특정 기간 아파트매매 실거래 자료 조회
df_sum = molit.DataCollector(molit.AptTrade, 41135, 202001, 202003)
```

아파트 매매실거래 자료 조회 외에 기타 실거래 데이터를 조회하고자 할 경우에는 위 코드에서 `molit.AptTrade` 대신, 위 소개 부분에 설명된 다른 메서드로 변경하여 동일하게 사용하면 됩니다.

<br>
    

## Colab으로 테스트해보기

아래 링크에 접속하시면 구글 코랩 환경에서 예제 코드를 테스트해볼 수 있습니다. 아파트 매매 실거래자료 조회 뿐만 아니라 다른 서비스들도 테스트해보실 수 있습니다.

[Colab Test Code](https://disq.us/url?url=https%3A%2F%2Fdrive.google.com%2Fopen%3Fid%3D1pFtMFr_te9T_maHjee8Sd8Yq9rTrE-4F%3AUps3RW1D7_xrpDTq3k2p_YQMVXk&cuid=5671700)

<br>

## Open Source Project

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}


`PublicDataReader`는 파이썬 오픈소스 프로젝트로 개발하고 있습니다. 크롤링 코드 작성과 같은 데이터 활용의 번거로움을 줄이고자 시작하게 되었습니다. 실제 활용시 발생하는 오류나 개선 사항 등 피드백을 남겨주시면 적극 반영하도록 하겠습니다. 또한, 이 프로젝트에 참여하고자 하시는 분들은 언제든지 아래 이메일 주소로 연락주시면 감사하겠습니다.

**E-mail : wooil@kakao.com**

<br>

### PublicDataReader 관련 글 목록

- [국토교통부 부동산 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [데이터 과학을 위한 파이썬 라이브러리 모음](https://wooiljeong.github.io/python/python_library/)

### PublicDataReader Github Repository

- [PublicDataReader](https://github.com/WooilJeong/PublicDataReader)  
