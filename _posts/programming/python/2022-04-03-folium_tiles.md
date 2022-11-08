---
title: "Folium - 배경지도 변경하기(feat.Vworld 오픈API)"
categories: python
tags: Folium
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---
{% raw %}
# Folium 배경지도 변경하기

Folium에서 기본적으로 제공하는 타일을 이용하여 배경지도를 변경하는 방법과 Vworld에서 제공하는 OpenAPI를 이용하여 배경지도를 변경하는 방법에 대해 알아보자.

<br>

## Vworld 오픈API 인증키 발급

우선 [Vworld 오픈API](https://www.vworld.kr/dev/v4api.do)에서 인증키를 발급받는다. 인증키를 발급받는 과정은 쉽게 파악할 수 있는 수준이므로 설명은 생략한다.

<br>

## Vworld 오픈API 인증키 설정파일(config.py) 생성

프로젝트 경로에 `config.py` 파일을 만든다. 이 파일을 열어 Vworld 오픈API에서 발급받은 인증키를 아래와 같이 `vworld_key`의 값으로 할당한다.

config.py
```python
vworld_key="VWORLD 인증키"
```

<br>

## Folium 설치

공간정보를 Python으로 시각화할 수 있도록 도와주는 `Folium` 라이브러리를 아래와 같이 설치한다. 

```bash
pip install folium
```

<br>

## Folium 배경지도 변경하기

### 모듈 임포트하기

공간정보 시각화 라이브러리인 `Folium` 과 설장파일(`config.py`)에 저장해둔 Vworld 오픈API의 인증키(`vworld_key`) 값을 불러온다.


```python
import folium
from config import vworld_key

print(f"Folium Version: {folium.__version__}")
```

    Folium Version: 0.12.1.post1
    

<br>

### folium 기본 지도 생성하기

시각화할 대상지의 위경도 좌표 값을 구해 아래와 같이 위도, 경도 순으로 중심좌표로 설정한다. 공간정보의 줌 크기(기본값: 10)도 설정할 수 있다. 예시의 위경도 값은 신논현역의 좌표값이다.


```python
# 지도 중심 경위도 좌표(위도, 경도) 설정하기
lat, lon = 37.504811111562, 127.025492036104
# 줌 설정하기
zoom_size = 15

# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

m
```

<iframe
  src="/assets/html/folium/OpenStreetMap.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### folium 배경지도 변경하기 (기본타일 이용하기)

별도의 타일을 지정하지 않으면 folium은 배경지도로 `OpenStreetMap`에서 제공하는 오픈 소스 지도를 사용한다. 이외에도 기본적으로 제공되는 다른 배경지도를 아래와 같이 사용할 수도 있다.

- 타일 종류
    - OpenStreetMap (기본값)
    - Stamen Terrain, Stamen Toner, Stamen Watercolor
    - CartoDB positron, CartoDB dark_matter


```python
# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

# 배경지도 타일 설정하기
tiles = "Stamen Terrain"
# 배경지도 타일 레이어를 지도에 추가하기
folium.TileLayer(tiles=tiles).add_to(m)

m
```

<iframe
  src="/assets/html/folium/Stamen Terrain.html"
  style="width:100%; height:500px;"
></iframe>

```python
# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

# 배경지도 타일 설정하기
tiles = "Stamen Toner"
# 배경지도 타일 레이어를 지도에 추가하기
folium.TileLayer(tiles=tiles).add_to(m)

m
```

<iframe
  src="/assets/html/folium/Stamen Toner.html"
  style="width:100%; height:500px;"
></iframe>

<br>

```python
# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

# 배경지도 타일 설정하기
tiles = "Stamen Watercolor"
# 배경지도 타일 레이어를 지도에 추가하기
folium.TileLayer(tiles=tiles).add_to(m)

m
```

<iframe
  src="/assets/html/folium/Stamen Watercolor.html"
  style="width:100%; height:500px;"
></iframe>

<br>

```python
# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

# 배경지도 타일 설정하기
tiles = "CartoDB positron"
# 배경지도 타일 레이어를 지도에 추가하기
folium.TileLayer(tiles=tiles).add_to(m)

m
```

<iframe
  src="/assets/html/folium/CartoDB positron.html"
  style="width:100%; height:500px;"
></iframe>

<br>

```python
# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

# 배경지도 타일 설정하기
tiles = "CartoDB dark_matter"
# 배경지도 타일 레이어를 지도에 추가하기
folium.TileLayer(tiles=tiles).add_to(m)

m
```

<iframe
  src="/assets/html/folium/CartoDB dark_matter.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### folium 배경지도 변경하기 (Vworld 오픈API 이용하기)

기본적으로 제공되는 배경지도 타일 외에도 Vworld 오픈API를 이용하여 배경지도를 변경할 수 있다. 단, 이와 같이 커스텀 URL을 사용하는 경우에는 `attr`을 필수로 설정해야한다. 

- 타일 종류 / 타일 타입
    - Base / png
    - gray / png
    - midnight / png
    - Hybrid / png
    - Satellite / jpeg


```python
# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

# 배경지도 타일 설정하기
layer = "Base"
tileType = "png"
tiles = f"http://api.vworld.kr/req/wmts/1.0.0/{vworld_key}/{layer}/{{z}}/{{y}}/{{x}}.{tileType}"
attr = "Vworld"

folium.TileLayer(
    tiles=tiles,
    attr=attr,
    overlay=True,
    control=True
).add_to(m)

m
```

<iframe
  src="/assets/html/folium/vworld_Base.html"
  style="width:100%; height:500px;"
></iframe>

<br>

```python
# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

# 배경지도 타일 설정하기
layer = "gray"
tileType = "png"
tiles = f"http://api.vworld.kr/req/wmts/1.0.0/{vworld_key}/{layer}/{{z}}/{{y}}/{{x}}.{tileType}"
attr = "Vworld"

folium.TileLayer(
    tiles=tiles,
    attr=attr,
    overlay=True,
    control=True
).add_to(m)

m
```

<iframe
  src="/assets/html/folium/vworld_gray.html"
  style="width:100%; height:500px;"
></iframe>

<br>

```python
# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

# 배경지도 타일 설정하기
layer = "midnight"
tileType = "png"
tiles = f"http://api.vworld.kr/req/wmts/1.0.0/{vworld_key}/{layer}/{{z}}/{{y}}/{{x}}.{tileType}"
attr = "Vworld"

folium.TileLayer(
    tiles=tiles,
    attr=attr,
    overlay=True,
    control=True
).add_to(m)

m
```

<iframe
  src="/assets/html/folium/vworld_midnight.html"
  style="width:100%; height:500px;"
></iframe>

<br>

```python
# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

# 배경지도 타일 설정하기
layer = "Hybrid"
tileType = "png"
tiles = f"http://api.vworld.kr/req/wmts/1.0.0/{vworld_key}/{layer}/{{z}}/{{y}}/{{x}}.{tileType}"
attr = "Vworld"

folium.TileLayer(
    tiles=tiles,
    attr=attr,
    overlay=True,
    control=True
).add_to(m)

m
```

<iframe
  src="/assets/html/folium/vworld_Hybrid.html"
  style="width:100%; height:500px;"
></iframe>

<br>

```python
# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

# 배경지도 타일 설정하기
layer = "Satellite"
tileType = "jpeg"
tiles = f"http://api.vworld.kr/req/wmts/1.0.0/{vworld_key}/{layer}/{{z}}/{{y}}/{{x}}.{tileType}"
attr = "Vworld"

folium.TileLayer(
    tiles=tiles,
    attr=attr,
    overlay=True,
    control=True
).add_to(m)

m
```

<iframe
  src="/assets/html/folium/vworld_Satellite.html"
  style="width:100%; height:500px;"
></iframe>

<br>

## 참고

- [Vworld 오픈API](https://www.vworld.kr/dev/v4api.do)
- [Folium](https://python-visualization.github.io/folium/)
- [OpenStreetMap](https://www.openstreetmap.org/#map=17/37.38512/127.11419)
{% endraw %}