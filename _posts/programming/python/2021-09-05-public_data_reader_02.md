---
title: "PublicDataReader - 상가업소 데이터 조회하기"
categories: python
tags: PublicDataReader
header:
  overlay_image: /assets/img/logo/PublicDataReader.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 소상공인진흥공단 상가업소 데이터 조회하기

<br>

![PNG](https://github.com/WooilJeong/PublicDataReader/blob/main/assets/img/logo.png?raw=true)

파이썬 [PublicDataReader](https://github.com/WooilJeong/PublicDataReader) 라이브러리를 이용하면 **공공데이터포털**에서 제공하는 **소상공인진흥공단 상가업소** 데이터를 쉽게 조회할 수 있습니다. 

<br>

## PublicDataReader

PublicDataReader는 공공 데이터를 자동으로 조회할 수 있는 파이썬 라이브러리입니다. 이 라이브러리로 공공데이터포털과 국가통계포털(KOSIS)과 같이 오픈 API 서비스로 제공하는 공공 데이터를 쉽게 조회할 수 있습니다. 인증키가 필요한 공공 데이터는 인증키를 사용하여 조회할 수 있고, 인증키가 필요하지 않은 데이터는 별도의 인증 절차 없이 조회할 수 있습니다. PublicDataReader를 이용하면 일반적인 공공 데이터 조회 과정에서 필요한 API 명세 찾기, 요청 작성, 반환된 데이터 정리 과정을 자동으로 처리할 수 있고, 웹에 공개된 데이터를 조회할 때도 데이터 수집과 가공 과정을 자동화할 수 있습니다. 이를 통해 코드 작성이 간결해지고 공공 데이터 조회 작업이 편리해집니다.

- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)

<br>

### 소상공인 상가업소 정보 조회 서비스

PublicDataReader를 통해 공공데이터포털에서 제공하는 Open API 서비스를 정상적으로 이용하려면 아래 서비스 신청 페이지 링크에 접속 후 서비스 이용 신청을 해야 합니다. 서비스 신청을 완료하면 Open API를 사용할 수 있는 서비스 키가 발급됩니다. 서비스 제공 기관에 따라 서비스 키 발급 후 약 1~2일이 지난 후 접근 권한이 부여될 수 있습니다. 충분한 시간이 지난 후에도 접근 권한이 부여되지 않는다면 서비스 제공처에 문의하는 것을 권장합니다.

- [소상공인 상가업소 정보 조회 서비스 신청 페이지](https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15012005)


| **서비스명**               | **카테고리명** |
| -------------------------- | -------------- |
| 지정 상권조회              | 지정상권       |
| 반경내 상권조회            | 반경상권       |
| 사각형내 상권조회          | 사각형상권     |
| 행정구역 단위 상권조회     | 행정구역상권   |
| 단일 상가업소 조회         | 단일상가       |
| 건물단위 상가업소 조회     | 건물상가       |
| 지번단위 상가업소 조회     | 지번상가       |
| 행정동 단위 상가업소 조회  | 행정동상가     |
| 상권내 상가업소 조회       | 상권상가       |
| 반경내 상가업소 조회       | 반경상가       |
| 사각형내 상가업소 조회     | 사각형상가     |
| 다각형내 상가업소 조회     | 다각형상가     |
| 업종별 상가업소 조회       | 업종별상가     |
| 수정일자기준 상가업소 조회 | 수정일자상가   |
| 상권정보 업종 대분류 조회  | 업종대분류     |
| 상권정보 업종 중분류 조회  | 업종중분류     |
| 상권정보 업종 소분류 조회  | 업종소분류     |

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

**공공데이터포털**에서 발급받은 **서비스 키**를 복사하여 다음과 같이 `serviceKey` 변수에 할당합니다. 오픈 API 서비스 키 발급 방법에 대해 궁금하신 분들은 구글에 '**공공데이터포털 오픈 API 사용법**'을 검색하시면 여러 문서들을 참조할 수 있습니다.

```python
service_key = "공공데이터포털에서 발급받은 서비스 키"
```

<br>

## 데이터 조회 세션 만들기

다음과 같이 발급받은 `serviceKey` 값을 이용해 부동산 실거래가 데이터를 조회할 `si` 세션을 만들어줍니다. 본 라이브러리를 정상적으로 이용하기 위해서는 소상공인 상가업소 정보 조회 서비스에 대한 OpenAPI 활용신청을 반드시 완료해야합니다.


```python
# 상가업소 정보 조회 클래스 임포트하기
from PublicDataReader import SmallShop

# 데이터 조회 API 인스턴스 만들기
api = SmallShop(service_key)
```

<br>

## 데이터 조회하기

- 지정 상권조회


```python
df = api.get_data(
    service_name = "지정상권",
    key = "9301",
)
df.tail(1)
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
      <th>상권번호</th>
      <th>상권명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>면적</th>
      <th>좌표개수</th>
      <th>좌표값</th>
      <th>데이터기준일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9301</td>
      <td>압구정 로데오거리_1</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>193739</td>
      <td>19</td>
      <td>MULTIPOLYGON (((127.04700219001 37.52446081421...</td>
      <td>2021-06-30</td>
    </tr>
  </tbody>
</table>
</div>



- 반경내 상권조회


```python
df = api.get_data(
    service_name = "반경상권",
    cx = 127.042325940821,
    cy = 37.5272105674053,
    radius = 500,
)
df.tail(1)
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
      <th>상권번호</th>
      <th>상권명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>면적</th>
      <th>좌표개수</th>
      <th>좌표값</th>
      <th>데이터기준일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>9316</td>
      <td>청담사거리_1</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>20169</td>
      <td>21</td>
      <td>MULTIPOLYGON (((127.045624467128 37.5237563085...</td>
      <td>2021-06-30</td>
    </tr>
  </tbody>
</table>
</div>



- 사각형내 상권조회


```python
df = api.get_data(
    service_name = "사각형상권",
    minx = 127.0327683531071,
    miny = 37.495967935149146,
    maxx = 127.04268179746694,
    maxy = 37.502402894207286
)
df.tail(1)
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
      <th>상권번호</th>
      <th>상권명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>면적</th>
      <th>좌표개수</th>
      <th>좌표값</th>
      <th>데이터기준일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>9308</td>
      <td>역삼역_2</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>51935</td>
      <td>32</td>
      <td>MULTIPOLYGON (((127.042493260333 37.5022663760...</td>
      <td>2021-06-30</td>
    </tr>
  </tbody>
</table>
</div>



- 행정구역 단위 상권조회


```python
df = api.get_data(
    service_name = "행정구역상권",
    divId = 'adongCd',
    key = '1168058000'
)
df.tail(1)
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
      <th>상권번호</th>
      <th>상권명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>면적</th>
      <th>좌표개수</th>
      <th>좌표값</th>
      <th>데이터기준일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>9323</td>
      <td>포스코사거리_2</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>79514</td>
      <td>34</td>
      <td>MULTIPOLYGON (((127.053620405866 37.5128036692...</td>
      <td>2021-06-30</td>
    </tr>
  </tbody>
</table>
</div>



- 단일 상가업소 조회


```python
df = api.get_data(
    service_name = "단일상가",
    divId = 'adongCd',
    key = '11757465'
)
df.tail(1)
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11757465</td>
      <td>스타벅스</td>
      <td>방배점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>1165010100108120002009897</td>
      <td>None</td>
      <td>서울특별시 서초구 방배로 211, (방배동)</td>
      <td>137832</td>
      <td>06562</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>126.99050928001</td>
      <td>37.4919441682448</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 39 columns</p>
</div>



- 건물단위 상가업소 조회


```python
df = api.get_data(
    service_name = "건물상가",
    key = '1168011000104940000004966'
)
df.tail(1)
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>82</th>
      <td>28508108</td>
      <td>아모레퍼시픽백화점갤러리아압구정</td>
      <td>None</td>
      <td>D</td>
      <td>소매</td>
      <td>D16</td>
      <td>화장품소매</td>
      <td>D16A01</td>
      <td>화장품판매점</td>
      <td>G47813</td>
      <td>...</td>
      <td>1168011000104940000004966</td>
      <td>갤러리아백화점</td>
      <td>서울특별시 강남구 압구정로 343, (압구정동)</td>
      <td>135902</td>
      <td>06008</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>127.04008070443</td>
      <td>37.5284986430328</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 39 columns</p>
</div>



- 지번단위 상가업소 조회


```python
df = api.get_data(
    service_name = "지번상가",
    key = '1165010100108120002'
)
df.tail(1)
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>17875647</td>
      <td>키무브필라테스스튜디오</td>
      <td>None</td>
      <td>N</td>
      <td>관광/여가/오락</td>
      <td>N05</td>
      <td>요가/단전/마사지</td>
      <td>N05A01</td>
      <td>요가/단식</td>
      <td>S96129</td>
      <td>...</td>
      <td>1165010100108120002009897</td>
      <td>None</td>
      <td>서울특별시 서초구 방배로 211, (방배동)</td>
      <td>137832</td>
      <td>06562</td>
      <td>None</td>
      <td>5</td>
      <td>None</td>
      <td>126.990621865291</td>
      <td>37.4919811723816</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 39 columns</p>
</div>



- 행정동 단위 상가업소 조회


```python
df = api.get_data(
    service_name = "행정동상가",
    divId = 'adongCd',
    key = '1168064000',
    indsLclsCd = 'Q'
)
df.tail(1)
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>999</th>
      <td>17078682</td>
      <td>투썸플레이스</td>
      <td>강남역KR타워점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>1168010100108250026024506</td>
      <td>None</td>
      <td>서울특별시 강남구 강남대로84길 13, (역삼동)</td>
      <td>135934</td>
      <td>06232</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>127.029572455617</td>
      <td>37.4968668925481</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 39 columns</p>
</div>



- 상권내 상가업소 조회


```python
df = api.get_data(
    service_name = "상권상가",
    key = '9368',
    indsLclsCd = 'Q'
)
df.tail(1)
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>188</th>
      <td>9136408</td>
      <td>KFC역삼점</td>
      <td>역삼점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q05</td>
      <td>닭/오리요리</td>
      <td>Q05A08</td>
      <td>후라이드/양념치킨</td>
      <td>I56193</td>
      <td>...</td>
      <td>1168010100106420010026120</td>
      <td>송암II빌딩</td>
      <td>서울특별시 강남구 논현로 509, (역삼동)</td>
      <td>135910</td>
      <td>06132</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>127.036078078957</td>
      <td>37.5019210138113</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 39 columns</p>
</div>



- 반경내 상가업소 조회


```python
df = api.get_data(
    service_name = "반경상가",
    radius = '500',
    cx = 127.03641615737838,
    cy = 37.50059843782878,
    indsLclsCd = 'Q'
)
df.tail(1)
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>847</th>
      <td>9136408</td>
      <td>KFC역삼점</td>
      <td>역삼점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q05</td>
      <td>닭/오리요리</td>
      <td>Q05A08</td>
      <td>후라이드/양념치킨</td>
      <td>I56193</td>
      <td>...</td>
      <td>1168010100106420010026120</td>
      <td>송암II빌딩</td>
      <td>서울특별시 강남구 논현로 509, (역삼동)</td>
      <td>135910</td>
      <td>06132</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>127.036078078957</td>
      <td>37.5019210138113</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 39 columns</p>
</div>



- 사각형내 상가업소 조회


```python
df = api.get_data(
    service_name = "사각형상가",
    minx = 127.0327683531071,
    miny = 37.495967935149146,
    maxx = 127.04268179746694,
    maxy = 37.502402894207286,
    indsLclsCd = 'Q'
)
df.tail(1)
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>579</th>
      <td>9136408</td>
      <td>KFC역삼점</td>
      <td>역삼점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q05</td>
      <td>닭/오리요리</td>
      <td>Q05A08</td>
      <td>후라이드/양념치킨</td>
      <td>I56193</td>
      <td>...</td>
      <td>1168010100106420010026120</td>
      <td>송암II빌딩</td>
      <td>서울특별시 강남구 논현로 509, (역삼동)</td>
      <td>135910</td>
      <td>06132</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>127.036078078957</td>
      <td>37.5019210138113</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 39 columns</p>
</div>



- 다각형내 상가업소 조회


```python
df = api.get_data(
    service_name = "다각형상가",
    key = 'POLYGON((127.02355609555755 37.504264372557095, 127.02496157306963 37.50590702991155, 127.0270858825753 37.50486867039889, 127.02628121988377 37.503489842823114))',
    indsLclsCd = 'Q'
)
df.tail(1)
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>47</th>
      <td>26260692</td>
      <td>신마포갈매기</td>
      <td>신논현역점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A02</td>
      <td>갈비/삼겹살</td>
      <td>I56111</td>
      <td>...</td>
      <td>1168010100108100000022737</td>
      <td>None</td>
      <td>서울특별시 강남구 강남대로110길 12, (역삼동)</td>
      <td>135931</td>
      <td>06123</td>
      <td>None</td>
      <td>2</td>
      <td>None</td>
      <td>127.025806325949</td>
      <td>37.5038461003511</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 39 columns</p>
</div>



- 업종별 상가업소 조회


```python
df = api.get_data(
    service_name = "업종별상가",
    divId = 'indsLclsCd',
    key = 'Q'
)
df.tail(1)
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>999</th>
      <td>11181582</td>
      <td>내수한우마을</td>
      <td>None</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>4371025041001700005054390</td>
      <td>None</td>
      <td>충청북도 청주시 청원구 내수읍 청암로 111, (학평리)</td>
      <td>363934</td>
      <td>28146</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>127.542578061469</td>
      <td>36.7252477746175</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 39 columns</p>
</div>



- 수정일자기준 상가업소 조회


```python
df = api.get_data(
    service_name = "수정일자상가",
    key = '20200101',
    indsLclsCd = 'Q'
)
df.tail(1)
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
      <th>chgGb</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>25852182</td>
      <td>그랜드테이블</td>
      <td>None</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q06</td>
      <td>양식</td>
      <td>Q06A05</td>
      <td>패밀리레스토랑</td>
      <td>I56114</td>
      <td>...</td>
      <td>해운대그랜드호텔</td>
      <td>부산광역시 해운대구 해운대해변로 217, (우동)</td>
      <td>612821</td>
      <td>48093</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>129.155174224526</td>
      <td>35.1591175731937</td>
      <td>U</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 40 columns</p>
</div>



- 상권정보업종 대분류 조회


```python
df = api.get_data(
    service_name = "업종대분류",
)
df.tail(1)
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
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>데이터기준일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>41</th>
      <td>R</td>
      <td>학문/교육</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>



- 상권정보업종 중분류 조회


```python
df = api.get_data(
    service_name = "업종중분류",
    indsLclsCd = 'Q'
)
df.tail(1)
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
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>데이터기준일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>31</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>



- 상권정보업종 소분류 조회


```python
df = api.get_data(
    service_name = "업종소분류",
    indsLclsCd = 'Q',
    indsMclsCd = 'Q01'
)
df.tail(1)
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
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>데이터기준일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A18</td>
      <td>황태전문</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 참고

- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)