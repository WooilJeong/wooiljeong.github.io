---
title: "Python으로 좌표계 변환하기"
categories: Python
tags: 
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

> # Python으로 좌표계 변환

## TM좌표계에서 WGS84좌표계로 변환하기

좌표계 변환 기능 구현을 위한 예시 데이터는 [지방행정 인허가 데이터개방](https://www.localdata.go.kr/main.do)의 일반음식점 관련 인허가 정보를 사용했다. 이 사이트에서는 `중부원점TM(EPSG:2097) 좌표계`를 기준으로 데이터를 제공하고 있다. 상황에 따라 비교적 익숙한 `경도 위도 좌표계` 즉, `WGS84(EPSG:4326) 좌표계` 기준 데이터가 필요할 때가 있다. 예시에서 사용된 좌표계 외에도 다양한 좌표계들이 존재한다. 관련 정보는 [한국 주요 좌표계 EPSG코드 및 proj4 인자 정리](https://www.osgeo.kr/17)를 참고하면 된다.

우선, 다음과 같이 `지방행정 인허가 데이터개방` 사이트에서 다운로드 받은 일반음식점 관련 데이터를 불러온다.


```python
import pandas as pd
import numpy as np
from pyproj import Proj, transform
import folium

# DataFrame 디스플레이 설정
pd.set_option('display.max_columns', 250)
pd.set_option('display.max_rows', 250)
pd.set_option('display.width', 100)

# Pandas Float 자릿수 표시 제한
pd.options.display.float_format = '{:.2f}'.format

# Sample Dataset
useCols = [
    "번호",
    "개방서비스명",
    "인허가일자",
    "영업상태구분코드",
    "영업상태명",
    "상세영업상태코드",
    "상세영업상태명",
    "폐업일자",
    "소재지전체주소",
    "도로명전체주소",
    "사업장명",
    "업태구분명",
    "좌표정보(x)",
    "좌표정보(y)",
]

df = pd.read_csv("./sample/sample_license.csv",
                 engine="python",
                 sep=",",
                 quotechar='"',
                 error_bad_lines=False,
                 usecols=useCols)
```


불러온 데이터의 사이즈가 꽤 크기 때문에 서울 지역으로 범위를 좁히자. 서울 지역 내에서도 여전히 데이터 사이즈가 크므로, 100개의 음식점 정보를 랜덤 추출한 뒤 좌표계 변환을 시도해보자.


```python
# 서울
df = df.loc[df['소재지전체주소'].str.startswith("서울", na=False)]
# 좌표 결측 제거
df = df.dropna(subset=['좌표정보(x)','좌표정보(y)'])
# 랜덤 샘플링 n: 100
df = df.sample(n=100, random_state=1)
# 컬럼명 재지정
df = df.rename(columns={"좌표정보(x)": "x", "좌표정보(y)": 'y'})
# 인덱스 재지정
df.index = range(len(df))
```

선택된 데이터는 다음과 같다. 우측 2개 컬럼이 바로 위치 좌표이다. 이 좌표값은 `TM(EPSG:2097) 좌표계`를 따르고 있는데, 좀 더 익숙한 `경도 위도 좌표계` 즉, `WGS84(EPSG:4326) 좌표계`로 변환해보자.


```python
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
      <th>번호</th>
      <th>개방서비스명</th>
      <th>인허가일자</th>
      <th>영업상태구분코드</th>
      <th>영업상태명</th>
      <th>상세영업상태코드</th>
      <th>상세영업상태명</th>
      <th>폐업일자</th>
      <th>소재지전체주소</th>
      <th>도로명전체주소</th>
      <th>사업장명</th>
      <th>업태구분명</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>95</th>
      <td>1443988</td>
      <td>일반음식점</td>
      <td>19951130</td>
      <td>3</td>
      <td>폐업</td>
      <td>2</td>
      <td>폐업</td>
      <td>19970904</td>
      <td>서울특별시 관악구 신림동 1432-70번지</td>
      <td>NaN</td>
      <td>비엔날레</td>
      <td>분식</td>
      <td>193662.65</td>
      <td>442668.04</td>
    </tr>
    <tr>
      <th>96</th>
      <td>960229</td>
      <td>일반음식점</td>
      <td>20041105</td>
      <td>3</td>
      <td>폐업</td>
      <td>2</td>
      <td>폐업</td>
      <td>20141001</td>
      <td>서울특별시 용산구 한강로3가 40-999번지 용산민자역사 421호</td>
      <td>서울특별시 용산구 한강대로23길 55, 421호 (한강로3가, 용산민자역사)</td>
      <td>이화교자칼국수</td>
      <td>한식</td>
      <td>196762.08</td>
      <td>447480.04</td>
    </tr>
    <tr>
      <th>97</th>
      <td>200566</td>
      <td>일반음식점</td>
      <td>20190809</td>
      <td>1</td>
      <td>영업/정상</td>
      <td>1</td>
      <td>영업</td>
      <td>NaN</td>
      <td>서울특별시 광진구 중곡동 341-7번지</td>
      <td>서울특별시 광진구 천호대로 573, 2층 (중곡동)</td>
      <td>일단포차</td>
      <td>정종/대포집/소주방</td>
      <td>207161.46</td>
      <td>450484.39</td>
    </tr>
    <tr>
      <th>98</th>
      <td>579687</td>
      <td>일반음식점</td>
      <td>20140710</td>
      <td>1</td>
      <td>영업/정상</td>
      <td>1</td>
      <td>영업</td>
      <td>NaN</td>
      <td>서울특별시 마포구 연남동 567-39번지 1층</td>
      <td>서울특별시 마포구 월드컵북로6길 93, 1층 (연남동)</td>
      <td>오이도(52nd Street)</td>
      <td>호프/통닭</td>
      <td>193110.64</td>
      <td>450912.06</td>
    </tr>
    <tr>
      <th>99</th>
      <td>183143</td>
      <td>일반음식점</td>
      <td>20071022</td>
      <td>1</td>
      <td>영업/정상</td>
      <td>1</td>
      <td>영업</td>
      <td>NaN</td>
      <td>서울특별시 영등포구 영등포동3가 5-10번지 외57필지 54호 3층 312호</td>
      <td>서울특별시 영등포구 영등포로 228 (영등포동3가,외57필지 54호 3층 312호)</td>
      <td>맛나고신나고</td>
      <td>호프/통닭</td>
      <td>191661.04</td>
      <td>446332.01</td>
    </tr>
  </tbody>
</table>
</div>



## EPSG:2097에서 EPSG:4326로 변환하기

다음과 같이 두 가지 프로젝션 인스턴스를 정의한다.


```python
# Projection 정의
# 중부원점(Bessel): 서울 등 중부지역 EPSG:2097
proj_1 = Proj(init='epsg:2097')

# WGS84 경위도: GPS가 사용하는 좌표계 EPSG:4326
proj_2 = Proj(init='epsg:4326')
```


아래 코드를 실행하면 데이터 프레임 우측 끝에 경도, 위도 컬럼이 생성되는 것을 확인할 수 있다.


```python
# 데이터 프레임 지정
DataFrame = df.copy()

x_list = []
y_list = []

for idx, row in DataFrame.iterrows():
    x, y = row['x'], row['y']
    x_, y_ = transform(proj_1, proj_2, x, y)
    x_list.append(x_)
    y_list.append(y_)
    
df['lon'] = x_list
df['lat'] = y_list
```



```python
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
      <th>번호</th>
      <th>개방서비스명</th>
      <th>인허가일자</th>
      <th>영업상태구분코드</th>
      <th>영업상태명</th>
      <th>상세영업상태코드</th>
      <th>상세영업상태명</th>
      <th>폐업일자</th>
      <th>소재지전체주소</th>
      <th>도로명전체주소</th>
      <th>사업장명</th>
      <th>업태구분명</th>
      <th>x</th>
      <th>y</th>
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>95</th>
      <td>1443988</td>
      <td>일반음식점</td>
      <td>19951130</td>
      <td>3</td>
      <td>폐업</td>
      <td>2</td>
      <td>폐업</td>
      <td>19970904</td>
      <td>서울특별시 관악구 신림동 1432-70번지</td>
      <td>NaN</td>
      <td>비엔날레</td>
      <td>분식</td>
      <td>193662.65</td>
      <td>442668.04</td>
      <td>126.93</td>
      <td>37.49</td>
    </tr>
    <tr>
      <th>96</th>
      <td>960229</td>
      <td>일반음식점</td>
      <td>20041105</td>
      <td>3</td>
      <td>폐업</td>
      <td>2</td>
      <td>폐업</td>
      <td>20141001</td>
      <td>서울특별시 용산구 한강로3가 40-999번지 용산민자역사 421호</td>
      <td>서울특별시 용산구 한강대로23길 55, 421호 (한강로3가, 용산민자역사)</td>
      <td>이화교자칼국수</td>
      <td>한식</td>
      <td>196762.08</td>
      <td>447480.04</td>
      <td>126.96</td>
      <td>37.53</td>
    </tr>
    <tr>
      <th>97</th>
      <td>200566</td>
      <td>일반음식점</td>
      <td>20190809</td>
      <td>1</td>
      <td>영업/정상</td>
      <td>1</td>
      <td>영업</td>
      <td>NaN</td>
      <td>서울특별시 광진구 중곡동 341-7번지</td>
      <td>서울특별시 광진구 천호대로 573, 2층 (중곡동)</td>
      <td>일단포차</td>
      <td>정종/대포집/소주방</td>
      <td>207161.46</td>
      <td>450484.39</td>
      <td>127.08</td>
      <td>37.56</td>
    </tr>
    <tr>
      <th>98</th>
      <td>579687</td>
      <td>일반음식점</td>
      <td>20140710</td>
      <td>1</td>
      <td>영업/정상</td>
      <td>1</td>
      <td>영업</td>
      <td>NaN</td>
      <td>서울특별시 마포구 연남동 567-39번지 1층</td>
      <td>서울특별시 마포구 월드컵북로6길 93, 1층 (연남동)</td>
      <td>오이도(52nd Street)</td>
      <td>호프/통닭</td>
      <td>193110.64</td>
      <td>450912.06</td>
      <td>126.92</td>
      <td>37.56</td>
    </tr>
    <tr>
      <th>99</th>
      <td>183143</td>
      <td>일반음식점</td>
      <td>20071022</td>
      <td>1</td>
      <td>영업/정상</td>
      <td>1</td>
      <td>영업</td>
      <td>NaN</td>
      <td>서울특별시 영등포구 영등포동3가 5-10번지 외57필지 54호 3층 312호</td>
      <td>서울특별시 영등포구 영등포로 228 (영등포동3가,외57필지 54호 3층 312호)</td>
      <td>맛나고신나고</td>
      <td>호프/통닭</td>
      <td>191661.04</td>
      <td>446332.01</td>
      <td>126.90</td>
      <td>37.52</td>
    </tr>
  </tbody>
</table>
</div>



서울 지도 위에 위에서 변환한 경도, 위도 값을 바탕으로 시각화하면 아래와 같다.


```python
lat_c = 37.53165351203043
lon_c = 126.9974246490573

m = folium.Map(location=[lat_c, lon_c], zoom_start=11)

for idx, row in df.iterrows():

    lat_ = row['lat']
    lon_ = row['lon']

    folium.Marker(location=[lat_, lon_],
                  radius=15
                  ).add_to(m)
```


```python
m
```

<iframe
  src="/assets/html/folium/210216.html"
  style="width:100%; height:500px;"
></iframe>


```python
m.save("./210216.html")
```
