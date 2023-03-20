---
title: "PublicDataReader - 토지대장 및 임야대장 조회하기"
categories: python
tags: PublicDataReader
header:
  overlay_image: /assets/img/logo/PublicDataReader.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 국토교통부 토지임야정보 데이터 조회하기

<br>

![PNG](https://github.com/WooilJeong/PublicDataReader/blob/main/assets/img/logo.png?raw=true)

파이썬 [PublicDataReader](https://github.com/WooilJeong/PublicDataReader) 라이브러리를 이용하면 **공공데이터포털**에서 제공하는 **국토교통부 토지임야정보** 데이터를 쉽게 조회할 수 있습니다. 

<br>

## PublicDataReader

PublicDataReader는 공공 데이터를 자동으로 조회할 수 있는 파이썬 라이브러리입니다. 이 라이브러리로 공공데이터포털과 국가통계포털(KOSIS)과 같이 오픈 API 서비스로 제공하는 공공 데이터를 쉽게 조회할 수 있습니다. 인증키가 필요한 공공 데이터는 인증키를 사용하여 조회할 수 있고, 인증키가 필요하지 않은 데이터는 별도의 인증 절차 없이 조회할 수 있습니다. PublicDataReader를 이용하면 일반적인 공공 데이터 조회 과정에서 필요한 API 명세 찾기, 요청 작성, 반환된 데이터 정리 과정을 자동으로 처리할 수 있고, 웹에 공개된 데이터를 조회할 때도 데이터 수집과 가공 과정을 자동화할 수 있습니다. 이를 통해 코드 작성이 간결해지고 공공 데이터 조회 작업이 편리해집니다.

- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)

<br>

## 국토교통부 토지임야정보 서비스

PublicDataReader를 통해 공공데이터포털에서 제공하는 Open API 서비스를 정상적으로 이용하려면 아래 서비스 신청 페이지 링크에 접속 후 서비스 이용 신청을 해야 합니다. 서비스 신청을 완료하면 Open API를 사용할 수 있는 서비스 키가 발급됩니다. 서비스 제공 기관에 따라 서비스 키 발급 후 약 1~2일이 지난 후 접근 권한이 부여될 수 있습니다. 충분한 시간이 지난 후에도 접근 권한이 부여되지 않는다면 서비스 제공처에 문의하는 것을 권장합니다.

- [토지임야정보 조회 서비스 신청 페이지](https://www.data.go.kr/data/15057917/openapi.do)

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

## 국토교통부 토지임야정보 조회 API 인스턴스 정의하기

PublicDataReader 라이브러리에서 `LandForestLedger`라는 클래스를 가져와 `api` 라는 토지임야정보 조회 API 인스턴스를 생성합니다. 이 때, `service_key` 값을 인자로 입력합니다.

```python
from PublicDataReader import LandForestLedger
api = LandForestLedger(service_key)
```

<br>

## 시군구코드와 법정동코드 조회하기

토지임야정보를 조회하려면 조회할 토지나 임야 필지가 속해 있는 시군구와 법정동 정보를 알아야 합니다. 예를 들어, 경기도 성남시 분당구 백현동에 있는 필지의 대장 정보를 조회한다고 가정하면, 분당구 백현동에 해당하는 시군구코드와 법정동코드 정보를 알아야 합니다. 아래 코드는 PublicDataReader 라이브러리를 사용하여 분당구 백현동과 일치하는 행을 찾는 작업을 수행합니다. 

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

## 토지임야목록 조회 서비스

### 입력 명세



| 항목명         | 설명                                                                                                                              | 데이터 타입   | 샘플 데이터   | 항목구분   |
|:-------------|:----------------------------------------------------------------------------------------------------------------------------------|:--------------|:--------------|:-----------|
| pnu_code    | PNU코드: 각 필지를 서로 구별하기 위하여 필지마다 붙이는 고유한 번호                                                                            | String        | 540           | 선택       |
| translate    | 컬럼명 한글 표시 여부<br>(한글 표시: True, 영문 표시: False)<br>※ 기본값: True                                                    | Boolean       | True          | 선택       |
| verbose      | 데이터 조회 진행 상황 메시지 출력 여부<br>(출력: True, 미출력: False)<br>※ 기본값: False                                          | Boolean       | False         | 선택       |
| wait_time    | API 추가 요청 시 대기 시간(초)<br>(30초: 30)<br>※ 기본값: 30                                                                      | Integer       | 30            | 선택       |


<br>

### 출력 명세



| 항목명(국문)     | 항목명(영문)        | 항목설명                              | 샘플데이터         |
| ----------- | -------------- | --------------------------------- | ------------- |
| 고유번호        | pnu            | 각 필지를 서로 구별하기 위하여 필지마다 붙이는 고유한 번호 | 1.11101E+18   |
| 법정동명        | ldCodeNm       | 토지가 소재한 소재지의 행정구역코드(법정동코드) 10자리   | 서울특별시 종로구 청운동 |
| 법정동코드       | ldCode         | 토지가 소재한 소재지의 행정구역 명칭(법정동명)        | 1111010100    |
| 지번          | mnnmSlno       | 필지에 부여하여 지적공부에 등록한 번호             | 126-25        |
| 대장구분코드      | regstrSeCode   | 토지가 위치한 토지의 대장 구분 (토지(임야)대장구분)코드  | 1             |
| 대장구분명       | regstrSeCodeNm | 코드 정보                             | 토지대장          |
| 지목코드        | lndcgrCode     | 토지의 주된 용도에 따라 토지의 종류를 구분한 지목코드    | 8             |
| 지목명         | lndcgrCodeNm   | 코드 정보                             | 대             |
| 면적(㎡)       | lndpclAr       | 지적공부에 등록한 필지의 수평면상 넓이(㎡)          | 376.9         |
| 소유구분코드      | posesnSeCode   | 국토를 토지 소유권 취득 주체에 따라 구분한 코드       | 6             |
| 소유구분명       | posesnSeCodeNm | 코드 정보                             | 법인            |
| 소유(공유)인수(명) | cnrsPsnCo      | 토지를 공동 소유하고있는 사람수(명)              | 2             |
| 축척구분코드      | ladFrtlSc      | 토지(임야)대장에 등록된 지적도(임야도)의 축척구분 코드   | 6             |
| 축척구분명       | ladFrtlScNm    | 코드 정보                             | 0.458333333   |
| 데이터기준일자     | lastUpdtDt     | 데이터 작성 기준일자                       | 2015-11-12    |


```python
from PublicDataReader import LandForestLedger

service_key = "공공 데이터 포털에서 발급받은 서비스 키"
api = LandForestLedger(service_key)

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
      <th>법정동명</th>
      <th>법정동코드</th>
      <th>지번</th>
      <th>대장구분코드</th>
      <th>대장구분명</th>
      <th>지목코드</th>
      <th>지목명</th>
      <th>면적(㎡)</th>
      <th>소유구분코드</th>
      <th>소유구분명</th>
      <th>소유(공유)인수(명)</th>
      <th>축척구분코드</th>
      <th>축척구분명</th>
      <th>데이터기준일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4113511000105420000</td>
      <td>경기도 성남시 분당구 백현동</td>
      <td>4113511000</td>
      <td>542-0</td>
      <td>1</td>
      <td>토지대장</td>
      <td>08</td>
      <td>대</td>
      <td>65893.5</td>
      <td>01</td>
      <td>개인</td>
      <td>0</td>
      <td>00</td>
      <td>수치</td>
      <td>2022-06-07</td>
    </tr>
  </tbody>
</table>
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

- 토지임야정보 조회

```python
pnu_code = next(it)
print(pnu_code)
api.get_data(pnu_code=pnu_code)
```


    1165010300200360000
    




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>고유번호</th>
      <th>법정동명</th>
      <th>법정동코드</th>
      <th>지번</th>
      <th>대장구분코드</th>
      <th>대장구분명</th>
      <th>지목코드</th>
      <th>지목명</th>
      <th>면적(㎡)</th>
      <th>소유구분코드</th>
      <th>소유구분명</th>
      <th>소유(공유)인수(명)</th>
      <th>축척구분코드</th>
      <th>축척구분명</th>
      <th>데이터기준일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1165010300200360000</td>
      <td>서울특별시 서초구 우면동</td>
      <td>1165010300</td>
      <td>36-0</td>
      <td>2</td>
      <td>임야대장</td>
      <td>05</td>
      <td>임야</td>
      <td>14777</td>
      <td>02</td>
      <td>국유지</td>
      <td>0</td>
      <td>30</td>
      <td>1:3000</td>
      <td>2022-06-07</td>
    </tr>
  </tbody>
</table>
</div>


<br>

## 참고

- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)