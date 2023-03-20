---
title: "PublicDataReader - 토지소유정보 조회하기"
categories: python
tags: PublicDataReader
header:
  overlay_image: /assets/img/logo/PublicDataReader.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 국토교통부 토지소유정보 데이터 조회하기

<br>

![PNG](https://github.com/WooilJeong/PublicDataReader/blob/main/assets/img/logo.png?raw=true)

파이썬 [PublicDataReader](https://github.com/WooilJeong/PublicDataReader) 라이브러리를 이용하면 **공공데이터포털**에서 제공하는 **국토교통부 토지소유정보** 데이터를 쉽게 조회할 수 있습니다. 

<br>

## PublicDataReader

PublicDataReader는 공공 데이터를 자동으로 조회할 수 있는 파이썬 라이브러리입니다. 이 라이브러리로 공공데이터포털과 국가통계포털(KOSIS)과 같이 오픈 API 서비스로 제공하는 공공 데이터를 쉽게 조회할 수 있습니다. 인증키가 필요한 공공 데이터는 인증키를 사용하여 조회할 수 있고, 인증키가 필요하지 않은 데이터는 별도의 인증 절차 없이 조회할 수 있습니다. PublicDataReader를 이용하면 일반적인 공공 데이터 조회 과정에서 필요한 API 명세 찾기, 요청 작성, 반환된 데이터 정리 과정을 자동으로 처리할 수 있고, 웹에 공개된 데이터를 조회할 때도 데이터 수집과 가공 과정을 자동화할 수 있습니다. 이를 통해 코드 작성이 간결해지고 공공 데이터 조회 작업이 편리해집니다.

- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)

<br>

## 국토교통부 토지소유정보 서비스

PublicDataReader를 통해 공공데이터포털에서 제공하는 Open API 서비스를 정상적으로 이용하려면 아래 서비스 신청 페이지 링크에 접속 후 서비스 이용 신청을 해야 합니다. 서비스 신청을 완료하면 Open API를 사용할 수 있는 서비스 키가 발급됩니다. 서비스 제공 기관에 따라 서비스 키 발급 후 약 1~2일이 지난 후 접근 권한이 부여될 수 있습니다. 충분한 시간이 지난 후에도 접근 권한이 부여되지 않는다면 서비스 제공처에 문의하는 것을 권장합니다.

- [토지소유정보 조회 서비스 신청 페이지](https://www.data.go.kr/data/15058047/openapi.do)

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

## 국토교통부 토지소유정보 조회 API 인스턴스 정의하기

PublicDataReader 라이브러리에서 `LandOwnership`라는 클래스를 가져와 `api` 라는 토지소유정보 조회 API 인스턴스를 생성합니다. 이 때, `service_key` 값을 인자로 입력합니다.

```python
from PublicDataReader import LandOwnership
api = LandOwnership(service_key)
```

<br>

## 시군구코드와 법정동코드 조회하기

토지소유정보를 조회하려면 조회할 토지나 임야 필지가 속해 있는 시군구와 법정동 정보를 알아야 합니다. 예를 들어, 경기도 성남시 분당구 백현동에 있는 필지의 대장 정보를 조회한다고 가정하면, 분당구 백현동에 해당하는 시군구코드와 법정동코드 정보를 알아야 합니다. 아래 코드는 PublicDataReader 라이브러리를 사용하여 분당구 백현동과 일치하는 행을 찾는 작업을 수행합니다. 

1. 먼저 PublicDataReader 라이브러리를 가져와 pdr라는 별칭으로 사용 할 수 있도록 합니다.
2. 'sigungu_name = "분당구" bdong_name = "백현동" 이 부분에서 검색할 시군구와 읍면동을 변수에 저장합니다.
3. code = pdr.code_bdong() code라는 변수에 pdr의 code_bdong() 함수를 실행하여 그 결과값을 저장합니다. 이 함수는 시군구와 읍면동에 해당하는 코드를 포함하는 데이터프레임을 반환합니다.
4. code.loc[(code['시군구명'].str.contains(sigungu_name)) & (code['읍면동명']==bdong_name)] 이 부분에서는 시군구명열에서 '분당구'를 포함하는 행과 읍면동명열에서 '백현동'인 행을 검색하는 검색식을 작성합니다.

조회 결과를 살펴보면 분당구에 해당하는 시군구코드는 41135이고, 백현동에 해당하는 법정동코드는 4113511000인 것을 알 수 있습니다. 법정동코드 10자리 중 처음 5자리는 시군구코드와 같고, 이후 5자리는 읍면동코드를 의미합니다. 주택인허가 조회 시 시군구코드와 법정동코드를 사용하는데, 이때 사용되는 법정동코드는 5자리 읍면동코드입니다. 즉, 주택인허가 조회 시 사용할 시군구코드는 41135이고, 법정동코드는 11000이 됩니다.

```python
import PublicDataReader as pdr
sigungu_name = "분당구"
bdong_name = "백현동"
code = pdr.code_bdong()
code.loc[(code['시군구명'].str.contains(sigungu_name)) &
         (code['읍면동명']==bdong_name)]
```


<div>
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
      <th>5162</th>
      <td>41</td>
      <td>경기도</td>
      <td>41135</td>
      <td>성남시 분당구</td>
      <td>4113511000</td>
      <td>백현동</td>
      <td></td>
      <td>19930115</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




<br>

## 토지소유정보 조회 서비스

### 입력 명세


| 항목명         | 설명                                                                                                                              | 데이터 타입   | 샘플 데이터   | 항목구분   |
|:-------------|:----------------------------------------------------------------------------------------------------------------------------------|:--------------|:--------------|:-----------|
| pnu_code    | PNU코드: 각 필지를 서로 구별하기 위하여 필지마다 붙이는 고유한 번호                                                                            | String        | 540           | 필수       |
| translate    | 컬럼명 한글 표시 여부<br>(한글 표시: True, 영문 표시: False)<br>※ 기본값: True                                                    | Boolean       | True          | 선택       |
| verbose      | 데이터 조회 진행 상황 메시지 출력 여부<br>(출력: True, 미출력: False)<br>※ 기본값: False                                          | Boolean       | False         | 선택       |
| wait_time    | API 추가 요청 시 대기 시간(초)<br>(30초: 30)<br>※ 기본값: 30                                                                      | Integer       | 30            | 선택       |


<br>

### 출력 명세


| 항목명(국문)   | 항목명(영문)               | 항목설명                                                  | 샘플데이터               |
| :--------- | :--------------------- | :----------------------------------------------------- | :------------------- |
| 고유번호      | pnu                   | 각 필지를 서로 구별하기 위하여 필지마다 붙이는 고유한 번호                     | 1165010800113320000 |
| 법정동코드     | ldCode                | 토지가 소재한 행정구역코드(법정동코드) 10자리                            | 1165010800          |
| 법정동명      | ldCodeNm              | 토지가 소재한 소재지의 행정구역 명칭(법정동명)                            | 서울특별시 서초구 서초동       |
| 대장구분코드    | regstrSeCode          | 토지가 위치한 토지의 대장 구분 (토지(임야)대장구분)코드                      | 1                   |
| 대장구분명     | regstrSeCodeNm        | 토지가 위치한 토지의 대장 구분 (토지(임야)대장구분)                        | 토지대장                |
| 지번        | mnnmSlno              | 필지에 부여하여 지적공부에 등록한 번호                                 | 1332-11             |
| 집합건물일련번호  | agbldgSn              | 집합건물일련번호                                              | 5091                |
| 건물동명      | buldDongNm            | 건물동명                                                  | 202                 |
| 건물층명      | buldFloorNm           | 건물층명                                                  | 17                  |
| 건물호명      | buldHoNm              | 건물호명                                                  | 1702                |
| 건물실명      | buldRoomNm            | 건물실명                                                  | 5                   |
| 공유인일련번호   | cnrsPsnSn             | 공유인일련번호                                               | 1                   |
| 기준연월      | stdrYm                | 기준연월                                                  | 42583               |
| 지목코드      | lndcgrCode            | 토지의 주된 용도에 따라 토지의 종류를 구분한 지목코드                        | 8                   |
| 지목        | lndcgrCodeNm          | 토지의 주된 용도에 따라 토지의 종류를 구분한 지목코드의 코드정보                  | 대                   |
| 토지면적(㎡)   | ndpclAr               | 지적공부에 등록한 필지의 수평면상 넓이(㎡)                              | 122                 |
| 공시지가(원/㎡) | pblntfPclnd           | 대한민국의 건설교통부가 토지의 가격을 조사, 감정을 해 공시함. 개별토지에한 공시 가격(원/㎡) | 1544000             |
| 소유구분코드    | posesnSeCode          | 국토를 토지 소유권 취득 주체에 따라 구분한 코드                           | 4                   |
| 소유구분      | posesnSeCodeNm        | 국토를 토지 소유권 취득 주체에 따라 구분                               | 시, 도유지              |
| 거주지구분코드   | resdncSeCode          | 거주지구분코드                                               | ZZ                  |
| 거주지구분     | resdncSeCodeNm        | 거주지구분                                                 | 구분없음                |
| 국가기관구분코드  | nationInsttSeCode     | 국가기관구분코드                                              | 2                   |
| 국가기관구분    | nationInsttSeCodeNm   | 국가기관구분                                                | 지자체                 |
| 소유권변동원인코드 | ownshipChgCauseCode   | 소유권변동원인코드                                             | 5                   |
| 소유권변동원인   | ownshipChgCauseCodeNm | 소유권변동원인                                               | 성명(명칭)변경            |
| 소유권변동일자   | ownshipChgDe          | 소유권변동일자                                               | 34080               |
| 공유인수      | cnrsPsnCo             | 공유인수                                                  | 0                   |
| 데이터기준일자   | lastUpdtDt            | 데이터 작성 기준일자                                           | 42597               |


```python
from PublicDataReader import LandOwnership

service_key = "공공 데이터 포털에서 발급받은 서비스 키"
api = LandOwnership(service_key)

df = api.get_data(
    pnu_code="4113511000105420000", 
)
df.head()
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>고유번호</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>대장구분코드</th>
      <th>대장구분명</th>
      <th>지번</th>
      <th>집합건물일련번호</th>
      <th>건물동명</th>
      <th>건물층명</th>
      <th>건물호명</th>
      <th>...</th>
      <th>거주지구분코드</th>
      <th>거주지구분</th>
      <th>국가기관구분코드</th>
      <th>국가기관구분</th>
      <th>소유권변동원인코드</th>
      <th>소유권변동원인</th>
      <th>소유권변동일자</th>
      <th>공유인수</th>
      <th>데이터기준일자</th>
      <th>lndpclAr</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4113511000105420000</td>
      <td>4113511000</td>
      <td>경기도 성남시 분당구 백현동</td>
      <td>1</td>
      <td>토지대장</td>
      <td>542</td>
      <td>0017</td>
      <td>101</td>
      <td>1</td>
      <td>101</td>
      <td>...</td>
      <td>02</td>
      <td>시도내</td>
      <td>ZZ</td>
      <td>구분없음</td>
      <td>03</td>
      <td>소유권이전</td>
      <td>2011-11-02</td>
      <td>1</td>
      <td>2023-01-14</td>
      <td>34.04</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4113511000105420000</td>
      <td>4113511000</td>
      <td>경기도 성남시 분당구 백현동</td>
      <td>1</td>
      <td>토지대장</td>
      <td>542</td>
      <td>0017</td>
      <td>101</td>
      <td>1</td>
      <td>101</td>
      <td>...</td>
      <td>03</td>
      <td>읍면동내</td>
      <td>ZZ</td>
      <td>구분없음</td>
      <td>03</td>
      <td>소유권이전</td>
      <td>2011-11-02</td>
      <td>1</td>
      <td>2023-01-14</td>
      <td>34.04</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4113511000105420000</td>
      <td>4113511000</td>
      <td>경기도 성남시 분당구 백현동</td>
      <td>1</td>
      <td>토지대장</td>
      <td>542</td>
      <td>0017</td>
      <td>101</td>
      <td>1</td>
      <td>103</td>
      <td>...</td>
      <td>46</td>
      <td>관외(전남)</td>
      <td>ZZ</td>
      <td>구분없음</td>
      <td>03</td>
      <td>소유권이전</td>
      <td>2011-11-17</td>
      <td>1</td>
      <td>2023-01-14</td>
      <td>34.04</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4113511000105420000</td>
      <td>4113511000</td>
      <td>경기도 성남시 분당구 백현동</td>
      <td>1</td>
      <td>토지대장</td>
      <td>542</td>
      <td>0017</td>
      <td>101</td>
      <td>1</td>
      <td>103</td>
      <td>...</td>
      <td>46</td>
      <td>관외(전남)</td>
      <td>ZZ</td>
      <td>구분없음</td>
      <td>03</td>
      <td>소유권이전</td>
      <td>2011-11-17</td>
      <td>1</td>
      <td>2023-01-14</td>
      <td>34.04</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4113511000105420000</td>
      <td>4113511000</td>
      <td>경기도 성남시 분당구 백현동</td>
      <td>1</td>
      <td>토지대장</td>
      <td>542</td>
      <td>0017</td>
      <td>101</td>
      <td>10</td>
      <td>1001</td>
      <td>...</td>
      <td>03</td>
      <td>읍면동내</td>
      <td>ZZ</td>
      <td>구분없음</td>
      <td>03</td>
      <td>소유권이전</td>
      <td>2011-10-27</td>
      <td>1</td>
      <td>2023-01-14</td>
      <td>34.04</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 29 columns</p>
</div>


<br>

## (참고) 건축물대장에서 PNU코드 목록 조회

- 법정동코드 목록 확인

```python
import PublicDataReader as pdr
code_bdong = pdr.code_bdong()
code_bdong.loc[
    (code_bdong['시도명']=='서울특별시') &
    (code_bdong['말소일자']=='') &
    (code_bdong['시군구명']=='서초구')
]
```


<div>

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
      <th>975</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165000000</td>
      <td></td>
      <td></td>
      <td>19880423</td>
      <td></td>
    </tr>
    <tr>
      <th>977</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165010100</td>
      <td>방배동</td>
      <td></td>
      <td>19890427</td>
      <td></td>
    </tr>
    <tr>
      <th>978</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165010200</td>
      <td>양재동</td>
      <td></td>
      <td>19920701</td>
      <td></td>
    </tr>
    <tr>
      <th>979</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165010300</td>
      <td>우면동</td>
      <td></td>
      <td>19920701</td>
      <td></td>
    </tr>
    <tr>
      <th>980</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165010400</td>
      <td>원지동</td>
      <td></td>
      <td>19920701</td>
      <td></td>
    </tr>
    <tr>
      <th>982</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165010600</td>
      <td>잠원동</td>
      <td></td>
      <td>19880423</td>
      <td></td>
    </tr>
    <tr>
      <th>983</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165010700</td>
      <td>반포동</td>
      <td></td>
      <td>19880423</td>
      <td></td>
    </tr>
    <tr>
      <th>984</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165010800</td>
      <td>서초동</td>
      <td></td>
      <td>19880423</td>
      <td></td>
    </tr>
    <tr>
      <th>985</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165010900</td>
      <td>내곡동</td>
      <td></td>
      <td>19880423</td>
      <td></td>
    </tr>
    <tr>
      <th>986</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165011000</td>
      <td>염곡동</td>
      <td></td>
      <td>19880423</td>
      <td></td>
    </tr>
    <tr>
      <th>987</th>
      <td>11</td>
      <td>서울특별시</td>
      <td>11650</td>
      <td>서초구</td>
      <td>1165011100</td>
      <td>신원동</td>
      <td></td>
      <td>19880423</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



<br>

- 건축물대장 조회 후 PNU코드 생성

```python
from PublicDataReader import BuildingLedger
bl = BuildingLedger(service_key)
buildings = bl.get_data("총괄표제부", sigungu_code="11650", bdong_code="10300")
transform_dict = {
    "0": "1",
    "1": "2",
    "2": "3",
}
buildings['필지구분코드'] = buildings['대지구분코드'].replace(transform_dict)
buildings['PNU'] = buildings['시군구코드'] + buildings['법정동코드'] + buildings['필지구분코드'] + buildings['번'].str.zfill(4) + buildings['지'].str.zfill(4)
buildings['필지구분코드'].value_counts()
```

    1    27
    2     1
    Name: 필지구분코드, dtype: int64

<br>

- 필지구분코드로 PNU코드 list_iterator 객체 생성

```python
it = iter(list(buildings.loc[buildings['필지구분코드']=='2']['PNU']))
```

<br>

- 토지소유정보 조회

```python
pnu_code = next(it)
print(pnu_code)
df = api.get_data("1165010300200360000")
df.head()
```


    1165010300200360000
    

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>고유번호</th>
      <th>법정동코드</th>
      <th>법정동명</th>
      <th>대장구분코드</th>
      <th>대장구분명</th>
      <th>지번</th>
      <th>집합건물일련번호</th>
      <th>건물동명</th>
      <th>건물층명</th>
      <th>건물호명</th>
      <th>...</th>
      <th>거주지구분코드</th>
      <th>거주지구분</th>
      <th>국가기관구분코드</th>
      <th>국가기관구분</th>
      <th>소유권변동원인코드</th>
      <th>소유권변동원인</th>
      <th>소유권변동일자</th>
      <th>공유인수</th>
      <th>데이터기준일자</th>
      <th>lndpclAr</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1165010300200360000</td>
      <td>1165010300</td>
      <td>서울특별시 서초구 우면동</td>
      <td>2</td>
      <td>임야대장</td>
      <td>36</td>
      <td>0000</td>
      <td>0000</td>
      <td>0000</td>
      <td>0000</td>
      <td>...</td>
      <td>ZZ</td>
      <td>구분없음</td>
      <td>01</td>
      <td>중앙부처</td>
      <td>03</td>
      <td>소유권이전</td>
      <td>1964-04-01</td>
      <td>0</td>
      <td>2023-01-14</td>
      <td>14777</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 29 columns</p>
</div>


<br>

## 참고

- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)