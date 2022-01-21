---
title: "Python으로 좌표계 변환하기"
categories: Python
tags: pyproj
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

> # Python으로 좌표계 변환

## TM좌표계에서 WGS84좌표계로 변환하기

좌표계 변환 기능 구현을 위한 예시 데이터는 [지방행정 인허가 데이터개방](https://www.localdata.go.kr/main.do)의 일반음식점 관련 인허가 정보를 사용했다. 이 사이트에서는 `중부원점TM(EPSG:2097) 좌표계`를 기준으로 데이터를 제공하고 있다. 상황에 따라 비교적 익숙한 `경도 위도 좌표계` 즉, `WGS84(EPSG:4326) 좌표계` 기준 데이터가 필요할 때가 있다. 예시에서 사용된 좌표계 외에도 다양한 좌표계들이 존재한다. 관련 정보는 [한국 주요 좌표계 EPSG코드 및 proj4 인자 정리](https://www.osgeo.kr/17)를 참고하면 된다.

<br>

## 필요 라이브러리

좌표계 변환 작업에 앞서 다음 파이썬 라이브러리를 불러온다.

- pandas
- numpy
- pyproj
- folium


```python
import pandas as pd
import numpy as np
import pyproj
import folium
```

<br>

## 샘플 데이터

다운받은 일반음식점 관련 인허가 정보 데이터를 Pandas DataFrame 객체로 불러온다. 실제 데이터에는 여러 컬럼이 존재하지만, 좌표정보만 불러오기로 한다. 


```python
df = pd.read_csv("./sample/fulldata_07_24_04_P_일반음식점.csv",
                 encoding='cp949',
                 usecols=['좌표정보(x)','좌표정보(y)'])
```

간혹 각 좌표 컬럼의 값으로 문자열이 들어가 있는 경우가 있다. 숫자형으로 데이터 타입을 변환한 후 이상값들은 제거하기로 한다.


```python
df['좌표정보(x)'] = pd.to_numeric(df['좌표정보(x)'], errors="coerce")
df['좌표정보(y)'] = pd.to_numeric(df['좌표정보(y)'], errors="coerce")

df = df.dropna()
df.index=range(len(df))
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
      <th>좌표정보(x)</th>
      <th>좌표정보(y)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1870592</th>
      <td>192085.702260</td>
      <td>182111.703093</td>
    </tr>
    <tr>
      <th>1870593</th>
      <td>190363.418899</td>
      <td>181905.028328</td>
    </tr>
    <tr>
      <th>1870594</th>
      <td>188322.201767</td>
      <td>183130.570776</td>
    </tr>
    <tr>
      <th>1870595</th>
      <td>188873.020499</td>
      <td>184152.828430</td>
    </tr>
    <tr>
      <th>1870596</th>
      <td>187771.639116</td>
      <td>179969.196777</td>
    </tr>
  </tbody>
</table>
</div>



위 좌표정보는 `TM(EPSG:2097) 좌표계`를 따르고 있는데, 좀 더 내게 익숙한 `경도 위도 좌표계` 즉, `WGS84(EPSG:4326) 좌표계`로 변환하기로 한다.

<br>

## EPSG:2097에서 EPSG:4326로 변환하기

우선 좌표계를 변환해주는 함수를 다음과 같이 정의한다.


```python
def project_array(coord, p1_type, p2_type):
    """
    좌표계 변환 함수
    - coord: x, y 좌표 정보가 담긴 NumPy Array
    - p1_type: 입력 좌표계 정보 ex) epsg:5179
    - p2_type: 출력 좌표계 정보 ex) epsg:4326
    """
    p1 = pyproj.Proj(init=p1_type)
    p2 = pyproj.Proj(init=p2_type)
    fx, fy = pyproj.transform(p1, p2, coord[:, 0], coord[:, 1])
    return np.dstack([fx, fy])[0]
```

좌표계 정보가 담긴 DataFrame 객체를 NumPy Array 객체로 만들고, 이 값을 위에서 정의한 함수에 다음과 같이 적용해본다.


```python
# DataFrame -> NumPy Array 변환
coord = np.array(df)
coord
```




    array([[284128.41408968, 428079.5825463 ],
           [346586.097961  , 261066.095208  ],
           [190593.35027262, 461111.28991305],
           ...,
           [188322.201767  , 183130.570776  ],
           [188873.020499  , 184152.82843   ],
           [187771.639116  , 179969.196777  ]])




```python
# 좌표계 정보 설정
p1_type = "epsg:2097"
p2_type = "epsg:4326"

# project_array() 함수 실행
result = project_array(coord, p1_type, p2_type)
result
```




    array([[127.94740197,  37.35095899],
           [128.62031642,  35.83880142],
           [126.89130156,  37.65233   ],
           ...,
           [126.86976731,  35.14724114],
           [126.87579812,  35.15646156],
           [126.86377243,  35.11873934]])



약 180만개의 좌표 정보가 1초만에 원하는 좌표계 정보로 변환되었다. 이제 경도 위도 좌표계로 변환된 NumPy Array를 DataFrame에 추가해본다.


```python
df['경도'] = result[:, 0]
df['위도'] = result[:, 1]
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
      <th>좌표정보(x)</th>
      <th>좌표정보(y)</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1870592</th>
      <td>192085.702260</td>
      <td>182111.703093</td>
      <td>126.911078</td>
      <td>35.138094</td>
    </tr>
    <tr>
      <th>1870593</th>
      <td>190363.418899</td>
      <td>181905.028328</td>
      <td>126.892182</td>
      <td>35.136216</td>
    </tr>
    <tr>
      <th>1870594</th>
      <td>188322.201767</td>
      <td>183130.570776</td>
      <td>126.869767</td>
      <td>35.147241</td>
    </tr>
    <tr>
      <th>1870595</th>
      <td>188873.020499</td>
      <td>184152.828430</td>
      <td>126.875798</td>
      <td>35.156462</td>
    </tr>
    <tr>
      <th>1870596</th>
      <td>187771.639116</td>
      <td>179969.196777</td>
      <td>126.863772</td>
      <td>35.118739</td>
    </tr>
  </tbody>
</table>
</div>



경도, 위도 좌표 정보를 바탕으로 지도에 시각화도 시도해볼 수 있다. 100개 정도만 랜덤 추출하여 그려본다.


```python
# 데이터 100개 랜덤 추출
sample = df.sample(n=100)

# 지도 중심 좌표 설정
lat_c, lon_c = 37.53165351203043, 126.9974246490573

# Folium 지도 객체 생성
m = folium.Map(location=[lat_c, lon_c], zoom_start=6)

# 마커 생성
for _, row in sample.iterrows():
    lat, lon = row['위도'], row['경도']
    folium.Marker(location=[lat, lon]).add_to(m)
    
m
```


<iframe
  src="/assets/html/folium/220121.html"
  style="width:100%; height:500px;"
></iframe>