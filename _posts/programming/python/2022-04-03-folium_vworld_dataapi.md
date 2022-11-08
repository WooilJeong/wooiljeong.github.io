---
title: "Folium - Vworld 데이터 시각화하기"
categories: python
tags: Folium
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---
{% raw %}
# Folium Vworld 데이터 시각화하기

Vworld 오픈API의 데이터API에서 제공하는 연속지적도 서비스를 이용하여 데이터를 Folium 지도 위에 시각화하는 방법에 대해 알아보자.

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

## Folium Vworld 데이터 시각화하기

### 모듈 임포트하기

공간정보 시각화 라이브러리인 `Folium` 과 HTTP 요청을 보내기 위한 모듈인 `requests` 그리고 설장파일(`config.py`)에 저장해둔 Vworld 오픈API의 인증키(`vworld_key`) 값을 불러온다.


```python
import folium
import requests
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
zoom_size = 18

# folium 지도 생성하기
m = folium.Map(
    location=[lat, lon],
    zoom_start=zoom_size
)

m
```

<iframe
  src="/assets/html/folium/vworld_dataapi_1.html"
  style="width:100%; height:500px;"
></iframe>

<br>

### 데이터 API를 이용하여 연속지적도 시각화하기

Vworld 오픈API 중 데이터 API에서 제공하는 연속지적도 서비스를 이용해보자. API 관련 자세한 레퍼런스는 [여기](https://www.vworld.kr/dev/v4dv_2ddataguide2_s002.do?svcIde=cadastral)를 참고하면 된다. 아래와 같이 `get_data` 함수를 정의한다..


```python
def get_data(key, pnuCode):
    """
    연속지적도

    종류: 2D 데이터API 2.0
    분류: 토지
    서비스명: 연속지적도
    서비스ID: LP_PA_CBND_BUBUN
    제공처: 국토교통부
    버전: 1.0
    - key: Vworld Open API 인증키
    - pnuCode: PNU코드 19자리
    """
    # 엔드포인트
    endpoint = "http://api.vworld.kr/req/data"

    # 요청 파라미터
    service = "data"
    request = "GetFeature"
    data = "LP_PA_CBND_BUBUN"
    page = 1
    size = 1000
    attrFilter = f"pnu:=:{pnuCode}"

    # 요청 URL
    url = f"{endpoint}?service={service}&request={request}&data={data}&key={key}&attrFilter={attrFilter}&page={page}&size={size}"
    # 요청 결과
    res = json.loads(requests.get(url).text)
    # GeoJson 생성
    featureCollection = res["response"]["result"]["featureCollection"]

    return featureCollection
```

<br>

위에서 정의한 `get_data` 함수의 인자로 신논현역 부근에 있는 808타워의 PNU코드를 넣고 함수를 호출한다. 결과는 GeoJson의 형식에 맞는 딕셔너리 객체로 반환된다. 


<br>

```python
# PNU코드 설정하기 (신논현역 부근에 있는 808타워의 PNU코드)
pnuCode = "1168010100108080000"

# GeoJson 데이터 생성
geo = get_data(vworld_key, pnuCode)
geo
```




    {'type': 'FeatureCollection',
     'bbox': [127.02486036065964,
      37.504197005024714,
      127.02525030489706,
      37.50451059661578],
     'features': [{'type': 'Feature',
       'geometry': {'type': 'MultiPolygon',
        'coordinates': [[[[127.02497161140379, 37.50421258228713],
           [127.02492116614656, 37.504197005024714],
           [127.02486036065964, 37.50432432808612],
           [127.02491003672799, 37.504411715073026],
           [127.02492870770126, 37.50444450770394],
           [127.02513003632357, 37.504504203804466],
           [127.02513716202056, 37.50450636477527],
           [127.0251514133874, 37.50451059661578],
           [127.02525030489706, 37.50429875214337],
           [127.02497161140379, 37.50421258228713]]]]},
       'properties': {'pnu': '1168010100108080000',
        'jibun': '808 대',
        'bonbun': '808',
        'bubun': '',
        'addr': '서울특별시 강남구 역삼동 808',
        'gosi_year': '2021',
        'gosi_month': '01',
        'jiga': '73680000'},
       'id': 'LP_PA_CBND_BUBUN.838473'}]}



<br>

이제 아래와 같이 GeoJson 레이어를 추가하면 된다.

<br>


```python
# GeoJson 레이어 추가하기
folium.GeoJson(data=geo,
               name="geojson",
               tooltip=folium.GeoJsonTooltip(fields=('pnu', 'addr'),
                                             aliases=('PNU코드', '주소'))
               ).add_to(m)

m
```

<iframe
  src="/assets/html/folium/vworld_dataapi_2.html"
  style="width:100%; height:500px;"
></iframe>

<br>

## 참고

- [Vworld 오픈API](https://www.vworld.kr/dev/v4api.do)
- [2D 데이터API 2.0 레퍼런스](https://www.vworld.kr/dev/v4dv_2ddataguide2_s001.do)
- [Folium](https://python-visualization.github.io/folium/)
- [OpenStreetMap](https://www.openstreetmap.org/#map=17/37.38512/127.11419)
{% endraw %}