---
title: "Mapboxgl - 서울 아파트 가격 3D 지도에 시각화하기"
categories: python
tags: Mapboxgl
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---
{% raw %}
# Python으로 서울 아파트 가격 지도에 시각화하기

Mapboxgl을 이용하여 2021년 1월 ~ 2022년 4월 초에 거래된 서울시 아파트의 실거래가를 바탕으로 법정동별 아파트 평균평당가 및 거래량을 3D 지도에 시각화해보자.

<br>

## Mapbox 인증키 발급하기

우선 [Mapbox](https://www.mapbox.com/)에서 인증키를 발급받는다. 인증키를 발급받는 과정은 쉽게 파악할 수 있는 수준이므로 설명은 생략한다.

<br>

## Mapbox 인증키 설정파일(config.py) 생성

프로젝트 경로에 `config.py` 파일을 만든다. 이 파일을 열어 Mapbox에서 발급받은 인증키를 아래와 같이 `mapbox_key`의 값으로 할당한다.

config.py
```python
mapbox_key="mapbox 인증키"
```

<br>

## Mapboxgl 설치

공간정보를 Python으로 시각화할 수 있도록 도와주는 `Mapboxgl` 라이브러리를 아래와 같이 설치한다. 

```bash
pip install mapboxgl
```

<br>

## 모듈 임포트하기

공간정보 시각화 라이브러리인 `Mapboxgl`와 설장파일(`config.py`)에 저장해둔 Mapbox의 인증키(`mapbox_key`) 값을 불러온다.


```python
import mapboxgl
from config import mapbox_key

print(f"Mapboxgl Version: {mapboxgl.__version__}")
```

    Mapboxgl Version: 0.10.2
    

<br>

## GeoJson 데이터 불러오기

지도 위에 표현할 GeoJson 형태의 데이터를 불러온다. 서울시 법정동 경계를 나타내는 MultiPolygon 정보와 각 법정동의 2021년도 1월 ~ 2022년 4월 초 사이의 아파트 평균전용평당가와 거래량 정보를 아래와 같은 형식으로 불러온다.

```json
{'type': 'FeatureCollection',
 'bbox': [126.76448395819257,
  37.42829747687726,
  127.18379465864182,
  37.70145527730673],
 'features': [{'type': 'Feature',
   'geometry': {'type': 'MultiPolygon',
    'coordinates': [[[[126.97555981186262, 37.58968064981368],
       [126.97549103898153, 37.58955195599864],
       [126.97545638241279, 37.58948324824816],
       ...
       [127.17017313959616, 37.56227975758214],
       [127.17030475013962, 37.562404318360336]]]]},
   'properties': {'full_nm': '서울특별시 강동구 강일동',
    'emd_kor_nm': '강일동',
    'emd_eng_nm': 'Gangil-dong',
    'emd_cd': '11740110',
    '전용평당가': 4469.661683316599,
    '거래량': 61},
   'id': 'LT_C_ADEMD_INFO.126688'}]}
```


<br>

```python
import json
with open("./data/data.json", "r") as f:
    geo = json.load(f)
```

<br>

## Mapboxgl로 2D 지도 시각화하기


```python
from mapboxgl.viz import ChoroplethViz
from mapboxgl.utils import create_color_stops

# 중심 경도 위도 (ex. 남산타워 좌표값)
lon, lat = 126.987867837993, 37.5511225714939
center = [lon, lat]

# 색상 범주 설정
color_breaks = list(range(1000, 12000, 2000))
color_stops = create_color_stops(color_breaks, colors='BuPu')

# 2D 지도 생성
viz = ChoroplethViz(
    access_token = mapbox_key,
    data = geo,
    color_property = '전용평당가',
    color_stops = color_stops,
    center = center,
    zoom = 10
)

# 출력
viz.show()
```

<iframe
  src="/assets/html/mapboxgl/viz_2d.html"
  style="width:100%; height:500px;"
></iframe>

<br>

## Mapboxgl로 3D 지도 시각화하기


```python
from mapboxgl.utils import create_numeric_stops

# 지도 회전 각도 설정 (좌우, 상하)
viz.bearing = 15
viz.pitch = 45

# 높이 설정 - '거래량'
viz.height_property = '거래량'

# 높이 스케일 설정
numeric_stops = create_numeric_stops(list(range(0, 2000, 250)), 0, 10000)

viz.height_stops = numeric_stops
viz.height_function_type = 'interpolate'

# render again
viz.show()
```

<iframe
  src="/assets/html/mapboxgl/viz_3d.html"
  style="width:100%; height:500px;"
></iframe>

<br>

## 시각화 결과를 HTML 파일로 저장하기


```python
html = open("./output/viz_3d.html", "w", encoding="UTF-8")
html.write(viz.create_html())
html.close()
```

<br>

## 참고

- [Mapbox](https://www.mapbox.com/)
- [mapboxgl-jupyter GitHub](https://github.com/mapbox/mapboxgl-jupyter)
{% endraw %}