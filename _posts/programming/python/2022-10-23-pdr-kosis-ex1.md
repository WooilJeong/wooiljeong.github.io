---
title: Python으로 주민등록인구 데이터 조회하기
categories: python
tags: PublicDataReader
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## Python으로 주민등록인구 데이터 조회하기

Python 라이브러리 PublicDataReader를 이용하면 최신 지역별 주민등록인구 데이터를 쉽게 가져올 수 있다. 해당 데이터는 국가통계포털(KOSIS) 웹 사이트에 접속해 직접 조회할 수도 있지만, KOSIS 공유서비스 Open API를 이용해서도 조회할 수 있다. PublicDataReader는 KOSIS 공유서비스 Open API의 데이터 조회 기능을 포함하고 있어, 이를 활용하면 원하는 데이터를 쉽게 찾고 조회할 수 있다. KOSIS 공유서비스 Open API 신청 방법과 PublicDataReader에서 제공하는 KOSIS 공유서비스 Open API의 모든 기능을 살펴보려면 [Python으로 KOSIS 데이터 조회하기](https://wooiljeong.github.io/python/pdr-kosis/)를 참고하면 된다. 여기서는 주민등록인구 데이터를 조회하는 방법을 다루기로 한다.

<br>

## 라이브러리 임포트하기

위에서 설치한 PublicDataReader를 임포트한 후 발급받은 KOSIS 공유서비스 Open API 사용자 인증키를 apiKey의 값으로 할당한다.


```python
import PublicDataReader as pdr
print(pdr.__version__)

# KOSIS 공유서비스 Open API 사용자 인증키
apiKey = "사용자 인증키"
```

    1.0.2
    

<br>

## 조회할 데이터 찾기

`Kosis`를 이용해 **KOSIS 통합검색** 기능을 사용할 수 있는 인스턴스인 `kosis_search`를 만든다. `searchNm`에는 조회할 데이터를 찾기 위한 키워드를 입력한다. `get_data`에 의해 반환된 데이터프레임에서 조회할 데이터를 찾아 **기관코드**인 **`ORG_ID`** 와 **통계표ID**인 **`TBL_ID`** 값을 확인한다.  **'행정구역(읍면동)별/5세별 주민등록인구(2011년~)'** 데이터의 **기관코드**는 **101**이고, **통계표ID**는 **DT_1B04005N** 이므로 이 값들을 데이터 조회 시 사용하기 위해 복사해둔다.


```python
# KOSIS 공유서비스 Open API 인스턴스 생성
serviceName = "KOSIS통합검색"
kosis_search = pdr.Kosis(apiKey, serviceName)

# 파라미터
searchNm = "읍면동 주민등록 인구"

# 데이터 조회
df = kosis_search.get_data(searchNm=searchNm)
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
      <th>ORG_ID</th>
      <th>ORG_NM</th>
      <th>TBL_ID</th>
      <th>TBL_NM</th>
      <th>STAT_ID</th>
      <th>STAT_NM</th>
      <th>VW_CD</th>
      <th>MT_ATITLE</th>
      <th>FULL_PATH_ID</th>
      <th>CONTENTS</th>
      <th>STRT_PRD_DE</th>
      <th>END_PRD_DE</th>
      <th>ITEM03</th>
      <th>REC_TBL_SE</th>
      <th>TBL_VIEW_URL</th>
      <th>LINK_URL</th>
      <th>STAT_DB_CNT</th>
      <th>QUERY</th>
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
      <td>행정구역(동읍면)별 5세별 총인구수 남자인구수 여자인구수 전국 서울특별시 종로구 청...</td>
      <td>2011</td>
      <td>2022</td>
      <td>* 주민등록 연령별 인구통계는 주민등록 신고에 따른 것으로 실제 연령과는 차이가 있...</td>
      <td>N</td>
      <td>https://kosis.kr/statisticsList/statisticsList...</td>
      <td>http://kosis.kr/statHtml/statHtml.do?orgId=101...</td>
      <td>542</td>
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
      <td>행정구역별(읍면동) 총인구(명) 전국 동부 읍부 면부 서울특별시 종로구 중구 용산구...</td>
      <td>2015</td>
      <td>2021</td>
      <td>■ 자료제공처: 통계청 통계정책과 □ 참고사항 - 주민등록에 의한 인구와는 차이가 ...</td>
      <td>N</td>
      <td>https://kosis.kr/statisticsList/statisticsList...</td>
      <td>http://kosis.kr/statHtml/statHtml.do?orgId=101...</td>
      <td>542</td>
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
      <td>행정구역(동읍면)별 5세별 인구 남 여 전국 서울특별시 종로구 청운동 청운효자동 효...</td>
      <td>1992</td>
      <td>2010</td>
      <td>행정안전부 02-2100-3825(전국 주민등록인구현황) 주민등록에 의한 집계(연말...</td>
      <td>N</td>
      <td>https://kosis.kr/statisticsList/statisticsList...</td>
      <td>http://kosis.kr/statHtml/statHtml.do?orgId=101...</td>
      <td>542</td>
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
      <td>행정구역(동읍면)별 5세별 인구 남 여 서울특별시 종로구 청운동 청운효자동 효자동 ...</td>
      <td>1992</td>
      <td>2010</td>
      <td>DT_1B04005 (행정구역(읍면동)별/5세별 주민등록인구(2011년~))에서 서...</td>
      <td>N</td>
      <td>https://kosis.kr/statisticsList/statisticsList...</td>
      <td>http://kosis.kr/statHtml/statHtml.do?orgId=101...</td>
      <td>542</td>
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
      <td>읍면동별 세대및인구현황별 주민등록 세대 및 인구 팽성읍 안중읍 포승읍 진위면 서탄면...</td>
      <td>2013</td>
      <td>2020</td>
      <td>자료 : 민원행정과</td>
      <td>N</td>
      <td>https://kosis.kr/statisticsList/statisticsList...</td>
      <td>http://kosis.kr/statHtml/statHtml.do?orgId=617...</td>
      <td>542</td>
      <td>읍면동 주민등록 인구</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 데이터 수록 주기와 시점 확인

`Kosis`를 이용해 **통계표 설명** 기능을 사용할 수 있는 인스턴스인 `kosis_desc`를 만든다. 상세기능 detailServiceName의 값으로 '자료갱신일'을 입력하고, 위에서 복사해둔 기관코드 orgId와 통계표ID tblId의 값들을 `get_data`의 인자로 입력하여 데이터 수록 주기와 시점을 확인한다. 수록주기(PRD_SE) 별 최근 수록시점(PRD_DE) 값을 조회하면 수록주기가 년인 경우는 2021년, 수록주기가 월인 경우는 2022년 9월이 최신 데이터임을 알 수 있다.


```python
# KOSIS OPEN API 인스턴스 생성
serviceName = "통계표설명"
kosis_desc = pdr.Kosis(apiKey, serviceName)

# 파라미터
detailServiceName = "자료갱신일"
orgId = "101"
tblId = "DT_1B04005N"

# 데이터 조회
df = kosis_desc.get_data(orgId=orgId, tblId=tblId, detailServiceName=detailServiceName)
df.groupby(by=['PRD_SE']).agg({"PRD_DE": ["min", "max"]})
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
      <th colspan="2" halign="left">PRD_DE</th>
    </tr>
    <tr>
      <th></th>
      <th>min</th>
      <th>max</th>
    </tr>
    <tr>
      <th>PRD_SE</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>년</th>
      <td>2011</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>월</th>
      <td>201207</td>
      <td>202209</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 데이터 항목 및 분류 확인하기

`Kosis`를 이용해 **통계표 설명** 기능을 사용할 수 있는 인스턴스인 `kosis_desc`를 만든다. 상세기능 detailServiceName의 값으로 '분류항목'을 입력하고, 이번에도 기관코드와 통계표ID를 `get_data`의 인자로 입력해 결과를 확인한다. **`OBJ_ID`** 는 분류ID로 이 값이 'ITEM' 이면 항목을 뜻하고, ITM_ID 값을 통계자료 조회 시 itmId 값으로 입력한다. **`OBJ_ID`** 값이 `ITEM` 이 아니고, **`OBJ_ID_SN`** 값에 숫자가 입력되어 있는 경우 이는 분류를 뜻하고, **`OBJ_ID_SN`** 의 숫자 값이 분류 수준을 뜻한다. 예를 들어, **`OBJ_ID`** 값이 A이고, **`OBJ_ID_SN`** 값이 1이라면, ITM_ID 값을 통계자료 조회 시 objL1의 값으로 입력한다. 


```python
# 파라미터
detailServiceName = "분류항목"
orgId = "101"
tblId = "DT_1B04005N"

# 데이터 조회
item = kosis_desc.get_data(orgId=orgId, tblId=tblId, detailServiceName=detailServiceName)
```

통계표 조회 시 항목은 총인구수, 남자인구수 그리고 여자인구수 모두 사용하기로 한다. ITM_ID의 값인 T2, T3 그리고 T4를 모두 복사해둔다.


```python
item.loc[item["OBJ_ID"]=="ITEM"]
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
      <th>ITM_NM</th>
      <th>TBL_ID</th>
      <th>ITM_NM_ENG</th>
      <th>ITM_ID</th>
      <th>OBJ_NM</th>
      <th>OBJ_NM_ENG</th>
      <th>ORG_ID</th>
      <th>OBJ_ID</th>
      <th>OBJ_ID_SN</th>
      <th>UP_ITM_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>총인구수</td>
      <td>DT_1B04005N</td>
      <td>Population</td>
      <td>T2</td>
      <td>항목</td>
      <td>Item code list</td>
      <td>101</td>
      <td>ITEM</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>남자인구수</td>
      <td>DT_1B04005N</td>
      <td>Male</td>
      <td>T3</td>
      <td>항목</td>
      <td>Item code list</td>
      <td>101</td>
      <td>ITEM</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>여자인구수</td>
      <td>DT_1B04005N</td>
      <td>Female</td>
      <td>T4</td>
      <td>항목</td>
      <td>Item code list</td>
      <td>101</td>
      <td>ITEM</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



분류1의 경우 전체 선택하기로 한다. ITM_ID를 일부 선택하려면 '00 11 11110' 과 같이 입력해야 하지만, 전체 선택의 경우 'ALL' 이라고 입력해도 된다. 여기에서는 종류가 많으므로 직접 값을 복사하지 않고 넘어간다.


```python
item.loc[item["OBJ_ID"]=="A"]
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
      <th>ITM_NM</th>
      <th>TBL_ID</th>
      <th>ITM_NM_ENG</th>
      <th>ITM_ID</th>
      <th>OBJ_NM</th>
      <th>OBJ_NM_ENG</th>
      <th>ORG_ID</th>
      <th>OBJ_ID</th>
      <th>OBJ_ID_SN</th>
      <th>UP_ITM_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>전국</td>
      <td>DT_1B04005N</td>
      <td>Whole country</td>
      <td>00</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>101</td>
      <td>A</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>서울특별시</td>
      <td>DT_1B04005N</td>
      <td>Seoul</td>
      <td>11</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>101</td>
      <td>A</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>종로구</td>
      <td>DT_1B04005N</td>
      <td>Jongno-gu</td>
      <td>11110</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>101</td>
      <td>A</td>
      <td>1</td>
      <td>11</td>
    </tr>
    <tr>
      <th>6</th>
      <td>청운효자동</td>
      <td>DT_1B04005N</td>
      <td>Cheongunhyoja-dong</td>
      <td>1111051500</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>101</td>
      <td>A</td>
      <td>1</td>
      <td>11110</td>
    </tr>
    <tr>
      <th>7</th>
      <td>사직동</td>
      <td>DT_1B04005N</td>
      <td>Sajik-dong</td>
      <td>1111053000</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>101</td>
      <td>A</td>
      <td>1</td>
      <td>11110</td>
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
    </tr>
    <tr>
      <th>4181</th>
      <td>서홍동</td>
      <td>DT_1B04005N</td>
      <td>Seohong-dong</td>
      <td>5013058000</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>101</td>
      <td>A</td>
      <td>1</td>
      <td>50130</td>
    </tr>
    <tr>
      <th>4182</th>
      <td>대륜동</td>
      <td>DT_1B04005N</td>
      <td>Daeryun-dong</td>
      <td>5013059000</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>101</td>
      <td>A</td>
      <td>1</td>
      <td>50130</td>
    </tr>
    <tr>
      <th>4183</th>
      <td>대천동</td>
      <td>DT_1B04005N</td>
      <td>Daecheon-dong</td>
      <td>5013060000</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>101</td>
      <td>A</td>
      <td>1</td>
      <td>50130</td>
    </tr>
    <tr>
      <th>4184</th>
      <td>중문동</td>
      <td>DT_1B04005N</td>
      <td>Jungmun-dong</td>
      <td>5013061000</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>101</td>
      <td>A</td>
      <td>1</td>
      <td>50130</td>
    </tr>
    <tr>
      <th>4185</th>
      <td>예래동</td>
      <td>DT_1B04005N</td>
      <td>Yerae-dong</td>
      <td>5013062000</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>101</td>
      <td>A</td>
      <td>1</td>
      <td>50130</td>
    </tr>
  </tbody>
</table>
<p>4183 rows × 10 columns</p>
</div>



분류2의 경우 연령별 구분은 사용하지 않고 총계만 조회하기로 한다. 따라서 ITM_ID 값이 0인 경우만 해당하므로 0을 복사해둔다.


```python
item.loc[item["OBJ_ID"]=="B"]
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
      <th>ITM_NM</th>
      <th>TBL_ID</th>
      <th>ITM_NM_ENG</th>
      <th>ITM_ID</th>
      <th>OBJ_NM</th>
      <th>OBJ_NM_ENG</th>
      <th>ORG_ID</th>
      <th>OBJ_ID</th>
      <th>OBJ_ID_SN</th>
      <th>UP_ITM_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4186</th>
      <td>계</td>
      <td>DT_1B04005N</td>
      <td>Total</td>
      <td>0</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4187</th>
      <td>0 - 4세</td>
      <td>DT_1B04005N</td>
      <td>0-4 Years old</td>
      <td>5</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4188</th>
      <td>5 - 9세</td>
      <td>DT_1B04005N</td>
      <td>5-9 Years old</td>
      <td>10</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4189</th>
      <td>10 - 14세</td>
      <td>DT_1B04005N</td>
      <td>10-14 Years old</td>
      <td>15</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4190</th>
      <td>15 - 19세</td>
      <td>DT_1B04005N</td>
      <td>15-19 Years old</td>
      <td>20</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4191</th>
      <td>20 - 24세</td>
      <td>DT_1B04005N</td>
      <td>20-24 Years old</td>
      <td>25</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4192</th>
      <td>25 - 29세</td>
      <td>DT_1B04005N</td>
      <td>25-29 Years old</td>
      <td>30</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4193</th>
      <td>30 - 34세</td>
      <td>DT_1B04005N</td>
      <td>30-34 Years old</td>
      <td>35</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4194</th>
      <td>35 - 39세</td>
      <td>DT_1B04005N</td>
      <td>35-39 Years old</td>
      <td>40</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4195</th>
      <td>40 - 44세</td>
      <td>DT_1B04005N</td>
      <td>40-44 Years old</td>
      <td>45</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4196</th>
      <td>45 - 49세</td>
      <td>DT_1B04005N</td>
      <td>45-49 Years old</td>
      <td>50</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4197</th>
      <td>50 - 54세</td>
      <td>DT_1B04005N</td>
      <td>50-54 Years old</td>
      <td>55</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4198</th>
      <td>55 - 59세</td>
      <td>DT_1B04005N</td>
      <td>55-59 Years old</td>
      <td>60</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4199</th>
      <td>60 - 64세</td>
      <td>DT_1B04005N</td>
      <td>60-64 Years old</td>
      <td>65</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4200</th>
      <td>65 - 69세</td>
      <td>DT_1B04005N</td>
      <td>65-69 Years old</td>
      <td>70</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4201</th>
      <td>70 - 74세</td>
      <td>DT_1B04005N</td>
      <td>70-74 Years old</td>
      <td>75</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4202</th>
      <td>75 - 79세</td>
      <td>DT_1B04005N</td>
      <td>75-79 Years old</td>
      <td>80</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4203</th>
      <td>80 - 84세</td>
      <td>DT_1B04005N</td>
      <td>80-84 Years old</td>
      <td>85</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4204</th>
      <td>85 - 89세</td>
      <td>DT_1B04005N</td>
      <td>85-89 Years old</td>
      <td>90</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4205</th>
      <td>90 - 94세</td>
      <td>DT_1B04005N</td>
      <td>90-94 Years old</td>
      <td>95</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4206</th>
      <td>95 - 99세</td>
      <td>DT_1B04005N</td>
      <td>95-99 Years old</td>
      <td>100</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4207</th>
      <td>100+</td>
      <td>DT_1B04005N</td>
      <td>100 Years old ＆ over</td>
      <td>105</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>101</td>
      <td>B</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 통계표 조회하기

`Kosis`를 이용해 **통계자료** 기능을 사용할 수 있는 인스턴스인 `kosis_data`를 만든다. **`행정구역(읍면동)별/5세별 주민등록인구(2011년~)`** 통계표를 조회를 위해 위에서 복사해둔 값들을 순서대로 `get_data`의 인자로 입력해준다. itmId의 값으로 모든 항목값을 입력했지만, objL1과 같이 'ALL'이라고 입력해도 전체 선택이 된다.


```python
# KOSIS OPEN API 인스턴스 생성
serviceName = "통계자료"
kosis_data = pdr.Kosis(apiKey, serviceName)

# 파라미터
orgId = "101"             # 기관코드
tblId = "DT_1B04005N"     # 통계표ID
objL1 = "ALL"             # 분류1 - 전체 선택
objL2 = "0"               # 분류2 - `계` 선택
itmId = "T2 T3 T4"        # 항목 - '총인구수', '남자인구수' 그리고 '여자인구수' 선택
prdSe = "M"               # 수록주기 - `1개월 주기` 선택
startPrdDe = "202209"     # 시작수록시점 (YYYYMM)
endPrdDe = "202209"       # 종료수록시점 (YYYYMM)

# 데이터 조회
df = kosis_data.get_data(orgId=orgId, tblId=tblId, objL1=objL1, objL2=objL2, itmId=itmId, prdSe=prdSe, startPrdDe=startPrdDe, endPrdDe=endPrdDe)
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
      <th>TBL_NM</th>
      <th>PRD_DE</th>
      <th>TBL_ID</th>
      <th>ITM_NM</th>
      <th>ITM_NM_ENG</th>
      <th>ITM_ID</th>
      <th>UNIT_NM</th>
      <th>ORG_ID</th>
      <th>UNIT_NM_ENG</th>
      <th>C1_OBJ_NM</th>
      <th>C1_OBJ_NM_ENG</th>
      <th>C2_OBJ_NM</th>
      <th>C2_OBJ_NM_ENG</th>
      <th>DT</th>
      <th>PRD_SE</th>
      <th>C2</th>
      <th>C1</th>
      <th>C1_NM</th>
      <th>C2_NM</th>
      <th>C1_NM_ENG</th>
      <th>C2_NM_ENG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>202209</td>
      <td>DT_1B04005N</td>
      <td>총인구수</td>
      <td>Population</td>
      <td>T2</td>
      <td>명</td>
      <td>101</td>
      <td>Person</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>51466658</td>
      <td>M</td>
      <td>0</td>
      <td>00</td>
      <td>전국</td>
      <td>계</td>
      <td>Whole country</td>
      <td>Total</td>
    </tr>
    <tr>
      <th>1</th>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>202209</td>
      <td>DT_1B04005N</td>
      <td>남자인구수</td>
      <td>Male</td>
      <td>T3</td>
      <td>명</td>
      <td>101</td>
      <td>Person</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>25653998</td>
      <td>M</td>
      <td>0</td>
      <td>00</td>
      <td>전국</td>
      <td>계</td>
      <td>Whole country</td>
      <td>Total</td>
    </tr>
    <tr>
      <th>2</th>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>202209</td>
      <td>DT_1B04005N</td>
      <td>여자인구수</td>
      <td>Female</td>
      <td>T4</td>
      <td>명</td>
      <td>101</td>
      <td>Person</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>25812660</td>
      <td>M</td>
      <td>0</td>
      <td>00</td>
      <td>전국</td>
      <td>계</td>
      <td>Whole country</td>
      <td>Total</td>
    </tr>
    <tr>
      <th>3</th>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>202209</td>
      <td>DT_1B04005N</td>
      <td>총인구수</td>
      <td>Population</td>
      <td>T2</td>
      <td>명</td>
      <td>101</td>
      <td>Person</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>9450768</td>
      <td>M</td>
      <td>0</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>계</td>
      <td>Seoul</td>
      <td>Total</td>
    </tr>
    <tr>
      <th>4</th>
      <td>행정구역(읍면동)별/5세별 주민등록인구(2011년~)</td>
      <td>202209</td>
      <td>DT_1B04005N</td>
      <td>남자인구수</td>
      <td>Male</td>
      <td>T3</td>
      <td>명</td>
      <td>101</td>
      <td>Person</td>
      <td>행정구역(동읍면)별</td>
      <td>By Administrative District</td>
      <td>5세별</td>
      <td>By Age Group (Five-Year)</td>
      <td>4582361</td>
      <td>M</td>
      <td>0</td>
      <td>11</td>
      <td>서울특별시</td>
      <td>계</td>
      <td>Seoul</td>
      <td>Total</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 피벗 테이블 만들기

통계표 조회 결과를 아래와 같이 원하는 형태로 재구조화하여 분석 목적에 맞게 사용하면 된다.


```python
pv = df.pivot(index=["C1","C1_NM","PRD_DE"], columns=["ITM_NM"], values="DT").reset_index()
pv.columns.name = None
pv['PRD_DE'] = pd.to_datetime(pv['PRD_DE'], format="%Y%m")
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
      <th>C1</th>
      <th>C1_NM</th>
      <th>PRD_DE</th>
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
      <td>2022-09-01</td>
      <td>25653998</td>
      <td>25812660</td>
      <td>51466658</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>2022-09-01</td>
      <td>4582361</td>
      <td>4868407</td>
      <td>9450768</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11110</td>
      <td>종로구</td>
      <td>2022-09-01</td>
      <td>68652</td>
      <td>73326</td>
      <td>141978</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1111051500</td>
      <td>청운효자동</td>
      <td>2022-09-01</td>
      <td>5356</td>
      <td>6337</td>
      <td>11693</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1111053000</td>
      <td>사직동</td>
      <td>2022-09-01</td>
      <td>4037</td>
      <td>5089</td>
      <td>9126</td>
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
      <td>2022-09-01</td>
      <td>5569</td>
      <td>5683</td>
      <td>11252</td>
    </tr>
    <tr>
      <th>3854</th>
      <td>5013059000</td>
      <td>대륜동</td>
      <td>2022-09-01</td>
      <td>7854</td>
      <td>7695</td>
      <td>15549</td>
    </tr>
    <tr>
      <th>3855</th>
      <td>5013060000</td>
      <td>대천동</td>
      <td>2022-09-01</td>
      <td>7018</td>
      <td>6826</td>
      <td>13844</td>
    </tr>
    <tr>
      <th>3856</th>
      <td>5013061000</td>
      <td>중문동</td>
      <td>2022-09-01</td>
      <td>6210</td>
      <td>6033</td>
      <td>12243</td>
    </tr>
    <tr>
      <th>3857</th>
      <td>5013062000</td>
      <td>예래동</td>
      <td>2022-09-01</td>
      <td>1992</td>
      <td>1916</td>
      <td>3908</td>
    </tr>
  </tbody>
</table>
<p>3858 rows × 6 columns</p>
</div>

<br>

## 참고

- [Python으로 KOSIS 데이터 조회하기](https://wooiljeong.github.io/python/pdr-kosis/)
- [PublicDataReader GitHub 저장소](https://github.com/WooilJeong/PublicDataReader)
- [KOSIS 국가통계포털 Open API 가이드](https://kosis.kr/serviceInfo/openAPIGuide.do)
- [KOSIS 공유서비스](https://kosis.kr/openapi/index/index.jsp)
