---
title: "PublicDataReader - 부동산 실거래가 조회하기"
categories: Python
tags: PublicDataReader
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 국토교통부 부동산 실거래 데이터 조회하기

<br>

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}

<br>

[![Github Badge](https://img.shields.io/github/followers/Github?label=PublicDataReader&logo=github&style=plastic)](https://github.com/WooilJeong/PublicDataReader)

<br>

### PublicDataReader 관련 글 목록

- [PublicDataReader - 부동산 실거래가 조회하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [PublicDataReader - 건축물대장 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_03/)
- [PublicDataReader - 상가업소 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_02/)

<br>

## 국토교통부 실거래가 정보 조회 서비스

- [국토교통부 실거래가 정보](https://www.data.go.kr/dataset/3050988/openapi.do)

| **서비스명**                          | **상품유형** | **거래유형** |
| ------------------------------------- | ------------ | ------------ |
| 아파트매매 실거래 상세 자료 조회      | 아파트       | 매매         |
| 아파트 전월세 자료 조회               | 아파트       | 전월세       |
| 아파트 분양권전매 신고 자료 조회      | 분양입주권   | 매매         |
| 오피스텔 매매 신고 조회               | 오피스텔     | 매매         |
| 오피스텔 전월세 신고 조회             | 오피스텔     | 전월세       |
| 연립다세대 매매 실거래자료 조회       | 연립다세대   | 매매         |
| 연립다세대 전월세 실거래자료 조회     | 연립다세대   | 전월세       |
| 단독/다가구 매매 실거래 조회          | 단독다가구   | 매매         |
| 단독/다가구 전월세 자료 조회          | 단독다가구   | 전월세       |
| 토지 매매 신고 조회                   | 토지         | 매매         |
| 상업업무용 부동산 매매 신고 자료 조회 | 상업업무용   | 매매         |

<br>

## Python 라이브러리 PublicDataReader 설치하기

터미널에서 `pip`로 다음과 같이 `PublicDataReader`의 최신버전을 설치합니다.

```bash
pip install --upgrade PublicDataReader
```

<br>


## 설치한 라이브러리를 임포트하고 관련 정보 확인하기

```python
# 1. 라이브러리 임포트하기
import PublicDataReader as pdr
print(pdr.__version__)
print(pdr.__info__)
```

    2021.11.16
    - Author : Wooil Jeong
    - E-mail : wooil@kakao.com
    - Github : https://github.com/WooilJeong/PublicDataReader
    - Blog : https://wooiljeong.github.io
   
<br>

## OpenAPI 서비스 키 입력하기

공공 데이터 포털에서 발급받은 서비스 키를 복사하여 다음과 같이 `serviceKey`에 문자열로 할당해줍니다. OpenAPI 서비스 키 발급 방법에 대해 궁금하신 분들은 구글에 '**공공 데이터 포털 Open API 사용법**'을 검색하시면 여러 문서들을 참조하실 수 있습니다. 검색 후 가장 상단에 있는 [이 블로그](https://jeong-pro.tistory.com/143)를 참조하셔도 됩니다.


<br>

```python
# 2. 공공 데이터 포털 OpenAPI 서비스 인증키 입력하기
serviceKey = "공공 데이터 포털에서 발급받은 서비스 키"
```

<br>

## 데이터 조회 세션 만들기

다음과 같이 발급받은 `serviceKey` 값을 이용해 부동산 실거래가 데이터를 조회할 `ts` 세션을 만들어줍니다. `debug`의 값을 `True`로 입력하면 아래와 같은 메시지를 확인할 수 있습니다. 메시지 출력을 원치 않는 경우 `False`를 입력하면 됩니다. 본 라이브러리를 정상적으로 이용하기 위해서는 국토교통부 실거래가 정보 조회 서비스에 대한 OpenAPI 활용신청을 반드시 완료해야합니다.


```python
# 3. 국토교통부 실거래가 정보 조회 OpenAPI 세션 정의하기
# debug: True이면 모든 메시지 출력, False이면 오류 메시지만 출력 (기본값: False)
ts = pdr.Transaction(serviceKey, debug=True)
```

    [INFO] 아파트 매매 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 아파트 전월세 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 오피스텔 매매 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 오피스텔 전월세 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 단독다가구 매매 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 단독다가구 전월세 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 연립다세대 매매 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 연립다세대 전월세 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 상업업무용 매매 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 토지 매매 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 분양입주권 매매 조회 서비스 정상 - (00) NORMAL SERVICE.
    

<br>

## 지역코드 검색하기

아래와 같이 `PublicDataReader`의 `code_list()` 메서드를 호출하면 전국의 지역코드를 `DataFrame` 형태로 확인할 수 있습니다.

```python
# 4. 지역코드(시군구코드) 검색하기
sigunguName = "분당구"                                  # 시군구코드: 41135
code = pdr.code_list()
code.loc[(code['시군구명'].str.contains(sigunguName, na=False)) &
         (code['읍면동명'].isna())]
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
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>법정동코드</th>
      <th>읍면동명</th>
      <th>동리명</th>
      <th>생성일자</th>
      <th>말소일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5133</th>
      <td>41</td>
      <td>경기도</td>
      <td>41135</td>
      <td>성남시 분당구</td>
      <td>4113500000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>19910916</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


## 부동산 실거래가 조회하기

`prod`에 부동산 상품 종류를, `trans`에 부동산 거래 유형을 각각 입력하고, `sigunguCode`와 `yearMonth`에 지역코드와 년월 정보를 입력합니다. 이후 위에서 정의한 데이터 조회 세션인 `ts`의 `read_data` 메서드를 호출하여 실거래가를 `DataFrame` 형태로 조회합니다.

```python
# 5. 지역, 월 별 데이터 프레임 만들기
prod="아파트"                                           # 부동산 상품 종류 (ex. 아파트, 오피스텔, 단독다가구 등)
trans="매매"                                            # 부동산 거래 유형 (ex. 매매, 전월세)
sigunguCode="41135"
yearMonth="202101"

df = ts.read_data(prod, trans, sigunguCode, yearMonth)
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
      <th>도로명</th>
      <th>법정동</th>
      <th>지번</th>
      <th>아파트</th>
      <th>건축년도</th>
      <th>층</th>
      <th>전용면적</th>
      <th>년</th>
      <th>월</th>
      <th>일</th>
      <th>거래금액</th>
      <th>도로명건물본번호코드</th>
      <th>도로명건물부번호코드</th>
      <th>도로명시군구코드</th>
      <th>도로명일련번호코드</th>
      <th>도로명지상지하코드</th>
      <th>도로명코드</th>
      <th>법정동본번코드</th>
      <th>법정동부번코드</th>
      <th>법정동시군구코드</th>
      <th>법정동읍면동코드</th>
      <th>법정동지번코드</th>
      <th>일련번호</th>
      <th>거래유형</th>
      <th>중개사소재지</th>
      <th>해제사유발생일</th>
      <th>해제여부</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>41135</td>
      <td>수내로</td>
      <td>분당동</td>
      <td>38</td>
      <td>샛별마을(우방)</td>
      <td>1994</td>
      <td>8</td>
      <td>84.99</td>
      <td>2021</td>
      <td>1</td>
      <td>7</td>
      <td>135,000</td>
      <td>00181</td>
      <td>00000</td>
      <td>41135</td>
      <td>01</td>
      <td>0</td>
      <td>3180039</td>
      <td>0038</td>
      <td>0000</td>
      <td>41135</td>
      <td>10100</td>
      <td>1</td>
      <td>41135-21</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>41135</td>
      <td>수내로</td>
      <td>분당동</td>
      <td>38</td>
      <td>샛별마을(우방)</td>
      <td>1994</td>
      <td>4</td>
      <td>84.99</td>
      <td>2021</td>
      <td>1</td>
      <td>8</td>
      <td>132,500</td>
      <td>00181</td>
      <td>00000</td>
      <td>41135</td>
      <td>01</td>
      <td>0</td>
      <td>3180039</td>
      <td>0038</td>
      <td>0000</td>
      <td>41135</td>
      <td>10100</td>
      <td>1</td>
      <td>41135-21</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>41135</td>
      <td>장안로41번길</td>
      <td>분당동</td>
      <td>66</td>
      <td>장안타운(건영)</td>
      <td>1993</td>
      <td>17</td>
      <td>162.85</td>
      <td>2021</td>
      <td>1</td>
      <td>8</td>
      <td>129,000</td>
      <td>00013</td>
      <td>00000</td>
      <td>41135</td>
      <td>01</td>
      <td>0</td>
      <td>4340380</td>
      <td>0066</td>
      <td>0000</td>
      <td>41135</td>
      <td>10100</td>
      <td>1</td>
      <td>41135-32</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>41135</td>
      <td>분당로</td>
      <td>분당동</td>
      <td>34</td>
      <td>샛별마을(라이프)</td>
      <td>1992</td>
      <td>6</td>
      <td>126.415</td>
      <td>2021</td>
      <td>1</td>
      <td>22</td>
      <td>153,000</td>
      <td>00190</td>
      <td>00000</td>
      <td>41135</td>
      <td>01</td>
      <td>0</td>
      <td>3180026</td>
      <td>0034</td>
      <td>0000</td>
      <td>41135</td>
      <td>10100</td>
      <td>1</td>
      <td>41135-19</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>41135</td>
      <td>수내로</td>
      <td>분당동</td>
      <td>39</td>
      <td>샛별마을(삼부)</td>
      <td>1992</td>
      <td>6</td>
      <td>69.96</td>
      <td>2021</td>
      <td>1</td>
      <td>23</td>
      <td>107,000</td>
      <td>00201</td>
      <td>00000</td>
      <td>41135</td>
      <td>01</td>
      <td>0</td>
      <td>3180039</td>
      <td>0039</td>
      <td>0000</td>
      <td>41135</td>
      <td>10100</td>
      <td>1</td>
      <td>41135-20</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>


<br>

## 특정 기간 동안의 실거래가를 조회하기

`prod`에 부동산 상품 종류를, `trans`에 부동산 거래 유형을 각각 입력하고, `sigunguCode`에 지역코드를 입력합니다. 이어서 `startYearMonth`와 `endYearMonth`에 조회 시작년월 및 조회 종료년월을 입력합니다. 이후 위에서 정의한 데이터 조회 세션인 `ts`의 `collect_data` 메서드를 호출하여 특정 기간 동안의 실거래가를 `DataFrame` 형태로 조회합니다.

```python
# 6. 지역, 기간 별 데이터 프레임 만들기
prod="아파트"                                           # 부동산 상품 종류 (ex. 아파트, 오피스텔, 단독다가구 등)
trans="매매"                                            # 부동산 거래 유형 (ex. 매매, 전월세)
sigunguCode="41135"
startYearMonth="202101"
endYearMonth="202111"

df = ts.collect_data(prod, trans, sigunguCode, startYearMonth, endYearMonth)
df.head()
```

    [INFO] 아파트 매매 202101 조회 시작
    [INFO] 아파트 매매 202102 조회 시작
    [INFO] 아파트 매매 202103 조회 시작
    [INFO] 아파트 매매 202104 조회 시작
    [INFO] 아파트 매매 202105 조회 시작
    [INFO] 아파트 매매 202106 조회 시작
    [INFO] 아파트 매매 202107 조회 시작
    [INFO] 아파트 매매 202108 조회 시작
    [INFO] 아파트 매매 202109 조회 시작
    [INFO] 아파트 매매 202110 조회 시작
    [INFO] 아파트 매매 202111 조회 시작
    




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
      <th>도로명</th>
      <th>법정동</th>
      <th>지번</th>
      <th>아파트</th>
      <th>건축년도</th>
      <th>층</th>
      <th>전용면적</th>
      <th>년</th>
      <th>월</th>
      <th>일</th>
      <th>거래금액</th>
      <th>도로명건물본번호코드</th>
      <th>도로명건물부번호코드</th>
      <th>도로명시군구코드</th>
      <th>도로명일련번호코드</th>
      <th>도로명지상지하코드</th>
      <th>도로명코드</th>
      <th>법정동본번코드</th>
      <th>법정동부번코드</th>
      <th>법정동시군구코드</th>
      <th>법정동읍면동코드</th>
      <th>법정동지번코드</th>
      <th>일련번호</th>
      <th>거래유형</th>
      <th>중개사소재지</th>
      <th>해제사유발생일</th>
      <th>해제여부</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>41135</td>
      <td>수내로</td>
      <td>분당동</td>
      <td>38</td>
      <td>샛별마을(우방)</td>
      <td>1994</td>
      <td>8</td>
      <td>84.99</td>
      <td>2021</td>
      <td>1</td>
      <td>7</td>
      <td>135,000</td>
      <td>00181</td>
      <td>00000</td>
      <td>41135</td>
      <td>01</td>
      <td>0</td>
      <td>3180039</td>
      <td>0038</td>
      <td>0000</td>
      <td>41135</td>
      <td>10100</td>
      <td>1</td>
      <td>41135-21</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>41135</td>
      <td>수내로</td>
      <td>분당동</td>
      <td>38</td>
      <td>샛별마을(우방)</td>
      <td>1994</td>
      <td>4</td>
      <td>84.99</td>
      <td>2021</td>
      <td>1</td>
      <td>8</td>
      <td>132,500</td>
      <td>00181</td>
      <td>00000</td>
      <td>41135</td>
      <td>01</td>
      <td>0</td>
      <td>3180039</td>
      <td>0038</td>
      <td>0000</td>
      <td>41135</td>
      <td>10100</td>
      <td>1</td>
      <td>41135-21</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>41135</td>
      <td>장안로41번길</td>
      <td>분당동</td>
      <td>66</td>
      <td>장안타운(건영)</td>
      <td>1993</td>
      <td>17</td>
      <td>162.85</td>
      <td>2021</td>
      <td>1</td>
      <td>8</td>
      <td>129,000</td>
      <td>00013</td>
      <td>00000</td>
      <td>41135</td>
      <td>01</td>
      <td>0</td>
      <td>4340380</td>
      <td>0066</td>
      <td>0000</td>
      <td>41135</td>
      <td>10100</td>
      <td>1</td>
      <td>41135-32</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>41135</td>
      <td>분당로</td>
      <td>분당동</td>
      <td>34</td>
      <td>샛별마을(라이프)</td>
      <td>1992</td>
      <td>6</td>
      <td>126.415</td>
      <td>2021</td>
      <td>1</td>
      <td>22</td>
      <td>153,000</td>
      <td>00190</td>
      <td>00000</td>
      <td>41135</td>
      <td>01</td>
      <td>0</td>
      <td>3180026</td>
      <td>0034</td>
      <td>0000</td>
      <td>41135</td>
      <td>10100</td>
      <td>1</td>
      <td>41135-19</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>41135</td>
      <td>수내로</td>
      <td>분당동</td>
      <td>39</td>
      <td>샛별마을(삼부)</td>
      <td>1992</td>
      <td>6</td>
      <td>69.96</td>
      <td>2021</td>
      <td>1</td>
      <td>23</td>
      <td>107,000</td>
      <td>00201</td>
      <td>00000</td>
      <td>41135</td>
      <td>01</td>
      <td>0</td>
      <td>3180039</td>
      <td>0039</td>
      <td>0000</td>
      <td>41135</td>
      <td>10100</td>
      <td>1</td>
      <td>41135-20</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>


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
# 1. 라이브러리 임포트하기
import PublicDataReader as pdr
print(pdr.__version__)
print(pdr.__info__)

# 2. 공공 데이터 포털 OpenAPI 서비스 인증키 입력하기
serviceKey = "공공 데이터 포털에서 발급받은 서비스 키"

# 3. 국토교통부 실거래가 정보 조회 OpenAPI 세션 정의하기
# debug: True이면 모든 메시지 출력, False이면 오류 메시지만 출력 (기본값: False)
ts = pdr.Transaction(serviceKey, debug=True)

# 4. 지역코드(시군구코드) 검색하기
sigunguName = "분당구"                                  # 시군구코드: 41135
code = pdr.code_list()
code.loc[(code['시군구명'].str.contains(sigunguName, na=False)) &
         (code['읍면동명'].isna())]

# 5. 지역, 월 별 데이터 프레임 만들기
prod="아파트"                                           # 부동산 상품 종류 (ex. 아파트, 오피스텔, 단독다가구 등)
trans="매매"                                            # 부동산 거래 유형 (ex. 매매, 전월세)
sigunguCode="41135"
yearMonth="202101"

df = ts.read_data(prod, trans, sigunguCode, yearMonth)
df.head()

# 6. 지역, 기간 별 데이터 프레임 만들기
prod="아파트"                                           # 부동산 상품 종류 (ex. 아파트, 오피스텔, 단독다가구 등)
trans="매매"                                            # 부동산 거래 유형 (ex. 매매, 전월세)
sigunguCode="41135"
startYearMonth="202101"
endYearMonth="202111"

df = ts.collect_data(prod, trans, sigunguCode, startYearMonth, endYearMonth)
df.head()
```

<br>

## PublicDataReader는 오픈소스 프로젝트로 개발하고 있습니다.

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}


`PublicDataReader`는 파이썬 오픈소스 프로젝트로 개발하고 있습니다. 궁금한 점이 있으시면 언제든지 아래 이메일이나 카카오톡 오픈채팅방을 이용해주세요. 감사합니다.

<br>

**E-mail : wooil@kakao.com**  
**[(Python) PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)**  
  - (Python) PublicDataReader 사용 관련 Q&A를 위한 카카오톡 오픈 채팅방입니다.

<br>

### PublicDataReader 관련 글 목록

- [PublicDataReader - 부동산 실거래가 조회하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [PublicDataReader - 건축물대장 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_03/)
- [PublicDataReader - 상가업소 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_02/)

### Github Repository

- [PublicDataReader](https://github.com/WooilJeong/PublicDataReader)  
