---
title: "PublicDataReader - 건축물대장 데이터 조회하기"
categories: Python
tags: PublicDataReader
header:
  overlay_image: /assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 국토교통부 건축물대장정보 데이터 조회하기

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

## 국토교통부 건축물대장정보 서비스

- [국토교통부 건축물대장정보 서비스](https://www.data.go.kr/data/15044713/openapi.do)

| **서비스명**                 | **카테고리명** |
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

다음과 같이 발급받은 `serviceKey` 값을 이용해 부동산 실거래가 데이터를 조회할 `bd` 세션을 만들어줍니다. `debug`의 값을 `True`로 입력하면 아래와 같은 메시지를 확인할 수 있습니다. 메시지 출력을 원치 않는 경우 `False`를 입력하면 됩니다. 본 라이브러리를 정상적으로 이용하기 위해서는 국토교통부 실거래가 정보 조회 서비스에 대한 OpenAPI 활용신청을 반드시 완료해야합니다.


```python
# 3. 국토교통부 건축물대장정보 서비스 OpenAPI 세션 정의하기
# debug: True이면 모든 메시지 출력, False이면 오류 메시지만 출력 (기본값: False)
bd = pdr.Building(serviceKey, debug=True)
```

    [INFO] 기본개요 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 총괄표제부 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 표제부 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 층별개요 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 부속지번 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 전유공용면적 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 오수정화시설 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 주택가격 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 전유부 조회 서비스 정상 - (00) NORMAL SERVICE.
    [INFO] 지역지구구역 조회 서비스 정상 - (00) NORMAL SERVICE.

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


## 건축물대장 조회하기

`category`에 건축물대장 종류를 입력하고, `sigunguCd`, `bjdongCd` 각각에 시군구코드와 읍면동코드를 입력합니다. `bun`과 `ji`에는 조회할 건축물의 본번과 부번을 입력합니다. 이후 위에서 정의한 데이터 조회 세션인 `bd`의 `read_data` 메서드를 호출하여 건축물대장을 `DataFrame` 형태로 조회합니다.

```python
# 5. 건축물대장정보 오퍼레이션별 데이터 조회
category = "기본개요"                                   # 건축물대장 종류 (ex. 표제부, 총괄표제부, 전유부 등)
sigunguCd = "41135"                                     # 시군구코드(5)
bjdongCd = "11000"                                      # 읍면동코드(5)
bun = "0541"                                            # 본번(4)
ji = "0000"                                             # 부번(4)

df = bd.read_data(category=category, sigunguCd=sigunguCd, bjdongCd=bjdongCd, bun=bun, ji=ji)
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
      <th>법정동코드</th>
      <th>건물명</th>
      <th>블록</th>
      <th>번</th>
      <th>외필지수</th>
      <th>생성일자</th>
      <th>구역코드</th>
      <th>구역코드명</th>
      <th>지</th>
      <th>지구코드</th>
      <th>지구코드명</th>
      <th>지역코드</th>
      <th>지역코드명</th>
      <th>로트</th>
      <th>관리건축물대장PK</th>
      <th>관리상위건축물대장PK</th>
      <th>새주소법정동코드</th>
      <th>새주소본번</th>
      <th>새주소도로코드</th>
      <th>새주소부번</th>
      <th>새주소지상지하코드</th>
      <th>도로명대지위치</th>
      <th>대지구분코드</th>
      <th>대지위치</th>
      <th>대장구분코드</th>
      <th>대장구분코드명</th>
      <th>대장종류코드</th>
      <th>대장종류코드명</th>
      <th>순번</th>
      <th>시군구코드</th>
      <th>특수지명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11000</td>
      <td>현대백화점 판교복합몰</td>
      <td></td>
      <td>0541</td>
      <td>0</td>
      <td>20200924</td>
      <td>UQQ300</td>
      <td>지구단위계획구역</td>
      <td>0000</td>
      <td></td>
      <td></td>
      <td>UQA210</td>
      <td>중심상업지역</td>
      <td></td>
      <td>41135-100259554</td>
      <td></td>
      <td>11001</td>
      <td>20.0</td>
      <td>411354340519</td>
      <td>0.0</td>
      <td>0</td>
      <td>경기도 성남시 분당구 판교역로146번길 20</td>
      <td>0</td>
      <td>경기도 성남시 분당구 백현동 541번지</td>
      <td>1</td>
      <td>일반</td>
      <td>2</td>
      <td>일반건축물</td>
      <td>1</td>
      <td>41135</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




<br>

## PublicDataReader는 오픈소스 프로젝트로 개발하고 있습니다.

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}


`PublicDataReader`는 파이썬 오픈소스 프로젝트로 개발하고 있습니다. 궁금한 점이 있으시면 언제든지 아래 이메일이나 카카오톡 오픈채팅방을 이용해주세요. 감사합니다.

<br>

**E-mail : wooil@kakao.com**  
**[카카오톡 오픈채팅방 링크](https://open.kakao.com/o/gFYXtP2c)**  

<br>

### PublicDataReader 관련 글 목록

- [PublicDataReader - 부동산 실거래가 조회하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [PublicDataReader - 건축물대장 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_03/)
- [PublicDataReader - 상가업소 데이터 조회하기](https://wooiljeong.github.io/python/public_data_reader_02/)

### Github Repository

- [PublicDataReader](https://github.com/WooilJeong/PublicDataReader)  
