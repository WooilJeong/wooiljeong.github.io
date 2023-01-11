---
title: "PublicDataReader - 부동산 실거래가 조회하기"
categories: python
tags: PublicDataReader
header:
  overlay_image: /assets/img/logo/PublicDataReader.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 국토교통부 부동산 실거래 데이터 조회하기

<br>

![PNG](https://github.com/WooilJeong/PublicDataReader/blob/main/assets/img/logo.png?raw=true)


파이썬 [PublicDataReader](https://github.com/WooilJeong/PublicDataReader) 라이브러리를 이용하면 **공공데이터포털**에서 제공하는 **국토교통부 부동산 실거래가** 데이터를 쉽게 조회할 수 있습니다. 

<br>

## PublicDataReader

PublicDataReader는 공공 데이터를 자동으로 조회할 수 있는 파이썬 라이브러리입니다. 이 라이브러리는 공공데이터포털과 국가통계포털(KOSIS)과 같이 Open API 서비스로 제공하는 공공 데이터를 쉽게 조회할 수 있도록 도와줍니다. 인증키가 필요한 공공 데이터는 인증키를 사용하여 조회할 수 있고, 인증키가 필요하지 않은 데이터는 별도의 인증 절차 없이 조회할 수 있습니다. PublicDataReader를 이용하면 일반적인 공공 데이터 조회 과정에서 필요한 API 명세 찾기, 요청 작성, 반환된 데이터 정리 과정을 자동으로 처리해줍니다. 또한, 웹에 공개된 데이터를 조회할 때도 데이터 수집과 가공 과정을 자동화해줍니다. 이를 통해 코드 작성이 간결해지고 공공 데이터 조회 작업이 편리해집니다.

- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)

<br>

## 국토교통부 실거래가 정보 조회 서비스

PublicDataReader를 통해 공공데이터포털에서 제공하는 Open API 서비스를 정상적으로 이용하려면 아래 표에서 사용할 서비스명 링크에 접속 후 서비스 이용 신청을 해야 합니다. 서비스 신청을 완료하면 Open API를 사용할 수 있는 서비스 키가 발급됩니다. 서비스 제공 기관에 따라 서비스 키 발급 후 약 1~2일이 지난 후 접근 권한이 부여될 수 있습니다. 충분한 시간이 지난 후에도 접근 권한이 부여되지 않는다면 서비스 제공처에 문의하는 것을 권장합니다.


| **서비스명**                          | **부동산 유형** | **거래 유형** |
| ------------------------------------- | ------------ | ------------ |
| [아파트매매 실거래 상세 자료 조회](https://www.data.go.kr/data/15057511/openapi.do)      | 아파트       | 매매         |
| [아파트 전월세 자료 조회](https://www.data.go.kr/data/15058017/openapi.do)               | 아파트       | 전월세       |
| [아파트 분양권전매 신고 자료 조회](https://www.data.go.kr/data/15056782/openapi.do)      | 분양입주권   | 매매         |
| [오피스텔 매매 신고 조회](https://www.data.go.kr/data/15058452/openapi.do)               | 오피스텔     | 매매         |
| [오피스텔 전월세 신고 조회](https://www.data.go.kr/data/15059249/openapi.do)             | 오피스텔     | 전월세       |
| [연립다세대 매매 실거래자료 조회](https://www.data.go.kr/data/15058038/openapi.do)       | 연립다세대   | 매매         |
| [연립다세대 전월세 실거래자료 조회](https://www.data.go.kr/data/15058016/openapi.do)     | 연립다세대   | 전월세       |
| [단독/다가구 매매 실거래 조회](https://www.data.go.kr/data/15058022/openapi.do)          | 단독다가구   | 매매         |
| [단독/다가구 전월세 자료 조회](https://www.data.go.kr/data/15058352/openapi.do)          | 단독다가구   | 전월세       |
| [토지 매매 신고 조회](https://www.data.go.kr/data/15056649/openapi.do)                   | 토지         | 매매         |
| [상업업무용 부동산 매매 신고 자료 조회](https://www.data.go.kr/data/15057267/openapi.do) | 상업업무용   | 매매         |
| [공장 및 창고 등 부동산 매매 신고 자료 조회](https://www.data.go.kr/data/15100574/openapi.do) | 공장창고등   | 매매         |


<br>

## PublicDataReader 설치하기

1. 운영체제(OS)에 따라 아래 중 하나를 선택합니다.

- Windows: CMD(명령 프롬프트) 실행
- Mac: Terminal(터미널) 실행

2. 아래 Shell 명령어를 입력 후 실행합니다.

```bash
pip install PublicDataReader --upgrade
```

<br>

## 오픈 API 서비스 키 입력하기

**공공데이터포털**에서 발급받은 **서비스 키**를 복사하여 다음과 같이 `service_key` 변수에 할당합니다. 오픈 API 서비스 키 발급 방법에 대해 궁금하신 분들은 구글에 '**공공데이터포털 오픈 API 사용법**'을 검색하시면 여러 문서들을 참조할 수 있습니다.

```python
service_key = "공공데이터포털에서 발급받은 서비스 키"
```

<br>

## 국토교통부 실거래가 조회 API 인스턴스 정의하기

PublicDataReader 라이브러리에서 `TransactionPrice`라는 클래스를 가져와 `api` 라는 실거래가 조회 API 인스턴스를 생성합니다. 이 때, `service_key` 값을 인자로 입력합니다.

```python
from PublicDataReader import TransactionPrice
api = TransactionPrice(service_key)
```

<br>


## 데이터 조회 시 필요한 입력 항목

| 이름             | 설명                                                                                                                               | 데이터 타입   | 샘플 데이터   | 항목구분    |
|:-----------------|:-----------------------------------------------------------------------------------------------------------------------------------|:--------------|:--------------|:------------|
| property_type    | 부동산 유형<br>(아파트, 오피스텔, 단독다가구, 연립다세대, 토지, 분양입주권, 공장창고등)                                            | String        | 아파트        | 필수        |
| trade_type       | 거래 유형<br>(매매, 전월세)                                                                                                        | String        | 매매          | 필수        |
| sigungu_code     | 시군구의 5자리 지역코드<br>(서울 서초구: 11650, 경기 성남 분당구: 41135)                                                           | String        | 11650         | 필수        |
| year_month       | 조회 년월 (단일 월 조회 시 필수)<br>(2023년 1월: 202301)<br>※ start_year_month와 end_year_month 모두 입력 시 기간 내 조회가 실행됨 | String        | 202301        | 조건부 필수 |
| start_year_month | 조회 시작 년월 (기간 내 조회 시 필수)<br>(2022년 1월 202201)                                                                       | String        | 202201        | 조건부 필수 |
| end_year_month   | 조회 종료 년월 (기간 내 조회 시 필수)<br>(2022년 12월: 202212)                                                                     | String        | 202212        | 조건부 필수 |
| verbose          | 데이터 조회 진행 상황 메시지 출력 여부<br>(출력: True, 미출력: False)<br>※ 기본값: False                                           | Boolean       | True          | 선택        |


<br>

## 시군구코드 조회하기

부동산 실거래가를 조회하려면 조회할 부동산이 속해 있는 시군구 정보를 알아야 합니다. 예를 들어, 경기도 성남시 분당구에 있는 아파트 매매 실거래가를 조회한다고 가정하면, 분당구에 해당하는 시군구코드 정보를 알아야 합니다. 아래 코드는 PublicDataReader 라이브러리를 사용하여 분당구와 일치하는 행을 찾는 작업을 수행합니다. 

1. 먼저 PublicDataReader 라이브러리를 가져와 pdr라는 별칭으로 사용 할 수 있도록 합니다.
2. '분당구'라는 이름을 가진 변수 sigungu_name을 선언합니다.
3. pdr.code_bdong()를 실행 하면 관련된 정보를 데이터프레임 형식으로 가져옵니다.
4. code라는 변수에 데이터프레임을 저장 합니다.
5. loc를 사용하여 특정 조건에 해당하는 행만 선택합니다.
- a. (code['시군구명'].str.contains(sigungu_name, na=False)) : '시군구명'열에서 "분당구"라는 문자열을 포함하는 행을 선택합니다.
- b. (code['읍면동명'].isna()) : '읍면동명'열이 NaN값인 행만 선택합니다.
6. 결과로 위에서 선택한 조건을 만족하는 행만 표시됩니다.

조회 결과를 살펴보면 분당구에 해당하는 시군구코드는 41135라는 것을 알 수 있습니다.

```python
import PublicDataReader as pdr
sigungu_name = "분당구"
code = pdr.code_bdong()
code.loc[(code['시군구명'].str.contains(sigungu_name, na=False)) &
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
      <th>5152</th>
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

<br>

## 부동산 실거래가 조회하기

'아파트매매 실거래 상세 자료 조회' 서비스 신청을 완료했다고 가정하고, 아파트 매매 실거래가를 조회합니다. api.get_data 메서드를 호출하여 데이터를 조회할 수 있습니다. 메서드의 인자들 중 property_type은 부동산 유형을 의미합니다. 여기에 '아파트'라고 입력합니다. trade_type은 거래 유형을 의미합니다. 여기에 '매매'라고 입력합니다. sigungu_code는 시군구코드를 의미합니다. 여기에 '41135'를 입력합니다. year_month는 조회년월을 의미합니다. 여기에 2022년 12월을 뜻하는 '202212'를 입력합니다. 메서드 호출 결과를 df 변수에 할당합니다.

```python
# 특정 년월 자료만 조회하기
df = api.get_data(
    property_type="아파트",
    trade_type="매매",
    sigungu_code="41135",
    year_month="202212",
    )
df.tail()
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
      <th>...</th>
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
      <th>33</th>
      <td>41135</td>
      <td>미금로22번길</td>
      <td>구미동</td>
      <td>212</td>
      <td>무지개(12단지)(주공뜨란체)</td>
      <td>1995</td>
      <td>9</td>
      <td>49.360</td>
      <td>2022</td>
      <td>12</td>
      <td>...</td>
      <td>0212</td>
      <td>0000</td>
      <td>41135</td>
      <td>11400</td>
      <td>1</td>
      <td>41135-187</td>
      <td>중개거래</td>
      <td>경기 성남분당구</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>34</th>
      <td>41135</td>
      <td>산운로</td>
      <td>운중동</td>
      <td>956</td>
      <td>산운마을8단지(부영사랑으로)</td>
      <td>2008</td>
      <td>11</td>
      <td>59.612</td>
      <td>2022</td>
      <td>12</td>
      <td>...</td>
      <td>0956</td>
      <td>0000</td>
      <td>41135</td>
      <td>11500</td>
      <td>1</td>
      <td>41135-434</td>
      <td>중개거래</td>
      <td>경기 성남분당구, 경기 용인기흥구</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>35</th>
      <td>41135</td>
      <td>판교원로82번길</td>
      <td>운중동</td>
      <td>918</td>
      <td>산운마을13단지(태영)</td>
      <td>2010</td>
      <td>17</td>
      <td>84.720</td>
      <td>2022</td>
      <td>12</td>
      <td>...</td>
      <td>0918</td>
      <td>0000</td>
      <td>41135</td>
      <td>11500</td>
      <td>1</td>
      <td>41135-477</td>
      <td>중개거래</td>
      <td>경기 성남분당구</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>36</th>
      <td>41135</td>
      <td>판교원로</td>
      <td>운중동</td>
      <td>922</td>
      <td>산운마을11단지(판교포레라움)</td>
      <td>2009</td>
      <td>5</td>
      <td>59.900</td>
      <td>2022</td>
      <td>12</td>
      <td>...</td>
      <td>0922</td>
      <td>0000</td>
      <td>41135</td>
      <td>11500</td>
      <td>1</td>
      <td>41135-466</td>
      <td>중개거래</td>
      <td>경기 성남분당구</td>
      <td>22.12.20</td>
      <td>O</td>
    </tr>
    <tr>
      <th>37</th>
      <td>41135</td>
      <td>판교원로</td>
      <td>운중동</td>
      <td>922</td>
      <td>산운마을11단지(판교포레라움)</td>
      <td>2009</td>
      <td>5</td>
      <td>59.900</td>
      <td>2022</td>
      <td>12</td>
      <td>...</td>
      <td>0922</td>
      <td>0000</td>
      <td>41135</td>
      <td>11500</td>
      <td>1</td>
      <td>41135-466</td>
      <td>중개거래</td>
      <td>경기 성남분당구</td>
      <td>None</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>

<br>


특정 조회년월이 아니라 특정 기간에 해당하는 데이터를 조회하려면 year_month 인자 대신 start_year_month와 end_year_month 인자를 이용하면 됩니다. 예를 들어, start_year_month에 '202212'를 입력하고, end_year_month에 '202301'을 입력한 후에 메서드를 실행하면 2022년 12월 부터 2023년 1월 까지 모든 실거래가 데이터를 조회합니다.

```python
# 특정 기간 자료 조회하기
df = api.get_data(
    property_type="아파트",
    trade_type="매매",
    sigungu_code="41135",
    start_year_month="202212",
    end_year_month="202301",
    )
df.tail()
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
      <th>...</th>
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
      <th>35</th>
      <td>41135</td>
      <td>판교원로82번길</td>
      <td>운중동</td>
      <td>918</td>
      <td>산운마을13단지(태영)</td>
      <td>2010</td>
      <td>17</td>
      <td>84.72</td>
      <td>2022</td>
      <td>12</td>
      <td>...</td>
      <td>0918</td>
      <td>0000</td>
      <td>41135</td>
      <td>11500</td>
      <td>1</td>
      <td>41135-477</td>
      <td>중개거래</td>
      <td>경기 성남분당구</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>36</th>
      <td>41135</td>
      <td>판교원로</td>
      <td>운중동</td>
      <td>922</td>
      <td>산운마을11단지(판교포레라움)</td>
      <td>2009</td>
      <td>5</td>
      <td>59.90</td>
      <td>2022</td>
      <td>12</td>
      <td>...</td>
      <td>0922</td>
      <td>0000</td>
      <td>41135</td>
      <td>11500</td>
      <td>1</td>
      <td>41135-466</td>
      <td>중개거래</td>
      <td>경기 성남분당구</td>
      <td>22.12.20</td>
      <td>O</td>
    </tr>
    <tr>
      <th>37</th>
      <td>41135</td>
      <td>판교원로</td>
      <td>운중동</td>
      <td>922</td>
      <td>산운마을11단지(판교포레라움)</td>
      <td>2009</td>
      <td>5</td>
      <td>59.90</td>
      <td>2022</td>
      <td>12</td>
      <td>...</td>
      <td>0922</td>
      <td>0000</td>
      <td>41135</td>
      <td>11500</td>
      <td>1</td>
      <td>41135-466</td>
      <td>중개거래</td>
      <td>경기 성남분당구</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>38</th>
      <td>41135</td>
      <td>서현로</td>
      <td>이매동</td>
      <td>124</td>
      <td>이매촌(한신)</td>
      <td>1992</td>
      <td>10</td>
      <td>50.10</td>
      <td>2023</td>
      <td>1</td>
      <td>...</td>
      <td>0124</td>
      <td>0000</td>
      <td>41135</td>
      <td>10600</td>
      <td>1</td>
      <td>41135-97</td>
      <td>직거래</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>39</th>
      <td>41135</td>
      <td>미금로</td>
      <td>구미동</td>
      <td>220</td>
      <td>무지개(4단지)(주공)</td>
      <td>1995</td>
      <td>16</td>
      <td>59.98</td>
      <td>2023</td>
      <td>1</td>
      <td>...</td>
      <td>0220</td>
      <td>0000</td>
      <td>41135</td>
      <td>11400</td>
      <td>1</td>
      <td>41135-220</td>
      <td>중개거래</td>
      <td>경기 성남분당구</td>
      <td>None</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>

<br>


## 전체 코드 정리

전체 코드를 정리하면 다음과 같습니다.

```python
# 서비스키 할당하기
service_key = "공공데이터포털에서 발급받은 서비스 키"

# 데이터 조회 인스턴스 만들기
from PublicDataReader import TransactionPrice
api = TransactionPrice(service_key)

# 시군구코드 조회하기
import PublicDataReader as pdr
sigungu_name = "분당구"
code = pdr.code_bdong()
code.loc[(code['시군구명'].str.contains(sigungu_name, na=False)) &
         (code['읍면동명'].isna())]

# 특정 년월 자료만 조회하기
df = api.get_data(
    property_type="아파트",
    trade_type="매매",
    sigungu_code="41135",
    year_month="202212",
    )

# 특정 기간 자료 조회하기
df = api.get_data(
    property_type="아파트",
    trade_type="매매",
    sigungu_code="41135",
    start_year_month="202212",
    end_year_month="202301",
    )
```

<br>

### 참고

- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)