---
title: Python folium 구글 지도를 배경으로 사용하기
categories: python
tags: Folium
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# Python으로 7가지 구글 지도를 쉽게 시각화하는 방법

<br>

## folium 설치

공간정보를 Python으로 시각화할 수 있도록 도와주는 `folium` 라이브러리를 아래와 같이 설치한다. 

```bash
pip install folium
```

<br>

## folium 임포트하기

공간정보 시각화 라이브러리인 `folium`을 불러온다.

```python
import folium
print(f"folium Version: {folium.__version__}")
```

    Folium Version: 0.12.1.post1
    

<br>

지도 시각화를 적용할 장소의 위도와 경도 그리고 줌 크기를 다음과 같이 미리 정의한다.


```python
# 위도, 경도
lat, lon = 37.504811111562, 127.025492036104
# 줌 크기
zoom_size = 12
```

## 구글 지도를 배경지도로 설정하기

### 1. Standard Road Map


```python
# 구글 지도 타일 설정
tiles = "http://mt0.google.com/vt/lyrs=m&hl=ko&x={x}&y={y}&z={z}"
# 속성 설정
attr = "Google"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
m
```

<iframe
  src="/assets/html/folium/google_map_1.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### 2.Terrain Map


```python
# 구글 지도 타일 설정
tiles = "http://mt0.google.com/vt/lyrs=p&hl=ko&x={x}&y={y}&z={z}"
# 속성 설정
attr = "Google"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
m
```

<iframe
  src="/assets/html/folium/google_map_2.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### 3.Somehow Altered Road Map


```python
# 구글 지도 타일 설정
tiles = "http://mt0.google.com/vt/lyrs=r&hl=ko&x={x}&y={y}&z={z}"
# 속성 설정
attr = "Google"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
m
```

<iframe
  src="/assets/html/folium/google_map_3.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### 4.Satellite Only


```python
# 구글 지도 타일 설정
tiles = "http://mt0.google.com/vt/lyrs=s&hl=ko&x={x}&y={y}&z={z}"
# 속성 설정
attr = "Google"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
m
```

<iframe
  src="/assets/html/folium/google_map_4.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### 5.Terrain Only


```python
# 구글 지도 타일 설정
tiles = "http://mt0.google.com/vt/lyrs=t&hl=ko&x={x}&y={y}&z={z}"
# 속성 설정
attr = "Google"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
m
```

<iframe
  src="/assets/html/folium/google_map_5.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### 6.Hybrid


```python
# 구글 지도 타일 설정
tiles = "http://mt0.google.com/vt/lyrs=y&hl=ko&x={x}&y={y}&z={z}"
# 속성 설정
attr = "Google"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
m
```

<iframe
  src="/assets/html/folium/google_map_6.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### 7.Roads Only


```python
# 구글 지도 타일 설정
tiles = "http://mt0.google.com/vt/lyrs=h&hl=ko&x={x}&y={y}&z={z}"
# 속성 설정
attr = "Google"
# 지도 객체 생성
m = folium.Map(location = [lat, lon],
               zoom_start = zoom_size,
               tiles = tiles,
               attr = attr)
m
```

<iframe
  src="/assets/html/folium/google_map_7.html"
  style="width:100%; height:500px;"
></iframe>

<br>

## 참고

- [Folium](https://python-visualization.github.io/folium/)
- [Leaflet 맛보기 Chapter. 2. Leaflet Google Map](https://bbong95.github.io/leaflet/2018/07/30/Leaflet-%EB%A7%9B%EB%B3%B4%EA%B8%B0-2%ED%83%84/)