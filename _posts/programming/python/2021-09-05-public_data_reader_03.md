---
title: "PublicDataReader - 건축물대장 데이터 조회하기"
categories: python
tags: PublicDataReader
header:
  overlay_image: /assets/img/logo/PublicDataReader.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 국토교통부 건축물대장정보 데이터 조회하기

<br>

![PNG](https://github.com/WooilJeong/PublicDataReader/blob/main/assets/img/logo.png?raw=true)

파이썬 [PublicDataReader](https://github.com/WooilJeong/PublicDataReader) 라이브러리를 이용하면 **공공데이터포털**에서 제공하는 **국토교통부 건축물대장정보** 데이터를 쉽게 조회할 수 있습니다. 

<br>

## PublicDataReader

PublicDataReader는 공공 데이터를 자동으로 조회할 수 있는 파이썬 라이브러리입니다. 이 라이브러리는 공공데이터포털과 국가통계포털(KOSIS)과 같이 Open API 서비스로 제공하는 공공 데이터를 쉽게 조회할 수 있도록 도와줍니다. 인증키가 필요한 공공 데이터는 인증키를 사용하여 조회할 수 있고, 인증키가 필요하지 않은 데이터는 별도의 인증 절차 없이 조회할 수 있습니다. PublicDataReader를 이용하면 일반적인 공공 데이터 조회 과정에서 필요한 API 명세 찾기, 요청 작성, 반환된 데이터 정리 과정을 자동으로 처리해줍니다. 또한, 웹에 공개된 데이터를 조회할 때도 데이터 수집과 가공 과정을 자동화해줍니다. 이를 통해 코드 작성이 간결해지고 공공 데이터 조회 작업이 편리해집니다.

- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)

<br>

## 국토교통부 건축물대장정보 서비스

PublicDataReader를 통해 공공데이터포털에서 제공하는 Open API 서비스를 정상적으로 이용하려면 아래 서비스 신청 페이지 링크에 접속 후 서비스 이용 신청을 해야 합니다. 서비스 신청을 완료하면 Open API를 사용할 수 있는 서비스 키가 발급됩니다. 서비스 제공 기관에 따라 서비스 키 발급 후 약 1~2일이 지난 후 접근 권한이 부여될 수 있습니다. 충분한 시간이 지난 후에도 접근 권한이 부여되지 않는다면 서비스 제공처에 문의하는 것을 권장합니다.

- [건축물대장정보 서비스 신청 페이지](https://www.data.go.kr/data/15044713/openapi.do)


| **서비스명**                 | **대장 유형** |
| ---------------------------- | -------------- |
| 건축물대장 기본개요 조회     | 기본개요       |
| 건축물대장 총괄표제부 조회   | 총괄표제부     |
| 건축물대장 표제부 조회       | 표제부         |
| 건축물대장 층별개요 조회     | 층별개요       |
| 건축물대장 부속지번 조회     | 부속지번       |
| 건축물대장 전유공용면적 조회 | 전유공용면적   |
| 건축물대장 오수정화시설 조회 | 오수정화시설   |
| 건축물대장 주택가격 조회     | 주택가격       |
| 건축물대장 전유부 조회       | 전유부         |
| 건축물대장 지역지구구역 조회 | 지역지구구역   |

<br>

## 국토교통부 건축소유자정보 서비스

- [건축소유자정보 서비스 신청 페이지](https://www.data.go.kr/data/15021136/openapi.do)

<div align="center">

| **서비스명**                 | **대장 유형** |
| :---------------------------- | :-------------- |
| 건축물소유정보 조회           | 소유자       |

</div>


<br>

## 테이블 관계도

![PNG](/assets/img/common/building_talbe_erd.png){: .align-center}

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

## 국토교통부 건축물대장정보 조회 API 인스턴스 정의하기

PublicDataReader 라이브러리에서 `BuildingLedger`라는 클래스를 가져와 `api` 라는 건축물대장정보 조회 API 인스턴스를 생성합니다. 이 때, `service_key` 값을 인자로 입력합니다.

```python
from PublicDataReader import BuildingLedger
api = BuildingLedger(service_key)
```

<br>

## 데이터 조회 시 필요한 입력 항목

| 이름         | 설명                                                                                                                              | 데이터 타입   | 샘플 데이터   | 항목구분   |
|:-------------|:----------------------------------------------------------------------------------------------------------------------------------|:--------------|:--------------|:-----------|
| ledger_type  | 건축물대장 유형<br>(기본개요, 총괄표제부, 표제부, 층별개요, 부속지번, 전유공용면적, 오수정화시설, 주택가격, 전유부, 지역지구구역, 소유자) | String        | 총괄표제부    | 필수       |
| sigungu_code | 시군구의 5자리 지역코드<br>(서울 서초구: 11650, 경기 성남 분당구: 41135)                                                          | String        | 41135         | 필수       |
| bdong_code   | 읍면동의 5자리 지역코드<br>(서울 서초구 잠원동: 10600, 경기 성남 분당구 백현동: 11000)                                            | String        | 11000         | 필수       |
| bun          | 주소 번지의 본번<br>(350번지: 350)                                                                                                | String        | 540           | 선택       |
| ji           | 주소 번지의 부번<br>(350-20번지: 20)                                                                                              | String        | nan           | 선택       |
| translate    | 컬럼명 한글 표시 여부<br>(한글 표시: True, 영문 표시: False)<br>※ 기본값: True                                                    | Boolean       | True          | 선택       |
| verbose      | 데이터 조회 진행 상황 메시지 출력 여부<br>(출력: True, 미출력: False)<br>※ 기본값: False                                          | Boolean       | False         | 선택       |
| wait_time    | API 추가 요청 시 대기 시간(초)<br>(30초: 30)<br>※ 기본값: 30                                                                      | Integer       | 30            | 선택       |

<br>

## 시군구코드와 법정동코드 조회하기

건축물대장정보를 조회하려면 조회할 건축물이 속해 있는 시군구와 법정동 정보를 알아야 합니다. 예를 들어, 경기도 성남시 분당구 백현동에 있는 건축물의 대장 정보를 조회한다고 가정하면, 분당구 백현동에 해당하는 시군구코드와 법정동코드 정보를 알아야 합니다. 아래 코드는 PublicDataReader 라이브러리를 사용하여 분당구 백현동과 일치하는 행을 찾는 작업을 수행합니다. 

1. 먼저 PublicDataReader 라이브러리를 가져와 pdr라는 별칭으로 사용 할 수 있도록 합니다.
2. 'sigungu_name = "분당구" bdong_name = "백현동" 이 부분에서 검색할 시군구와 읍면동을 변수에 저장합니다.
3. code = pdr.code_bdong() code라는 변수에 pdr의 code_bdong() 함수를 실행하여 그 결과값을 저장합니다. 이 함수는 시군구와 읍면동에 해당하는 코드를 포함하는 데이터프레임을 반환합니다.
4. code.loc[(code['시군구명'].str.contains(sigungu_name, na=False)) & (code['읍면동명']==bdong_name)] 이 부분에서는 시군구명열에서 '분당구'를 포함하는 행과 읍면동명열에서 '백현동'인 행을 검색하는 검색식을 작성합니다.

조회 결과를 살펴보면 분당구에 해당하는 시군구코드는 41135이고, 백현동에 해당하는 법정동코드는 4113511000인 것을 알 수 있습니다. 법정동코드 10자리 중 처음 5자리는 시군구코드와 같고, 이후 5자리는 읍면동코드를 의미합니다. 건축물대장 조회 시 시군구코드와 법정동코드를 사용하는데, 이때 사용되는 법정동코드는 5자리 읍면동코드입니다. 즉, 건축물대장 조회 시 사용할 시군구코드는 41135이고, 법정동코드는 11000이 됩니다.

```python
import PublicDataReader as pdr
sigungu_name = "분당구"
bdong_name = "백현동"
code = pdr.code_bdong()
code.loc[(code['시군구명'].str.contains(sigungu_name, na=False)) &
         (code['읍면동명']==bdong_name)]
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

## 건축물대장 조회하기

'건축물대장정보 서비스' 신청을 완료했다고 가정하고, 건축물대장 총괄표제부를 조회합니다. api.get_data 메서드를 호출하여 데이터를 조회할 수 있습니다. 메서드의 인자들 중 ledger_type은 건축물대장 유형을 의미합니다. 여기에 '총괄표제부'라고 입력합니다. sigungu_code는 시군구코드를 의미합니다. 여기에 '41135'를 입력합니다. bdong_code는 법정동코드를 의미합니다. 여기에 '11000'을 입력합니다. bun과 ji는 선택 변수로 본번과 부번을 의미합니다. bun에 '540'을 입력합니다. 메서드 호출 결과를 df 변수에 할당합니다.

```python
# 건축물대장 총괄표제부 조회하기
df = api.get_data(
    ledger_type="총괄표제부", 
    sigungu_code="41135", 
    bdong_code="11000", 
    bun="540", 
    ji="",
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
      <td>7756.72</td>
      <td>38963.39</td>
      <td>3</td>
      <td>71.94</td>
      <td>11000</td>
      <td>힐스테이트 판교역(7-1BL)</td>
      <td>None</td>
      <td>0540</td>
      <td>0</td>
      <td>20220825</td>
      <td>...</td>
      <td>총괄표제부</td>
      <td>1</td>
      <td>41135</td>
      <td>None</td>
      <td>20181121</td>
      <td>122211.55</td>
      <td>883</td>
      <td>20220822</td>
      <td>599.69</td>
      <td>64655.84</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 65 columns</p>
</div>

<br>

## 전체 코드 정리

전체 코드를 정리하면 다음과 같습니다.

```python
# 서비스키 할당하기
service_key = "공공데이터포털에서 발급받은 서비스 키"

# 데이터 조회 인스턴스 만들기
from PublicDataReader import BuildingLedger
api = BuildingLedger(service_key)

# 시군구코드와 법정동코드 조회하기
import PublicDataReader as pdr
sigungu_name = "분당구"
bdong_name = "백현동"
code = pdr.code_bdong()
code.loc[(code['시군구명'].str.contains(sigungu_name, na=False)) &
         (code['읍면동명']==bdong_name)]

# 건축물대장 총괄표제부 조회하기
df = api.get_data(
    ledger_type="총괄표제부", 
    sigungu_code="41135", 
    bdong_code="11000", 
    bun="540", 
    ji="",
    )
```



<br>

### 참고

- [PublicDataReader 깃허브 저장소](https://github.com/WooilJeong/PublicDataReader)
- [PublicDataReader 사용자 모임](https://open.kakao.com/o/gbt2Pl2d)