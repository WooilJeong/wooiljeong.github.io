---
title: PublicDataReader - 주민등록인구 데이터 조회하기
categories: python
tags: PublicDataReader
header:
  overlay_image: /assets/img/logo/PublicDataReader.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## 파이썬으로 주민등록인구 데이터 조회하기

![PNG](https://github.com/WooilJeong/PublicDataReader/blob/main/assets/img/logo.png?raw=true)

Python 라이브러리 PublicDataReader를 이용하면 최신 지역별 주민등록인구 데이터를 쉽게 가져올 수 있다. 해당 데이터는 국가통계포털(KOSIS) 웹 사이트에 접속해 직접 조회할 수도 있지만, KOSIS 공유서비스 Open API를 이용해서도 조회할 수 있다. PublicDataReader는 KOSIS 공유서비스 Open API의 데이터 조회 기능을 포함하고 있어, 이를 활용하면 원하는 데이터를 쉽게 찾고 조회할 수 있다. KOSIS 공유서비스 Open API 신청 방법과 PublicDataReader에서 제공하는 KOSIS 공유서비스 Open API의 모든 기능을 살펴보려면 [Python으로 KOSIS 데이터 조회하기](https://wooiljeong.github.io/python/pdr-kosis/)를 참고하면 된다. 여기서는 주민등록인구 데이터를 조회하는 방법을 다루기로 한다.

<br>

## 설치하기

- 운영체제(OS)에 따라 아래 중 하나를 선택한다.
    - Windows: CMD(명령 프롬프트) 실행
    - Mac: Terminal(터미널) 실행

아래 Shell 명령어를 입력 후 실행한다.

```bash
pip install PublicDataReader --upgrade
```

<br>

## 라이브러리 임포트하기

앞에서 설치한 PublicDataReader에서 Kosis 클래스를 임포트한다. KOSIS 공유서비스에서 발급받은 오픈 API 사용자 인증키 정보를 service_key 변수에 할당한다. Kosis의 인자료 service_key를 입력하여 데이터 조회 인스턴스 api를 만든다.


```python
from PublicDataReader import Kosis

# KOSIS 공유서비스 Open API 사용자 인증키
service_key = "사용자 인증키"

# 인스턴스 생성하기
api = Kosis(service_key)
```

<br>

## 조회할 데이터 찾기

api.get_data() 메서드의 첫 번째 인자로 'KOSIS통합검색'을 지정한다. `searchNm`에는 조회할 데이터를 찾기 위한 키워드를 입력한다. 반환된 데이터프레임에서 조회할 데이터를 찾아 **기관ID(ORG_ID)**와 **통계표ID(TBL_ID)** 값을 확인한다.  **'행정구역(읍면동)별/5세별 주민등록인구(2011년~)'** 데이터의 **기관ID**는 **101**이고, **통계표ID**는 **DT_1B04005N** 이므로 이 값들을 데이터를 조회할 때 사용한다. 참고로 api.get_data() 메서드에 translate=False 옵션을 지정하면 영문 컬럼명으로 데이터를 조회할 수 있다. 이 인자의 기본값은 True이므로 국문 컬럼명으로 조회한다.


```python
df = api.get_data(
    "KOSIS통합검색",
    searchNm="읍면동 주민등록 인구"
)
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
      <th>기관ID</th>
      <th>기관명</th>
      <th>통계표ID</th>
      <th>통계표명</th>
      <th>조사ID</th>
      <th>조사명</th>
      <th>KOSIS목록구분</th>
      <th>KOSIS통계표위치</th>
      <th>통계표위치</th>
      <th>통계표주요내용</th>
      <th>수록기간시작일</th>
      <th>수록기간종료일</th>
      <th>통계표주석</th>
      <th>추천통계표여부</th>
      <th>KOSIS목록URL</th>
      <th>KOSIS통계표URL</th>
      <th>검색결과건수</th>
      <th>검색어명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>101</td>
      <td>행정안전부</td>
      <td>DT_1B04005N</td>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>2008001</td>
      <td>주민등록인구현황</td>
      <td>MT_ZTITLE</td>
      <td>인구 &gt; 주민등록인구현황</td>
      <td>A &gt; A_7</td>
      <td>행정구역(동읍면)별 5세별 남자인구수 여자인구수 총인구수 개포3동 신당제4동 중림동...</td>
      <td>2011</td>
      <td>2022</td>
      <td>* 등록구분의 ＂전체＂는 ＂거주자＂, ＂거주불명자＂, ＂재외국민＂이 포함된 자료입니...</td>
      <td>N</td>
      <td>https://kosis.kr/statisticsList/statisticsList...</td>
      <td>http://kosis.kr/statHtml/statHtml.do?orgId=101...</td>
      <td>573</td>
      <td>읍면동 주민등록 인구</td>
    </tr>
    <tr>
      <th>1</th>
      <td>101</td>
      <td>통계청</td>
      <td>INH_1IN1503_01</td>
      <td>인구총조사 인구(시도/시/군/구)</td>
      <td>1962001</td>
      <td>인구총조사</td>
      <td>MT_GTITLE01</td>
      <td>주제별 &gt; 인구</td>
      <td>101</td>
      <td>행정구역별(읍면동) 총인구(명) 면부 동대문구 노원구 마포구 양천구 읍부 남구 연제...</td>
      <td>2015</td>
      <td>2021</td>
      <td>■ 자료제공처: 통계청 통계정책과 □ 참고사항 - 주민등록에 의한 인구와는 차이가 ...</td>
      <td>N</td>
      <td>https://kosis.kr/statisticsList/statisticsList...</td>
      <td>http://kosis.kr/statHtml/statHtml.do?orgId=101...</td>
      <td>573</td>
      <td>읍면동 주민등록 인구</td>
    </tr>
    <tr>
      <th>2</th>
      <td>101</td>
      <td>행정안전부</td>
      <td>DT_1B04005</td>
      <td>행정구역(읍면동)별/5세별 주민등록인구</td>
      <td>2008001</td>
      <td>주민등록인구현황</td>
      <td>MT_ZTITLE</td>
      <td>인구 &gt; 주민등록인구현황</td>
      <td>A &gt; A_7</td>
      <td>행정구역(동읍면)별 5세별 인구 남 여 여주읍오학출장소 월배1동 종고동 연등동 광무...</td>
      <td>1992</td>
      <td>2010</td>
      <td>행정안전부 02-2100-3825(전국 주민등록인구현황) 주민등록에 의한 집계(연말...</td>
      <td>N</td>
      <td>https://kosis.kr/statisticsList/statisticsList...</td>
      <td>http://kosis.kr/statHtml/statHtml.do?orgId=101...</td>
      <td>573</td>
      <td>읍면동 주민등록 인구</td>
    </tr>
    <tr>
      <th>3</th>
      <td>101</td>
      <td>서울특별시</td>
      <td>INH_1B04005_11</td>
      <td>서울특별시(읍면동)별/5세별 주민등록인구</td>
      <td>MT_OTITLE13</td>
      <td>시도통계</td>
      <td>MT_ZTITLE</td>
      <td>지역통계 &gt; 인구 및 사회(사회조사 외) &gt; 서울특별시 &gt; 주민등록인구통계</td>
      <td>V &gt; V_3 &gt; V_3_201 &gt; 201_20103</td>
      <td>행정구역(동읍면)별 5세별 여 남 인구 숭인1동 을지로3・4・5가동 청구동 신당5동...</td>
      <td>1992</td>
      <td>2010</td>
      <td>DT_1B04005 (행정구역(읍면동)별/5세별 주민등록인구(2011년~))에서 서...</td>
      <td>N</td>
      <td>https://kosis.kr/statisticsList/statisticsList...</td>
      <td>http://kosis.kr/statHtml/statHtml.do?orgId=101...</td>
      <td>573</td>
      <td>읍면동 주민등록 인구</td>
    </tr>
    <tr>
      <th>4</th>
      <td>617</td>
      <td>경기도 평택시</td>
      <td>DT_61701_B000016</td>
      <td>주민등록 세대 및 인구</td>
      <td>MT_OTITLE12</td>
      <td>시군구기본통계</td>
      <td>MT_ZTITLE</td>
      <td>지역통계 &gt; 지자체 기본통계 &gt; 경기도 &gt; 경기도평택시기본통계 &gt; 인구</td>
      <td>V &gt; V_1 &gt; V_1_210 &gt; 210_210A_617_61701 &gt; 210_2...</td>
      <td>읍면동별 세대및인구현황별 주민등록 세대 및 인구 고덕면 신평동 비전1동 팽성읍 청북...</td>
      <td>2013</td>
      <td>2020</td>
      <td>자료 : 민원행정과</td>
      <td>N</td>
      <td>https://kosis.kr/statisticsList/statisticsList...</td>
      <td>http://kosis.kr/statHtml/statHtml.do?orgId=617...</td>
      <td>573</td>
      <td>읍면동 주민등록 인구</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 데이터 수록 주기와 시점 확인

api.get_data() 메서드의 첫 번째 인자로 '통계표설명'을 지정한다. 통계표설명의 경우 상세기능도 추가적으로 지정해야 한다. 두 번째 인자로 '자료갱신일'을 입력하고, 위에서 복사해둔 기관ID와 통계표ID의 값들을 api.get_data()의 인자로 입력해 데이터 수록주기와 수록시점을 확인한다. 수록주기(PRD_SE) 별 최근 수록시점(PRD_DE) 값을 조회하면 수록주기가 년인 경우는 2022년, 수록주기가 월인 경우는 2022년 11월이 최신 데이터임을 알 수 있다.


```python
df = api.get_data(
    "통계표설명",
    "자료갱신일",
    orgId="101",
    tblId="DT_1B04005N"
)

df.groupby(by=['수록주기']).agg({"수록시점": ["min", "max"]})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="2" halign="left">수록시점</th>
    </tr>
    <tr>
      <th></th>
      <th>min</th>
      <th>max</th>
    </tr>
    <tr>
      <th>수록주기</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>년</th>
      <td>2011</td>
      <td>2022</td>
    </tr>
    <tr>
      <th>월</th>
      <td>201207</td>
      <td>202212</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 데이터 항목 및 분류 확인하기

api.get_data() 메서드의 첫 번째 인자로 '통계표설명'을 지정한다. 두 번째 인자로 '분류항목'을 입력하고, 이번에도 기관ID와 통계표ID를 api.get_data()의 인자로 입력해 결과를 확인한다. 분류ID의 값이 'ITEM' 이면 항목을 뜻하고 이 때, 분류값ID의 값을 통계자료 조회 시 itmId의 값으로 입력한다. 분류ID 값이 `ITEM` 이 아니고, 분류값순번 값에 숫자가 입력되어 있는 경우 이는 분류를 뜻하고, 분류값순번의 숫자 값이 분류 수준을 뜻한다. 예를 들어, 분류ID 값이 A이고, 분류값순번 값이 1이라면, 분류ID 값을 통계자료 조회 시 objL1의 값으로 입력한다. 


```python
item = api.get_data(
    "통계표설명",
    "분류항목",
    orgId="101",
    tblId="DT_1B04005N",
)
item.head()
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
      <th>기관ID</th>
      <th>통계표ID</th>
      <th>코드ID</th>
      <th>코드명</th>
      <th>분류ID</th>
      <th>분류명</th>
      <th>분류영문명</th>
      <th>분류값ID</th>
      <th>분류값명</th>
      <th>분류값영문명</th>
      <th>상위분류값ID</th>
      <th>분류값순번</th>
      <th>CD_ENG_NM</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>14STD04553</td>
      <td>명</td>
      <td>ITEM</td>
      <td>항목</td>
      <td>Item code list</td>
      <td>T2</td>
      <td>총인구수</td>
      <td>Population</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Person</td>
    </tr>
    <tr>
      <th>1</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>14STD04553</td>
      <td>명</td>
      <td>ITEM</td>
      <td>항목</td>
      <td>Item code list</td>
      <td>T3</td>
      <td>남자인구수</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Person</td>
    </tr>
    <tr>
      <th>2</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>14STD04553</td>
      <td>명</td>
      <td>ITEM</td>
      <td>항목</td>
      <td>Item code list</td>
      <td>T4</td>
      <td>여자인구수</td>
      <td>Female</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Person</td>
    </tr>
    <tr>
      <th>3</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>00</td>
      <td>전국</td>
      <td>Whole country</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>Seoul</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



통계표 조회 시 항목은 총인구수, 남자인구수 그리고 여자인구수 모두 사용하기로 한다. 분류값ID의 값인 T2, T3 그리고 T4를 모두 복사해둔다.


```python
item.loc[item["분류ID"]=="ITEM"]
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
      <th>기관ID</th>
      <th>통계표ID</th>
      <th>코드ID</th>
      <th>코드명</th>
      <th>분류ID</th>
      <th>분류명</th>
      <th>분류영문명</th>
      <th>분류값ID</th>
      <th>분류값명</th>
      <th>분류값영문명</th>
      <th>상위분류값ID</th>
      <th>분류값순번</th>
      <th>CD_ENG_NM</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>14STD04553</td>
      <td>명</td>
      <td>ITEM</td>
      <td>항목</td>
      <td>Item code list</td>
      <td>T2</td>
      <td>총인구수</td>
      <td>Population</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Person</td>
    </tr>
    <tr>
      <th>1</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>14STD04553</td>
      <td>명</td>
      <td>ITEM</td>
      <td>항목</td>
      <td>Item code list</td>
      <td>T3</td>
      <td>남자인구수</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Person</td>
    </tr>
    <tr>
      <th>2</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>14STD04553</td>
      <td>명</td>
      <td>ITEM</td>
      <td>항목</td>
      <td>Item code list</td>
      <td>T4</td>
      <td>여자인구수</td>
      <td>Female</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Person</td>
    </tr>
  </tbody>
</table>
</div>



분류1의 경우 전체 선택하기로 한다. 분류값ID를 일부 선택하려면 '00 11 11110' 과 같이 공백으로 구분해 입력해야 하지만 전체 선택의 경우 'ALL' 이라고 입력한다. 여기에서는 종류가 많으므로 직접 값을 복사하지 않고 넘어간다.


```python
item.loc[item["분류ID"]=="A"]
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
      <th>기관ID</th>
      <th>통계표ID</th>
      <th>코드ID</th>
      <th>코드명</th>
      <th>분류ID</th>
      <th>분류명</th>
      <th>분류영문명</th>
      <th>분류값ID</th>
      <th>분류값명</th>
      <th>분류값영문명</th>
      <th>상위분류값ID</th>
      <th>분류값순번</th>
      <th>CD_ENG_NM</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>00</td>
      <td>전국</td>
      <td>Whole country</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>Seoul</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>11110</td>
      <td>종로구</td>
      <td>Jongno-gu</td>
      <td>11</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>1111051500</td>
      <td>청운효자동</td>
      <td>Cheongunhyoja-dong</td>
      <td>11110</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>1111053000</td>
      <td>사직동</td>
      <td>Sajik-dong</td>
      <td>11110</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4182</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>5013058000</td>
      <td>서홍동</td>
      <td>Seohong-dong</td>
      <td>50130</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4183</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>5013059000</td>
      <td>대륜동</td>
      <td>Daeryun-dong</td>
      <td>50130</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4184</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>5013060000</td>
      <td>대천동</td>
      <td>Daecheon-dong</td>
      <td>50130</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4185</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>5013061000</td>
      <td>중문동</td>
      <td>Jungmun-dong</td>
      <td>50130</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4186</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>5013062000</td>
      <td>예래동</td>
      <td>Yerae-dong</td>
      <td>50130</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>4184 rows × 13 columns</p>
</div>



분류2의 경우 연령별 구분은 사용하지 않고 총계만 조회하기로 한다. 따라서 분류값ID 값이 0인 경우만 해당하므로 0을 복사해둔다.


```python
item.loc[item["분류ID"]=="B"]
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
      <th>기관ID</th>
      <th>통계표ID</th>
      <th>코드ID</th>
      <th>코드명</th>
      <th>분류ID</th>
      <th>분류명</th>
      <th>분류영문명</th>
      <th>분류값ID</th>
      <th>분류값명</th>
      <th>분류값영문명</th>
      <th>상위분류값ID</th>
      <th>분류값순번</th>
      <th>CD_ENG_NM</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4187</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>0</td>
      <td>계</td>
      <td>Total</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4188</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>5</td>
      <td>0 - 4세</td>
      <td>0-4 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4189</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>10</td>
      <td>5 - 9세</td>
      <td>5-9 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4190</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>15</td>
      <td>10 - 14세</td>
      <td>10-14 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4191</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>20</td>
      <td>15 - 19세</td>
      <td>15-19 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4192</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>25</td>
      <td>20 - 24세</td>
      <td>20-24 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4193</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>30</td>
      <td>25 - 29세</td>
      <td>25-29 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4194</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>35</td>
      <td>30 - 34세</td>
      <td>30-34 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4195</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>40</td>
      <td>35 - 39세</td>
      <td>35-39 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4196</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>45</td>
      <td>40 - 44세</td>
      <td>40-44 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4197</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>50</td>
      <td>45 - 49세</td>
      <td>45-49 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4198</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>55</td>
      <td>50 - 54세</td>
      <td>50-54 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4199</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>60</td>
      <td>55 - 59세</td>
      <td>55-59 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4200</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>65</td>
      <td>60 - 64세</td>
      <td>60-64 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4201</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>70</td>
      <td>65 - 69세</td>
      <td>65-69 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4202</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>75</td>
      <td>70 - 74세</td>
      <td>70-74 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4203</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>80</td>
      <td>75 - 79세</td>
      <td>75-79 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4204</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>85</td>
      <td>80 - 84세</td>
      <td>80-84 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4205</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>90</td>
      <td>85 - 89세</td>
      <td>85-89 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4206</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>95</td>
      <td>90 - 94세</td>
      <td>90-94 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4207</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>100</td>
      <td>95 - 99세</td>
      <td>95-99 Years old</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4208</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>105</td>
      <td>100+</td>
      <td>100 Years old ＆ over</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 통계표 조회하기

api.get_data() 메서드의 첫 번째 인자로 '통계자료'를 지정한다. **`행정구역(읍면동)별/5세별 주민등록인구(2011년~)`** 통계표를 조회를 위해 위에서 복사해둔 값들을 순서대로 api.get_data()의 인자로 입력한다. itmId의 값으로 모든 항목값을 입력했지만, objL1과 같이 'ALL'이라고 입력해도 전체 선택이 된다.


```python
df = api.get_data(
    "통계자료",
    orgId="101",
    tblId="DT_1B04005N",
    objL1="ALL",
    objL2="0",
    itmId="T2 T3 T4",
    prdSe="M",
    startPrdDe="202211",
    endPrdDe="202211",
)
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
      <th>기관ID</th>
      <th>통계표ID</th>
      <th>통계표명</th>
      <th>분류명1</th>
      <th>분류영문명1</th>
      <th>분류값명1</th>
      <th>분류값영문명1</th>
      <th>분류값ID1</th>
      <th>분류명2</th>
      <th>분류영문명2</th>
      <th>분류값명2</th>
      <th>분류값영문명2</th>
      <th>분류값ID2</th>
      <th>항목ID</th>
      <th>항목명</th>
      <th>항목영문명</th>
      <th>단위명</th>
      <th>단위영문명</th>
      <th>수록주기</th>
      <th>수록시점</th>
      <th>수치값</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>전국</td>
      <td>Whole country</td>
      <td>00</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>계</td>
      <td>Total</td>
      <td>0</td>
      <td>T2</td>
      <td>총인구수</td>
      <td>Population</td>
      <td>명</td>
      <td>Person</td>
      <td>M</td>
      <td>202211</td>
      <td>51450829</td>
    </tr>
    <tr>
      <th>1</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>전국</td>
      <td>Whole country</td>
      <td>00</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>계</td>
      <td>Total</td>
      <td>0</td>
      <td>T3</td>
      <td>남자인구수</td>
      <td>Male</td>
      <td>명</td>
      <td>Person</td>
      <td>M</td>
      <td>202211</td>
      <td>25643889</td>
    </tr>
    <tr>
      <th>2</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>전국</td>
      <td>Whole country</td>
      <td>00</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>계</td>
      <td>Total</td>
      <td>0</td>
      <td>T4</td>
      <td>여자인구수</td>
      <td>Female</td>
      <td>명</td>
      <td>Person</td>
      <td>M</td>
      <td>202211</td>
      <td>25806940</td>
    </tr>
    <tr>
      <th>3</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>서울특별시</td>
      <td>Seoul</td>
      <td>11</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>계</td>
      <td>Total</td>
      <td>0</td>
      <td>T2</td>
      <td>총인구수</td>
      <td>Population</td>
      <td>명</td>
      <td>Person</td>
      <td>M</td>
      <td>202211</td>
      <td>9436836</td>
    </tr>
    <tr>
      <th>4</th>
      <td>101</td>
      <td>DT_1B04005N</td>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>서울특별시</td>
      <td>Seoul</td>
      <td>11</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>계</td>
      <td>Total</td>
      <td>0</td>
      <td>T3</td>
      <td>남자인구수</td>
      <td>Male</td>
      <td>명</td>
      <td>Person</td>
      <td>M</td>
      <td>202211</td>
      <td>4574781</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 피벗 테이블 만들기

통계표 조회 결과를 아래와 같이 원하는 형태로 재구조화하여 분석 목적에 맞게 사용하면 된다.


```python
pv = df.pivot(index=["분류값ID1","분류값명1","수록시점"], columns=["항목명"], values="수치값").reset_index()
pv.columns.name = None
pv['수록시점'] = pd.to_datetime(pv['수록시점'], format="%Y%m")
numCols = ["남자인구수","여자인구수","총인구수"]
for col in numCols:
    pv[col] = pd.to_numeric(pv[col])
pv
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
      <th>분류값ID1</th>
      <th>분류값명1</th>
      <th>수록시점</th>
      <th>남자인구수</th>
      <th>여자인구수</th>
      <th>총인구수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>00</td>
      <td>전국</td>
      <td>2022-11-01</td>
      <td>25643889</td>
      <td>25806940</td>
      <td>51450829</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>2022-11-01</td>
      <td>4574781</td>
      <td>4862055</td>
      <td>9436836</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11110</td>
      <td>종로구</td>
      <td>2022-11-01</td>
      <td>68488</td>
      <td>73162</td>
      <td>141650</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1111051500</td>
      <td>청운효자동</td>
      <td>2022-11-01</td>
      <td>5366</td>
      <td>6311</td>
      <td>11677</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1111053000</td>
      <td>사직동</td>
      <td>2022-11-01</td>
      <td>4041</td>
      <td>5065</td>
      <td>9106</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3853</th>
      <td>5013058000</td>
      <td>서홍동</td>
      <td>2022-11-01</td>
      <td>5555</td>
      <td>5670</td>
      <td>11225</td>
    </tr>
    <tr>
      <th>3854</th>
      <td>5013059000</td>
      <td>대륜동</td>
      <td>2022-11-01</td>
      <td>7846</td>
      <td>7734</td>
      <td>15580</td>
    </tr>
    <tr>
      <th>3855</th>
      <td>5013060000</td>
      <td>대천동</td>
      <td>2022-11-01</td>
      <td>7030</td>
      <td>6828</td>
      <td>13858</td>
    </tr>
    <tr>
      <th>3856</th>
      <td>5013061000</td>
      <td>중문동</td>
      <td>2022-11-01</td>
      <td>6227</td>
      <td>6055</td>
      <td>12282</td>
    </tr>
    <tr>
      <th>3857</th>
      <td>5013062000</td>
      <td>예래동</td>
      <td>2022-11-01</td>
      <td>1987</td>
      <td>1896</td>
      <td>3883</td>
    </tr>
  </tbody>
</table>
<p>3858 rows × 6 columns</p>
</div>


<br>

## 참고

- [Python으로 KOSIS 데이터 조회하기](https://wooiljeong.github.io/python/pdr-kosis/)
- [KOSIS 국가통계포털 Open API 가이드](https://kosis.kr/serviceInfo/openAPIGuide.do)
- [KOSIS 공유서비스](https://kosis.kr/openapi/index/index.jsp)
- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)