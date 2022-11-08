---
title: "PublicDataReader - 상가업소 데이터 조회하기"
categories: python
tags: PublicDataReader
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 소상공인진흥공단 상가업소 데이터 조회하기

<br>

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}

<br>

### 참고 
- **[GitHub](https://github.com/WooilJeong/PublicDataReader)**  

- **사용설명서**  
  - [(블로그) 부동산 실거래가 조회하기](https://wooiljeong.github.io/python/public_data_reader_01/)
  - [(블로그) 건축물대장 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_03/)
  - [(블로그) 상가업소 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_02/)

- **실습**  
  - [Colab에서 PublicDataReader 실행하기](https://colab.research.google.com/drive/1fgT0D_tP-JyglobtDFfYQ6wQXfWWujIV?usp=sharing)  

- **문의**  
  - **이메일**: wooil@kakao.com  
  - **카카오톡 오픈채팅방**: [(Python) PublicDataReader Q&A](https://open.kakao.com/o/gbt2Pl2d)  


<br>

### 소상공인 상가업소 정보 조회 서비스

- [소상공인 상가업소 정보](https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15012005)

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

```

    1.0.0
   
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

다음과 같이 발급받은 `serviceKey` 값을 이용해 부동산 실거래가 데이터를 조회할 `si` 세션을 만들어줍니다. `debug`의 값을 `True`로 입력하면 아래와 같은 메시지를 확인할 수 있습니다. 메시지 출력을 원치 않는 경우 `False`를 입력하면 됩니다. 본 라이브러리를 정상적으로 이용하기 위해서는 소상공인 상가업소 정보 조회 서비스에 대한 OpenAPI 활용신청을 반드시 완료해야합니다.


```python
# 3. 소상공인 상가업소 정보 조회 OpenAPI 인스턴스 생성하기
# debug: True이면 모든 메시지 출력, False이면 오류 메시지만 출력 (기본값: False)
si = pdr.StoreInfo(serviceKey, debug=True)
```

<br>

```python
# 4. 데이터프레임으로 자료 조회하기

# 4-1. 지정상권
category = "지정상권"

key = "9174"

df = si.read_data(category=category, key=key)
df.head(1)
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
      <td>9174</td>
      <td>인사동</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11110</td>
      <td>종로구</td>
      <td>226875</td>
      <td>21</td>
      <td>MULTIPOLYGON (((126.986059148338 37.5765234907...</td>
      <td>2021-06-30</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-2. 반경상권
category = "반경상권"

radius = 500
cx = 127.03641615737838
cy = 37.50059843782878

df = si.read_data(category=category, radius=radius, cx=cx, cy=cy)
df.head(1)
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
      <td>9368</td>
      <td>강남역_5</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>146972</td>
      <td>53</td>
      <td>MULTIPOLYGON (((127.032936062846 37.5068531522...</td>
      <td>2021-06-30</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-3. 사각형상권
category = "사각형상권"

minx = 127.0327683531071
miny = 37.495967935149146
maxx = 127.04268179746694
maxy = 37.502402894207286

df = si.read_data(category=category, minx=minx, miny=miny, maxx=maxx, maxy=maxy)
df.head(1)
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
      <td>9368</td>
      <td>강남역_5</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>146972</td>
      <td>53</td>
      <td>MULTIPOLYGON (((127.032936062846 37.5068531522...</td>
      <td>2021-06-30</td>
    </tr>
  </tbody>
</table>
</div>


## 상가업소 정보 조회하기


```python
# 4-4. 행정구역상권
category = "행정구역상권"

divId = 'adongCd'
key = '1168058000'

df = si.read_data(category=category,divId=divId, key=key)
df.head(1)
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
      <td>9290</td>
      <td>삼성역_3</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>74618</td>
      <td>13</td>
      <td>MULTIPOLYGON (((127.065749573729 37.5141311587...</td>
      <td>2021-06-30</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-5. 단일상가
category = "단일상가"

key = '11757465'

df = si.read_data(category=category, key=key)
df.head(1)
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
      <th>표준산업분류명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>행정동코드</th>
      <th>행정동명</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>PNU코드</th>
      <th>대지구분코드</th>
      <th>대지구분명</th>
      <th>지번본번지</th>
      <th>지번부번지</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물부번지</th>
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
      <td>비알콜 음료점업</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165062100</td>
      <td>방배4동</td>
      <td>1165010100</td>
      <td>방배동</td>
      <td>1165010100108120002</td>
      <td>1</td>
      <td>대지</td>
      <td>812</td>
      <td>2</td>
      <td>서울특별시 서초구 방배동 812-2</td>
      <td>116503121010</td>
      <td>서울특별시 서초구 방배로</td>
      <td>211</td>
      <td></td>
      <td>1165010100108120002009897</td>
      <td></td>
      <td>서울특별시 서초구 방배로 211</td>
      <td>137060</td>
      <td>06562</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>126.99050928001</td>
      <td>37.4919441682448</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-6. 건물상가
category = "건물상가"

key = '1168011000104940000004966'

df = si.read_data(category=category, key=key)
df.head(1)
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
      <th>표준산업분류명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>행정동코드</th>
      <th>행정동명</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>PNU코드</th>
      <th>대지구분코드</th>
      <th>대지구분명</th>
      <th>지번본번지</th>
      <th>지번부번지</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물부번지</th>
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
      <td>11802586</td>
      <td>브루클린더버거조인트</td>
      <td>갤러리아점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q07</td>
      <td>패스트푸드</td>
      <td>Q07A04</td>
      <td>패스트푸드</td>
      <td>I56199</td>
      <td>그외 기타 음식점업</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>1168054500</td>
      <td>압구정동</td>
      <td>1168011000</td>
      <td>압구정동</td>
      <td>1168011000104940000</td>
      <td>1</td>
      <td>대지</td>
      <td>494</td>
      <td></td>
      <td>서울특별시 강남구 압구정동 494</td>
      <td>116803122007</td>
      <td>서울특별시 강남구 압구정로</td>
      <td>343</td>
      <td></td>
      <td>1168011000104940000004966</td>
      <td>갤러리아백화점</td>
      <td>서울특별시 강남구 압구정로 343</td>
      <td>135902</td>
      <td>06008</td>
      <td></td>
      <td>5</td>
      <td></td>
      <td>127.04008070443</td>
      <td>37.5284986430328</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-7. 지번상가
category = "지번상가"

key = '1165010100108120002'
indsLclsCd = 'Q'

df = si.read_data(category=category, key=key, indsLclsCd=indsLclsCd)
df.head(1)
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
      <th>표준산업분류명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>행정동코드</th>
      <th>행정동명</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>PNU코드</th>
      <th>대지구분코드</th>
      <th>대지구분명</th>
      <th>지번본번지</th>
      <th>지번부번지</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물부번지</th>
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
      <td>비알콜 음료점업</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165062100</td>
      <td>방배4동</td>
      <td>1165010100</td>
      <td>방배동</td>
      <td>1165010100108120002</td>
      <td>1</td>
      <td>대지</td>
      <td>812</td>
      <td>2</td>
      <td>서울특별시 서초구 방배동 812-2</td>
      <td>116503121010</td>
      <td>서울특별시 서초구 방배로</td>
      <td>211</td>
      <td></td>
      <td>1165010100108120002009897</td>
      <td></td>
      <td>서울특별시 서초구 방배로 211</td>
      <td>137060</td>
      <td>06562</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>126.99050928001</td>
      <td>37.4919441682448</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-8. 행정동상가
category = "행정동상가"

divId = 'adongCd'
key = '1168064000'
indsLclsCd = 'Q'

df = si.read_data(category=category, divId=divId, key=key, indsLclsCd=indsLclsCd)
df.head(1)
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
      <th>표준산업분류명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>행정동코드</th>
      <th>행정동명</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>PNU코드</th>
      <th>대지구분코드</th>
      <th>대지구분명</th>
      <th>지번본번지</th>
      <th>지번부번지</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물부번지</th>
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
      <td>10395773</td>
      <td>김가네</td>
      <td>르네상스점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q04</td>
      <td>분식</td>
      <td>Q04A01</td>
      <td>라면김밥분식</td>
      <td>I56194</td>
      <td>분식 및 김밥 전문점</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>1168064000</td>
      <td>역삼1동</td>
      <td>1168010100</td>
      <td>역삼동</td>
      <td>1168010100107000000</td>
      <td>1</td>
      <td>대지</td>
      <td>700</td>
      <td></td>
      <td>서울특별시 강남구 역삼동 700</td>
      <td>116803005086</td>
      <td>서울특별시 강남구 언주로</td>
      <td>520</td>
      <td></td>
      <td>1168010100107000001022298</td>
      <td></td>
      <td>서울특별시 강남구 언주로 520</td>
      <td>135080</td>
      <td>06147</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.042045125528</td>
      <td>37.5049340266371</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-9. 상권상가
category = "상권상가"

key = '9368'
indsLclsCd = 'Q'

df = si.read_data(category=category, key=key, indsLclsCd=indsLclsCd)
df.head(1)
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
      <th>표준산업분류명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>행정동코드</th>
      <th>행정동명</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>PNU코드</th>
      <th>대지구분코드</th>
      <th>대지구분명</th>
      <th>지번본번지</th>
      <th>지번부번지</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물부번지</th>
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
      <td>11913254</td>
      <td>에스엠커피</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>비알콜 음료점업</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>1168064000</td>
      <td>역삼1동</td>
      <td>1168010100</td>
      <td>역삼동</td>
      <td>1168010100106370019</td>
      <td>1</td>
      <td>대지</td>
      <td>637</td>
      <td>19</td>
      <td>서울특별시 강남구 역삼동 637-19</td>
      <td>116804166718</td>
      <td>서울특별시 강남구 테헤란로13길</td>
      <td>16</td>
      <td></td>
      <td>1168010100106370019023581</td>
      <td></td>
      <td>서울특별시 강남구 테헤란로13길 16</td>
      <td>135080</td>
      <td>06131</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.032327172938</td>
      <td>37.5007369962584</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-10. 반경상가
category = "반경상가"

radius = '500'
cx = 127.03641615737838
cy = 37.50059843782878
indsLclsCd = 'Q'

df = si.read_data(category=category, radius=radius, cx=cx, cy=cy, indsLclsCd=indsLclsCd)
df.head(1)
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
      <th>표준산업분류명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>행정동코드</th>
      <th>행정동명</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>PNU코드</th>
      <th>대지구분코드</th>
      <th>대지구분명</th>
      <th>지번본번지</th>
      <th>지번부번지</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물부번지</th>
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
      <td>10445804</td>
      <td>이바디</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>한식 음식점업</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>1168064000</td>
      <td>역삼1동</td>
      <td>1168010100</td>
      <td>역삼동</td>
      <td>1168010100107470010</td>
      <td>1</td>
      <td>대지</td>
      <td>747</td>
      <td>10</td>
      <td>서울특별시 강남구 역삼동 747-10</td>
      <td>116804166195</td>
      <td>서울특별시 강남구 논현로75길</td>
      <td>13</td>
      <td></td>
      <td>1168010100107470010024760</td>
      <td></td>
      <td>서울특별시 강남구 논현로75길 13</td>
      <td>135080</td>
      <td>06247</td>
      <td></td>
      <td></td>
      <td></td>
      <td>127.03771175216</td>
      <td>37.4962396611819</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-11. 사각형상가
category = "사각형상가"

minx = 127.0327683531071
miny = 37.495967935149146
maxx = 127.04268179746694
maxy = 37.502402894207286
indsLclsCd = 'Q'

df = si.read_data(category=category, minx=minx, miny=miny, maxx=maxx, maxy=maxy, indsLclsCd=indsLclsCd)
df.head(1)
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
      <th>표준산업분류명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>행정동코드</th>
      <th>행정동명</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>PNU코드</th>
      <th>대지구분코드</th>
      <th>대지구분명</th>
      <th>지번본번지</th>
      <th>지번부번지</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물부번지</th>
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
      <td>10445804</td>
      <td>이바디</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>한식 음식점업</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>1168064000</td>
      <td>역삼1동</td>
      <td>1168010100</td>
      <td>역삼동</td>
      <td>1168010100107470010</td>
      <td>1</td>
      <td>대지</td>
      <td>747</td>
      <td>10</td>
      <td>서울특별시 강남구 역삼동 747-10</td>
      <td>116804166195</td>
      <td>서울특별시 강남구 논현로75길</td>
      <td>13</td>
      <td></td>
      <td>1168010100107470010024760</td>
      <td></td>
      <td>서울특별시 강남구 논현로75길 13</td>
      <td>135080</td>
      <td>06247</td>
      <td></td>
      <td></td>
      <td></td>
      <td>127.03771175216</td>
      <td>37.4962396611819</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-12. 다각형상가
category = "다각형상가"

key = 'POLYGON((127.02355609555755 37.504264372557095, 127.02496157306963 37.50590702991155, 127.0270858825753 37.50486867039889, 127.02628121988377 37.503489842823114))'
indsLclsCd = 'Q'

df = si.read_data(category=category, key=key, indsLclsCd=indsLclsCd)
df.head(1)
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
      <th>표준산업분류명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>행정동코드</th>
      <th>행정동명</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>PNU코드</th>
      <th>대지구분코드</th>
      <th>대지구분명</th>
      <th>지번본번지</th>
      <th>지번부번지</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물부번지</th>
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
      <td>11766922</td>
      <td>투썸플레이스</td>
      <td>신논현역점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>비알콜 음료점업</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>11680</td>
      <td>강남구</td>
      <td>1168052100</td>
      <td>논현1동</td>
      <td>1168010800</td>
      <td>논현동</td>
      <td>1168010800102000007</td>
      <td>1</td>
      <td>대지</td>
      <td>200</td>
      <td>7</td>
      <td>서울특별시 강남구 논현동 200-7</td>
      <td>116802102001</td>
      <td>서울특별시 강남구 강남대로</td>
      <td>476</td>
      <td></td>
      <td>1168010800102000007000001</td>
      <td>URBANHIVE</td>
      <td>서울특별시 강남구 강남대로 476</td>
      <td>135010</td>
      <td>06120</td>
      <td></td>
      <td>1</td>
      <td></td>
      <td>127.024774692428</td>
      <td>37.5049008315565</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-13. 업종별상가
category = "업종별상가"

divId = 'indsLclsCd'
key = 'Q'

df = si.read_data(category=category, divId=divId, key=key)
df.head(1)
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
      <th>표준산업분류명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>행정동코드</th>
      <th>행정동명</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>PNU코드</th>
      <th>대지구분코드</th>
      <th>대지구분명</th>
      <th>지번본번지</th>
      <th>지번부번지</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물부번지</th>
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
      <td>한식 음식점업</td>
      <td>45</td>
      <td>전라북도</td>
      <td>45111</td>
      <td>전주시 완산구</td>
      <td>4511170200</td>
      <td>삼천2동</td>
      <td>4511113700</td>
      <td>삼천동1가</td>
      <td>4511113700106950005</td>
      <td>1</td>
      <td>대지</td>
      <td>695</td>
      <td>5</td>
      <td>전라북도 전주시 완산구 삼천동1가 695-5</td>
      <td>451114598396</td>
      <td>전라북도 전주시 완산구 하거마1길</td>
      <td>32</td>
      <td></td>
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
  </tbody>
</table>
</div>




```python
# 4-14. 수정일자상가
category = "수정일자상가"

key = '20200101'
indsLclsCd = 'Q'

df = si.read_data(category=category, key=key, indsLclsCd=indsLclsCd)
df.head(1)
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
      <th>표준산업분류명</th>
      <th>시도코드</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>시군구명</th>
      <th>행정동코드</th>
      <th>행정동명</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>PNU코드</th>
      <th>대지구분코드</th>
      <th>대지구분명</th>
      <th>지번본번지</th>
      <th>지번부번지</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물부번지</th>
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
      <td>16390214</td>
      <td>IN:SSABar</td>
      <td></td>
      <td>Q</td>
      <td>음식</td>
      <td>Q09</td>
      <td>유흥주점</td>
      <td>Q09A01</td>
      <td>호프/맥주</td>
      <td>I56219</td>
      <td>기타 주점업</td>
      <td>46</td>
      <td>전라남도</td>
      <td>46130</td>
      <td>여수시</td>
      <td>4613078000</td>
      <td>쌍봉동</td>
      <td>4613012800</td>
      <td>학동</td>
      <td>4613012800100940005</td>
      <td>1</td>
      <td>대지</td>
      <td>94</td>
      <td>5</td>
      <td>전라남도 여수시 학동 94-5</td>
      <td>461304646589</td>
      <td>전라남도 여수시 시청동3길</td>
      <td>20</td>
      <td></td>
      <td>4613012800100940005034312</td>
      <td></td>
      <td>전라남도 여수시 시청동3길 20</td>
      <td>555809</td>
      <td>59689</td>
      <td></td>
      <td>2</td>
      <td></td>
      <td>127.665098299541</td>
      <td>34.7585551004235</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-15. 업종대분류
category = "업종대분류"

df = si.read_data(category=category, key=key)
df.head(1)
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
      <th>0</th>
      <td>A</td>
      <td>1차산업</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-16. 업종중분류
category = "업종중분류"

indsLclsCd = 'Q'

df = si.read_data(category=category, key=key, indsLclsCd=indsLclsCd)
df.head(1)
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
      <th>0</th>
      <td>Q</td>
      <td>보건</td>
      <td>Q14</td>
      <td>기타음식업</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4-17. 업종소분류
category = "업종소분류"

indsLclsCd = 'Q'
indsMclsCd = 'Q01'

df = si.read_data(category=category, key=key, indsLclsCd=indsLclsCd, indsMclsCd=indsMclsCd)
df.head(1)
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
      <th>0</th>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A02</td>
      <td>갈비/삼겹살</td>
      <td>2015-12-17</td>
    </tr>
  </tbody>
</table>
</div>











<br>

### 참고 
- **[GitHub](https://github.com/WooilJeong/PublicDataReader)**  

- **사용설명서**  
  - [(블로그) 부동산 실거래가 조회하기](https://wooiljeong.github.io/python/public_data_reader_01/)
  - [(블로그) 건축물대장 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_03/)
  - [(블로그) 상가업소 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_02/)

- **실습**  
  - [Colab에서 PublicDataReader 실행하기](https://colab.research.google.com/drive/1fgT0D_tP-JyglobtDFfYQ6wQXfWWujIV?usp=sharing)  

- **문의**  
  - **이메일**: wooil@kakao.com  
  - **카카오톡 오픈채팅방**: [(Python) PublicDataReader Q&A](https://open.kakao.com/o/gbt2Pl2d)  

