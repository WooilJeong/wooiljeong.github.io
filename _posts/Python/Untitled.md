## Open API 서비스 키 입력하기

공공 데이터 포털에서 발급받은 서비스 키를 복사하여 다음과 같이 `serviceKey`에 문자열로 할당해줍니다. 오픈 API 서비스 키 발급에 관해 모르시는 분들은 구글에 '**공공 데이터 포털 Open API 사용법**'을 검색해보시면 여러 문서를 참조하실 수 있습니다. 검색 후 가장 상단에 있는 [이 블로그](https://jeong-pro.tistory.com/143)를 참조하셔도 됩니다.


```python
import PublicDataReader as pdr
print(pdr.__version__)
```

    
    >>> PublicDataReader Version : 0.1.1
    
    - Author : Wooil Jeong
    - E-mail : wooil@kakao.com
    - Github : https://github.com/WooilJeong/PublicDataReader
    - Blog : https://wooiljeong.github.io
    
    

## 서비스 키로 아파트매매 실거래 자료 수집기 만들기

다음과 같이 아파트매매 실거래 자료를 수집할 `AptTrade` 수집기를 만들어줍니다. 위에서 정의한 `serviceKey`를 다음과 같이 입력해줍니다.


```python
serviceKey="OPEN API SERVICE KEY HERE"
```

## 서비스 키로 아파트매매 실거래 자료 수집기 만들기

다음과 같이 아파트매매 실거래 자료를 수집할 `AptTrade` 수집기를 만들어줍니다. 위에서 정의한 `serviceKey`를 다음과 같이 입력해줍니다.


```python
AptTrade = pdr.AptTradeReader(serviceKey)
```

    >>> 서비스가 정상 작동합니다.
    

## 주소 검색을 통해 지역코드 검색하기

아파트매매 실거래 자료 Open API 기술 문서에 따르면 조회하고자 하는 지역의 코드는 본래 법정동 코드 10자리 중 앞 5자리를 사용한다고 정의되어 있습니다. 이 부분도 직접 크롤링 코드를 작성할 때 꽤 번거로운 부분이었는데, 지역명을 문자열로 입력받아 지역코드를 반환하는 함수를 만들었습니다. 즉, 조회하고자 하는 지역의 이름을 검색하고, 출력된 '법정구코드'를 아래 데이터 수집 과정에서 활용하시면 됩니다.


```python
df_code = AptTrade.CodeFinder("분당구")
df_code.head(1)
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



## 특정 월의 아파트매매 실거래 데이터 불러오기

다음과 같이 `AptTrade.DataReader()`에 `지역코드`와 `계약월`을 입력해줍니다. 결과는 `Pandas`의 `DataFrame` 형태로 반환됩니다.


```python
df = AptTrade.DataReader("41135", "202004")
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



## 특정 기간 동안의 아파트매매 실거래 데이터 불러오기

특정 월 뿐 아니라 `AptTrade.DataCollector()`를 사용하면, 특정 기간 동안의 데이터를 한 번에 수집할 수 있습니다. `지역코드`, `시작월`, `종료월` 순으로 입력하면 됩니다. 위의 `AptTrade.DataReader()`와 같이 결과는 `Pandas`의 `DataFrame` 형태로 반환됩니다.



```python
df_sum = AptTrade.DataCollector("41135", "2020-01", "2020-03")
df_sum.head()
```

    >>> LAWD_CD : 41135 DEAL_YMD : 202001
    >>> LAWD_CD : 41135 DEAL_YMD : 202002
    >>> LAWD_CD : 41135 DEAL_YMD : 202003
    




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



## 전체 코드 정리


```python
import PublicDataReader as pdr

# Open API 서비스 키 초기화
serviceKey = "OPEN API SERVICE KEY HERE"
Apt = pdr.AptTradeReader(serviceKey)

# 지역코드 검색기
df_code = AptTrade.CodeFinder("분당구")
df_code.head(1)

# 특정 월 아파트매매 실거래 자료 수집기
df = AptTrade.DataReader("41135", "202003")

# 특정 기간 아파트매매 실거래 자료 수집기
df_sum = AptTrade.DataCollector("41135", "2020-01", "2020-03")
```

    >>> 서비스가 정상 작동합니다.
    >>> Python Logic Error. e-mail : wooil@kakao.com
    >>> LAWD_CD : 41135 DEAL_YMD : 202001
    >>> LAWD_CD : 41135 DEAL_YMD : 202002
    >>> LAWD_CD : 41135 DEAL_YMD : 202003
    

## Colab으로 테스트해보기

아래 링크에 접속하시면 구글 코랩 환경에서 예제 코드를 테스트해볼 수 있습니다. 아파트 매매 실거래자료 조회 뿐만 아니라 다른 서비스들도 테스트해보실 수 있습니다.

[Colab Test Code](https://disq.us/url?url=https%3A%2F%2Fdrive.google.com%2Fopen%3Fid%3D1pFtMFr_te9T_maHjee8Sd8Yq9rTrE-4F%3AUps3RW1D7_xrpDTq3k2p_YQMVXk&cuid=5671700)

<br>

## Open Source Project

![PNG](/assets/img/post_img/2019-12-04-public_data_reader_01/img_logo.png){: .align-center}


`PublicDataReader`는 파이썬 오픈소스 프로젝트로 개발하고 있습니다. 크롤링 코드 작성과 같은 데이터 활용의 번거로움을 줄이고자 시작하게 되었습니다. 실제 활용시 발생하는 오류나 개선 사항 등 피드백을 남겨주시면 적극 반영하도록 하겠습니다. 또한, 이 프로젝트에 참여하고자 하시는 분들은 언제든지 아래 이메일 주소로 연락주시면 감사하겠습니다.

**E-mail : wooil@kakao.com**

<br>

### PublicDataReader 관련 글 목록

- [국토교통부 부동산 데이터 수집하기](https://wooiljeong.github.io/python/public_data_reader_01/)
- [데이터 과학을 위한 파이썬 라이브러리 모음](https://wooiljeong.github.io/python/python_library/)

### PublicDataReader Github Repository

- [PublicDataReader](https://github.com/WooilJeong/PublicDataReader)  






```python

```
