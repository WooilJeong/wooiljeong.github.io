---
title: Python folium 네이버 지도를 배경으로 사용하기
categories: python
tags: Folium
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# Python으로 4가지 네이버 지도를 쉽게 시각화하는 방법

<br>

## folium 설치

공간정보를 Python으로 시각화할 수 있도록 도와주는 `folium` 라이브러리를 아래와 같이 설치한다. 

```bash
pip install folium
```

<br>

## folium 임포트하기


```python
import folium
print(f"folium Version: {folium.__version__}")
```

    folium Version: 0.12.1.post1


지도 시각화를 적용할 장소의 위도와 경도 그리고 줌 크기를 다음과 같이 미리 정의한다.


```python
# 위도, 경도
lat, lon = 37.504811111562, 127.025492036104
# 줌 크기
zoom_size = 15
```

## 네이버 지도를 배경지도로 설정하기

### 1.일반지도(basic)


```python
# 네이버지도 타일 설정
tiles = "https://map.pstatic.net/nrb/styles/basic/1663918673/{z}/{x}/{y}@2x.png?mt=bg.ol.sw"
# 속성 설정
attr = "Naver"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
folium.Marker(location = [lat, lon]).add_to(m)
m
```

<iframe
  src="/assets/html/folium/naver_map_1.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### 2.위성지도(satellite)


```python
# 네이버지도 타일 설정
tiles = "https://map.pstatic.net/nrb/styles/satellite/1663918673/{z}/{x}/{y}@2x.png?mt=bg.ol.sw"
# 속성 설정
attr = "Naver"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
folium.Marker(location = [lat, lon]).add_to(m)
m
```

<iframe
  src="/assets/html/folium/naver_map_2.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### 3.지형도(terrain)


```python
# 네이버지도 타일 설정
tiles = "https://map.pstatic.net/nrb/styles/terrain/1663918673/{z}/{x}/{y}@2x.png?mt=bg.ol.sw"
# 속성 설정
attr = "Naver"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
folium.Marker(location = [lat, lon]).add_to(m)
m
```

<iframe
  src="/assets/html/folium/naver_map_3.html"
  style="width:100%; height:500px;"
></iframe>

<br>


### 4.지적편집도(cadastral)


```python
# 네이버지도 타일 설정
tiles = "https://map.pstatic.net/nrb/styles/basic/1663918673/{z}/{x}/{y}@2x.png?mt=bg.ol.sw.lp"
# 속성 설정
attr = "Naver"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
folium.Marker(location = [lat, lon]).add_to(m)
m
```

<iframe
  src="/assets/html/folium/naver_map_4.html"
  style="width:100%; height:500px;"
></iframe>

<br>

## 참고

- [Folium](https://python-visualization.github.io/folium/)
- [Leaflet 맛보기 Chapter. 3. Leaflet Naver Map](https://bbong95.github.io/leaflet/2018/08/02/Leaflet-%EB%A7%9B%EB%B3%B4%EA%B8%B0-3%ED%83%84/)
