---
title: "카카오 로컬 API 파이썬 컨트롤러 만들기"
categories: python
tags: 
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

> # 카카오 로컬 API

[카카오 로컬 API 문서](https://developers.kakao.com/docs/latest/ko/local/common#intro)에 따르면 **카카오 로컬(local) API**는 키워드로 특정 장소 정보를 조회하거나, 좌표를 주소 또는 행정구역으로 변환하는 등 장소에 대한 정보를 제공한다. 특정 카테고리로 장소를 검색하는 등 폭넓은 활용이 가능하며, 지번 주소와 도로명 주소 체계를 모두 지원한다. 구체적인 기능 목록은 다음과 같다.

**기능 목록**  

No | Name |	Description
-- | ----- | -------------
01 | 주소 검색 |	특정 주소에 해당하는 장소 정보의 좌표 검색
02 | 좌표-행정구역정보 변환 |	좌표 값을 행정동, 법정동으로 변환<br>맛집, 날씨 등 위치에 맞는 정보 제공 서비스에 활용
03 | 좌표-주소 변환 |	특정 좌표의 지번 주소 및 도로명 주소 제공
04 | 좌표계 변환 |	특정 체계의 좌표 값을 다른 체계의 좌표 값으로 변환<br>지원되는 좌표계: WGS84, WCONGNAMUL, CONGNAMUL, WTM, TM, KTM, UTM, BESSEL, WKTM, WUTM
05 | 키워드 검색 |	키워드로 관련 장소 및 상세 정보 검색<br>각 장소 상세 페이지 URL 제공
06 | 카테고리 검색 |	카테고리로 관련 장소 및 상세 정보 검색<br>각 장소 상세 페이지 URL 제공

파이썬으로 카카오 로컬 API 서비스를 쉽게 이용할 수 있도록 하기 위해 간단한 컨트롤러를 만들어보자. 먼저, API를 사용하려면 [Kakao Developers - 내 애플리케이션](https://developers.kakao.com/console/app)에서 애플리케이션을 추가해야 한다. 여기에서는 애플리케이션이 추가되었다고 가정하고 진행한다. `json`과 `requests` 모듈을 import하고 간단한 컨트롤러 클래스를 만들어보겠다. 각 메서드의 구체적인 동작 방식은 [개발 가이드](https://developers.kakao.com/docs/latest/ko/local/dev-guide)를 참조하였다.

**참고**  

- [PyKakao](https://github.com/WooilJeong/PyKakao)

PyKakao는 kakao developers에서 제공하는 로컬(Local) API를 이용할 수 있는 Python Client이다. PyKakao를 설치한 경우 아래와 같이 직접 API 클래스를 구현하지 않아도 된다.


<br>

## KakaoLocalAPI 클래스 생성

Kakao Developers 에서 발급받은 REST API 키로 클래스를 초기화할 수 있도록 설정한다.

```python
import json
import requests

class KakaoLocalAPI:
    """
    Kakao Local API 컨트롤러
    """

    def __init__(self, rest_api_key):
        """
        Rest API키 초기화 및 기능 별 URL 설정
        """

        # REST API 키 설정
        self.rest_api_key = rest_api_key
        self.headers = {"Authorization": "KakaoAK {}".format(rest_api_key)}

        # 서비스 별 URL 설정

        # 01 주소 검색
        self.URL_01 = "https://dapi.kakao.com/v2/local/search/address.json"
        # 02 좌표-행정구역정보 변환
        self.URL_02 = "https://dapi.kakao.com/v2/local/geo/coord2regioncode.json"
        # 03 좌표-주소 변환
        self.URL_03 = "https://dapi.kakao.com/v2/local/geo/coord2address.json"
        # 04 좌표계 변환
        self.URL_04 = "https://dapi.kakao.com/v2/local/geo/transcoord.json"
        # 05 키워드 검색
        self.URL_05 = "https://dapi.kakao.com/v2/local/search/keyword.json"
        # 06 카테고리 검색
        self.URL_06 = "https://dapi.kakao.com/v2/local/search/category.json"
```

<br>

## 01 주소 검색 메서드

```python
    def search_address(self, query, analyze_type=None, page=None, size=None):
        """
        01 주소 검색
        """
        params = {"query": f"{query}"}

        if analyze_type != None:
            params["analyze_type"] = f"{analyze_type}"

        if page != None:
            params['page'] = f"{page}"

        if size != None:
            params['size'] = f"{size}"

        res = requests.get(self.URL_01, headers=self.headers, params=params)
        document = json.loads(res.text)

        return document
```

<br>

## 02 좌표-행정구역정보 변환 메서드

```python

    def geo_coord2regioncode(self, x, y, input_coord=None, output_coord=None):
        """
        02 좌표-행정구역정보 변환
        """
        params = {"x": f"{x}",
                  "y": f"{y}"}
        
        if input_coord != None:
            params['input_coord'] = f"{input_coord}"
        
        if output_coord != None:
            params['output_coord'] = f"{output_coord}"
            
        res = requests.get(self.URL_02, headers=self.headers, params=params)
        document = json.loads(res.text)
        
        return document
```

<br>

## 03 좌표-주소 변환 메서드

```python

    def geo_coord2address(self, x, y, input_coord=None):
        """
        03 좌표-주소 변환
        """
        params = {"x": f"{x}",
                  "y": f"{y}"}
        
        if input_coord != None:
            params['input_coord'] = f"{input_coord}"
            
        res = requests.get(self.URL_03, headers=self.headers, params=params)
        document = json.loads(res.text)
        
        return document
        
```

<br>

## 04 좌표계 변환 메서드

```python

    def geo_transcoord(self, x, y, output_coord, input_coord=None):
        """
        04 좌표계 변환
        """
        params = {"x": f"{x}",
                  "y": f"{y}",
                  "output_coord": f"{output_coord}"}
        
        if input_coord != None:
            params['input_coord'] = f"{input_coord}"
        
        res = requests.get(self.URL_04, headers=self.headers, params=params)
        document = json.loads(res.text)
        
        return document
```

<br>

## 05 키워드 검색 메서드

```python

    def search_keyword(self,query,category_group_code=None,x=None,y=None,radius=None,rect=None,page=None,size=None,sort=None):
        """
        05 키워드 검색
        """
        params = {"query": f"{query}"}
        
        if category_group_code != None:
            params['category_group_code'] = f"{category_group_code}"
        if x != None:
            params['x'] = f"{x}"
        if y != None:
            params['y'] = f"{y}"
        if radius != None:
            params['radius'] = f"{radius}"
        if rect != None:
            params['rect'] = f"{rect}"
        if page != None:
            params['page'] = f"{page}"
        if size != None:
            params['size'] = f"{params}"
        if sort != None:
            params['sort'] = f"{sort}"
        
        res = requests.get(self.URL_05, headers=self.headers, params=params)
        document = json.loads(res.text)
        
        return document
```

<br>

## 06 카테고리 검색 메서드

```python

    def search_category(self, category_group_code, x, y, radius=None, rect=None, page=None, size=None, sort=None):
        """
        06 카테고리 검색
        """
        params = {'category_group_code': f"{category_group_code}",
                  'x': f"{x}",
                  'y': f"{y}"}
        
        if radius != None:
            params['radius'] = f"{radius}"
        if rect != None:
            params['rect'] = f"{rect}"
        if page != None:
            params['page'] = f"{page}"
        if size != None:
            params['size'] = f"{size}"
        if sort != None:
            params['sort'] = f"{sort}"
            
        res = requests.get(self.URL_06, headers=self.headers, params=params)
        document = json.loads(res.text)
        
        return document
```

<br>

## API 인증 및 인스턴스 생성

카카오에서 발급받은 REST API 키로 인스턴스 `kakao`를 초기화한다.


```python
# REST API 키
rest_api_key = "REST API KEY"

kakao = KakaoLocalAPI(rest_api_key)
```

<br>

## 01 주소 검색


```python
## Set params
query = "경기도 성남시 분당구 백현동 판교역로146번길 20"

## Request
result_01 = kakao.search_address(query)
result_01
```




    {'documents': [{'address': {'address_name': '경기 성남시 분당구 백현동 541',
        'b_code': '4113511000',
        'h_code': '4113565700',
        'main_address_no': '541',
        'mountain_yn': 'N',
        'region_1depth_name': '경기',
        'region_2depth_name': '성남시 분당구',
        'region_3depth_h_name': '백현동',
        'region_3depth_name': '백현동',
        'sub_address_no': '',
        'x': '127.1120874858',
        'y': '37.3927946193963'},
       'address_name': '경기 성남시 분당구 판교역로146번길 20',
       'address_type': 'ROAD_ADDR',
       'road_address': {'address_name': '경기 성남시 분당구 판교역로146번길 20',
        'building_name': '현대백화점 판교점',
        'main_building_no': '20',
        'region_1depth_name': '경기',
        'region_2depth_name': '성남시 분당구',
        'region_3depth_name': '백현동',
        'road_name': '판교역로146번길',
        'sub_building_no': '',
        'underground_yn': 'N',
        'x': '127.1120874858',
        'y': '37.3927946193963',
        'zone_no': '13529'},
       'x': '127.1120874858',
       'y': '37.3927946193963'}],
     'meta': {'is_end': True, 'pageable_count': 1, 'total_count': 1}}



<br>

## 02 좌표-행정구역정보 변환


```python
## Set params
x = 127.02449138906029
y = 37.50229485705552

input_coord = "WGS84" # WGS84, WCONGNAMUL, CONGNAMUL, WTM, TM
output_coord = "TM" # WGS84, WCONGNAMUL, CONGNAMUL, WTM, TM

## Request
result_2 = kakao.geo_coord2regioncode(x,y, input_coord, output_coord)
result_2
```




    {'meta': {'total_count': 2},
     'documents': [{'region_type': 'B',
       'code': '1165010800',
       'address_name': '서울특별시 서초구 서초동',
       'region_1depth_name': '서울특별시',
       'region_2depth_name': '서초구',
       'region_3depth_name': '서초동',
       'region_4depth_name': '',
       'x': 201618.64566571394,
       'y': 442961.8021336915},
      {'region_type': 'H',
       'code': '1165053100',
       'address_name': '서울특별시 서초구 서초4동',
       'region_1depth_name': '서울특별시',
       'region_2depth_name': '서초구',
       'region_3depth_name': '서초4동',
       'region_4depth_name': '',
       'x': 202060.27404560513,
       'y': 444501.80743905855}]}



<br>

## 03 좌표-주소 변환


```python
## Set params
x = 127.02449138906029
y = 37.50229485705552

input_coord = "WGS84" # WGS84, WCONGNAMUL, CONGNAMUL, WTM, TM

## Request
result_3 = kakao.geo_coord2address(x,y, input_coord)
result_3
```




    {'meta': {'total_count': 1},
     'documents': [{'road_address': None,
       'address': {'address_name': '서울 서초구 서초동 1309-12',
        'region_1depth_name': '서울',
        'region_2depth_name': '서초구',
        'region_3depth_name': '서초동',
        'mountain_yn': 'N',
        'main_address_no': '1309',
        'sub_address_no': '12',
        'zip_code': ''}}]}



<br>

## 04 좌표계 변환


```python
## Set params
x = 127.02449138906029
y = 37.50229485705552

input_coord = "WGS84" # WGS84, WCONGNAMUL, CONGNAMUL, WTM, TM
output_coord = "TM" # WGS84, WCONGNAMUL, CONGNAMUL, WTM, TM

## Request
result_4 = kakao.geo_transcoord(x,y,output_coord)
result_4
```




    {'meta': {'total_count': 1},
     'documents': [{'x': 202095.86924850685, 'y': 444453.71117371507}]}



<br>

## 05 키워드 검색


```python
## Set params
query = "현대백화점"

## Request
result_5 = kakao.search_keyword(query)
result_5
```




    {'documents': [{'address_name': '서울 강남구 삼성동 159-7',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '18384587',
       'phone': '02-552-2233',
       'place_name': '현대백화점 무역센터점',
       'place_url': 'http://place.map.kakao.com/18384587',
       'road_address_name': '서울 강남구 테헤란로 517',
       'x': '127.0598086324825',
       'y': '37.50857933831633'},
      {'address_name': '서울 강남구 압구정동 429',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '21297272',
       'phone': '02-547-2233',
       'place_name': '현대백화점 압구정본점',
       'place_url': 'http://place.map.kakao.com/21297272',
       'road_address_name': '서울 강남구 압구정로 165',
       'x': '127.02747045214494',
       'y': '37.527371799149506'},
      {'address_name': '경기 성남시 분당구 백현동 541',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '27572094',
       'phone': '031-5170-2233',
       'place_name': '현대백화점 판교점',
       'place_url': 'http://place.map.kakao.com/27572094',
       'road_address_name': '경기 성남시 분당구 판교역로146번길 20',
       'x': '127.112052030116',
       'y': '37.3927971758692'},
      {'address_name': '서울 양천구 목동 916',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '26825477',
       'phone': '02-2163-2233',
       'place_name': '현대백화점 목동점',
       'place_url': 'http://place.map.kakao.com/26825477',
       'road_address_name': '서울 양천구 목동동로 257',
       'x': '126.87525853214666',
       'y': '37.526654089298255'},
      {'address_name': '대구 중구 계산동2가 200',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '26588534',
       'phone': '053-245-2233',
       'place_name': '현대백화점 대구점',
       'place_url': 'http://place.map.kakao.com/26588534',
       'road_address_name': '대구 중구 달구벌대로 2077',
       'x': '128.59063111852757',
       'y': '35.86662835745753'},
      {'address_name': '서울 구로구 신도림동 692',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '26776860',
       'phone': '02-2622-2233',
       'place_name': '현대백화점 디큐브시티점',
       'place_url': 'http://place.map.kakao.com/26776860',
       'road_address_name': '서울 구로구 경인로 662',
       'x': '126.88958060554663',
       'y': '37.50910419634123'},
      {'address_name': '서울 성북구 길음동 20-1',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '8156625',
       'phone': '02-2117-2233',
       'place_name': '현대백화점 미아점',
       'place_url': 'http://place.map.kakao.com/8156625',
       'road_address_name': '서울 성북구 동소문로 315',
       'x': '127.028748311194',
       'y': '37.608482732465'},
      {'address_name': '서울 서대문구 창천동 30-33',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '21695719',
       'phone': '02-3145-2233',
       'place_name': '현대백화점 신촌점',
       'place_url': 'http://place.map.kakao.com/21695719',
       'road_address_name': '서울 서대문구 신촌로 83',
       'x': '126.93581652251622',
       'y': '37.556153500544426'},
      {'address_name': '서울 강동구 천호동 572',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '13125937',
       'phone': '02-488-2233',
       'place_name': '현대백화점 천호점',
       'place_url': 'http://place.map.kakao.com/13125937',
       'road_address_name': '서울 강동구 천호대로 1005',
       'x': '127.1244432563588',
       'y': '37.538841968226784'},
      {'address_name': '충북 청주시 흥덕구 복대동 3380',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '17912136',
       'phone': '043-909-2233',
       'place_name': '현대백화점 충청점',
       'place_url': 'http://place.map.kakao.com/17912136',
       'road_address_name': '충북 청주시 흥덕구 직지대로 308',
       'x': '127.426506441269',
       'y': '36.6448287847106'},
      {'address_name': '경기 고양시 일산서구 대화동 2602',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '12526820',
       'phone': '031-822-2233',
       'place_name': '현대백화점 킨텍스점',
       'place_url': 'http://place.map.kakao.com/12526820',
       'road_address_name': '경기 고양시 일산서구 호수로 817',
       'x': '126.75118105784215',
       'y': '37.66818614056687'},
      {'address_name': '경기 부천시 중동 1164',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '14661403',
       'phone': '032-623-2233',
       'place_name': '현대백화점 중동점',
       'place_url': 'http://place.map.kakao.com/14661403',
       'road_address_name': '경기 부천시 길주로 180',
       'x': '126.76206845886405',
       'y': '37.50434088197503'},
      {'address_name': '울산 남구 삼산동 1521-1',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '8154974',
       'phone': '052-228-2233',
       'place_name': '현대백화점 울산점',
       'place_url': 'http://place.map.kakao.com/8154974',
       'road_address_name': '울산 남구 삼산로 261',
       'x': '129.33574650999',
       'y': '35.5395950301785'},
      {'address_name': '서울 서대문구 창천동 30-1',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '26992232',
       'phone': '02-3145-2233',
       'place_name': '현대백화점유플렉스 신촌점',
       'place_url': 'http://place.map.kakao.com/26992232',
       'road_address_name': '서울 서대문구 연세로 13',
       'x': '126.93661730245256',
       'y': '37.55673777941326'},
      {'address_name': '부산 동구 범일동 62-5',
       'category_group_code': '',
       'category_group_name': '',
       'category_name': '가정,생활 > 백화점 > 현대백화점',
       'distance': '',
       'id': '21748301',
       'phone': '051-667-2233',
       'place_name': '현대백화점 부산점',
       'place_url': 'http://place.map.kakao.com/21748301',
       'road_address_name': '부산 동구 범일로 125',
       'x': '129.0586370818922',
       'y': '35.141263504491185'}],
     'meta': {'is_end': False,
      'pageable_count': 45,
      'same_name': {'keyword': '현대백화점', 'region': [], 'selected_region': ''},
      'total_count': 3642}}



<br>

## 06 카테고리 검색


```python
## Set params
category_group_code = "MT1"
x = 127.02449138906029
y = 37.50229485705552
radius = 20000

## Request
result_6 = kakao.search_category(category_group_code, x, y, radius)
result_6
```




    {'documents': [{'address_name': '서울 서초구 서초동 1316-28',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 노브랜드',
       'distance': '469',
       'id': '1712876948',
       'phone': '02-537-5827',
       'place_name': '노브랜드 서울서초점',
       'place_url': 'http://place.map.kakao.com/1712876948',
       'road_address_name': '서울 서초구 서초대로73길 7',
       'x': '127.02538233025993',
       'y': '37.49812386955666'},
      {'address_name': '서울 서초구 서초4동 1302-4',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 이마트 에브리데이',
       'distance': '231',
       'id': '12421792',
       'phone': '02-534-3651',
       'place_name': '이마트에브리데이 서초동점',
       'place_url': 'http://place.map.kakao.com/12421792',
       'road_address_name': '서울 서초구 서운로 216',
       'x': '127.021872220302',
       'y': '37.5023574981132'},
      {'address_name': '서울 서초구 서초동 1685-8',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 노브랜드',
       'distance': '1085',
       'id': '320786076',
       'phone': '02-537-8217',
       'place_name': '노브랜드 서울서초G5점',
       'place_url': 'http://place.map.kakao.com/320786076',
       'road_address_name': '서울 서초구 서초중앙로24길 27',
       'x': '127.01617980670726',
       'y': '37.495104413126136'},
      {'address_name': '서울 강남구 역삼동 832-6',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 노브랜드',
       'distance': '1272',
       'id': '420935474',
       'phone': '02-2051-9666',
       'place_name': '노브랜드 강남역삼점',
       'place_url': 'http://place.map.kakao.com/420935474',
       'road_address_name': '서울 강남구 강남대로 324',
       'x': '127.03089279874023',
       'y': '37.49202454273762'},
      {'address_name': '서울 강남구 역삼동 824-11',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 롯데슈퍼',
       'distance': '785',
       'id': '1580062235',
       'phone': '02-569-4007',
       'place_name': '롯데슈퍼 강남역가맹점',
       'place_url': 'http://place.map.kakao.com/1580062235',
       'road_address_name': '서울 강남구 강남대로84길 23',
       'x': '127.030555642102',
       'y': '37.4971207255714'},
      {'address_name': '서울 강남구 논현동 140',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > GS더프레시',
       'distance': '859',
       'id': '26890613',
       'phone': '02-548-6071',
       'place_name': 'GS더프레시 강남영동점',
       'place_url': 'http://place.map.kakao.com/26890613',
       'road_address_name': '서울 강남구 강남대로128길 20',
       'x': '127.023652458739',
       'y': '37.5100084909034'},
      {'address_name': '서울 강남구 역삼동 833-4',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 이마트 에브리데이',
       'distance': '1217',
       'id': '2115939257',
       'phone': '02-6951-3011',
       'place_name': '이마트에브리데이 역삼점',
       'place_url': 'http://place.map.kakao.com/2115939257',
       'road_address_name': '서울 강남구 역삼로 124',
       'x': '127.03326371111147',
       'y': '37.49384213144032'},
      {'address_name': '서울 서초구 서초동 1685-3',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 홈플러스익스프레스',
       'distance': '1088',
       'id': '10979066',
       'phone': '02-3478-8545',
       'place_name': '홈플러스익스프레스 서초점',
       'place_url': 'http://place.map.kakao.com/10979066',
       'road_address_name': '서울 서초구 서초중앙로 188',
       'x': '127.012801613753',
       'y': '37.4992305364984'},
      {'address_name': '서울 서초구 반포동 20-51',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 홈플러스익스프레스',
       'distance': '1048',
       'id': '11779677',
       'phone': '02-536-8545',
       'place_name': '홈플러스익스프레스 반포점',
       'place_url': 'http://place.map.kakao.com/11779677',
       'road_address_name': '서울 서초구 사평대로45길 10-25',
       'x': '127.013938393932',
       'y': '37.506611446308'},
      {'address_name': '서울 서초구 반포동 30-25',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > GS더프레시',
       'distance': '1090',
       'id': '21308698',
       'phone': '02-592-1832',
       'place_name': 'GS더프레시 서초점',
       'place_url': 'http://place.map.kakao.com/21308698',
       'road_address_name': '서울 서초구 서초중앙로 238',
       'x': '127.01218644358',
       'y': '37.50290084932801'},
      {'address_name': '서울 서초구 반포동 19-4',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 노브랜드',
       'distance': '1604',
       'id': '914807858',
       'phone': '02-537-8491',
       'place_name': '노브랜드 강남터미널점',
       'place_url': 'http://place.map.kakao.com/914807858',
       'road_address_name': '서울 서초구 신반포로 194',
       'x': '127.006971407417',
       'y': '37.5060570437569'},
      {'address_name': '서울 서초구 반포동 54-1',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 롯데슈퍼',
       'distance': '1214',
       'id': '707649582',
       'phone': '',
       'place_name': '롯데슈퍼 반포2점',
       'place_url': 'http://place.map.kakao.com/707649582',
       'road_address_name': '서울 서초구 고무래로 22',
       'x': '127.010757453712',
       'y': '37.5025716705172'},
      {'address_name': '서울 강남구 역삼동 748-14',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 롯데슈퍼',
       'distance': '1299',
       'id': '26875613',
       'phone': '02-3453-5053',
       'place_name': '롯데슈퍼 역삼3점',
       'place_url': 'http://place.map.kakao.com/26875613',
       'road_address_name': '서울 강남구 역삼로 153',
       'x': '127.03601665566812',
       'y': '37.49502768006294'},
      {'address_name': '서울 강남구 논현동 129',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 슈퍼마켓 > 대형슈퍼 > 홈플러스익스프레스',
       'distance': '1349',
       'id': '16272886',
       'phone': '02-516-8546',
       'place_name': '홈플러스익스프레스 학동역점',
       'place_url': 'http://place.map.kakao.com/16272886',
       'road_address_name': '서울 강남구 학동로 176',
       'x': '127.030245631984',
       'y': '37.5135533395888'},
      {'address_name': '서울 서초구 잠원동 70-2',
       'category_group_code': 'MT1',
       'category_group_name': '대형마트',
       'category_name': '가정,생활 > 대형마트 > 킴스클럽',
       'distance': '1693',
       'id': '18276130',
       'phone': '02-530-5000',
       'place_name': '킴스클럽 강남점',
       'place_url': 'http://place.map.kakao.com/18276130',
       'road_address_name': '서울 서초구 잠원로 51',
       'x': '127.00743769002541',
       'y': '37.50923936481701'}],
     'meta': {'is_end': False,
      'pageable_count': 45,
      'same_name': None,
      'total_count': 665}}



<br>

## 전체 소스 코드

```python
import json
import requests

class KakaoLocalAPI:
    """
    Kakao Local API 컨트롤러
    """

    def __init__(self, rest_api_key):
        """
        Rest API키 초기화 및 기능 별 URL 설정
        """

        # REST API 키 설정
        self.rest_api_key = rest_api_key
        self.headers = {"Authorization": "KakaoAK {}".format(rest_api_key)}

        # 서비스 별 URL 설정

        # 01 주소 검색
        self.URL_01 = "https://dapi.kakao.com/v2/local/search/address.json"
        # 02 좌표-행정구역정보 변환
        self.URL_02 = "https://dapi.kakao.com/v2/local/geo/coord2regioncode.json"
        # 03 좌표-주소 변환
        self.URL_03 = "https://dapi.kakao.com/v2/local/geo/coord2address.json"
        # 04 좌표계 변환
        self.URL_04 = "https://dapi.kakao.com/v2/local/geo/transcoord.json"
        # 05 키워드 검색
        self.URL_05 = "https://dapi.kakao.com/v2/local/search/keyword.json"
        # 06 카테고리 검색
        self.URL_06 = "https://dapi.kakao.com/v2/local/search/category.json"

    def search_address(self, query, analyze_type=None, page=None, size=None):
        """
        01 주소 검색
        """
        params = {"query": f"{query}"}

        if analyze_type != None:
            params["analyze_type"] = f"{analyze_type}"

        if page != None:
            params['page'] = f"{page}"

        if size != None:
            params['size'] = f"{size}"

        res = requests.get(self.URL_01, headers=self.headers, params=params)
        document = json.loads(res.text)

        return document
    
    def geo_coord2regioncode(self, x, y, input_coord=None, output_coord=None):
        """
        02 좌표-행정구역정보 변환
        """
        params = {"x": f"{x}",
                  "y": f"{y}"}
        
        if input_coord != None:
            params['input_coord'] = f"{input_coord}"
        
        if output_coord != None:
            params['output_coord'] = f"{output_coord}"
            
        res = requests.get(self.URL_02, headers=self.headers, params=params)
        document = json.loads(res.text)
        
        return document
        
    
    def geo_coord2address(self, x, y, input_coord=None):
        """
        03 좌표-주소 변환
        """
        params = {"x": f"{x}",
                  "y": f"{y}"}
        
        if input_coord != None:
            params['input_coord'] = f"{input_coord}"
            
        res = requests.get(self.URL_03, headers=self.headers, params=params)
        document = json.loads(res.text)
        
        return document
        
    
    def geo_transcoord(self, x, y, output_coord, input_coord=None):
        """
        04 좌표계 변환
        """
        params = {"x": f"{x}",
                  "y": f"{y}",
                  "output_coord": f"{output_coord}"}
        
        if input_coord != None:
            params['input_coord'] = f"{input_coord}"
        
        res = requests.get(self.URL_04, headers=self.headers, params=params)
        document = json.loads(res.text)
        
        return document
        
    
    def search_keyword(self,query,category_group_code=None,x=None,y=None,radius=None,rect=None,page=None,size=None,sort=None):
        """
        05 키워드 검색
        """
        params = {"query": f"{query}"}
        
        if category_group_code != None:
            params['category_group_code'] = f"{category_group_code}"
        if x != None:
            params['x'] = f"{x}"
        if y != None:
            params['y'] = f"{y}"
        if radius != None:
            params['radius'] = f"{radius}"
        if rect != None:
            params['rect'] = f"{rect}"
        if page != None:
            params['page'] = f"{page}"
        if size != None:
            params['size'] = f"{params}"
        if sort != None:
            params['sort'] = f"{sort}"
        
        res = requests.get(self.URL_05, headers=self.headers, params=params)
        document = json.loads(res.text)
        
        return document
    
            
        
    def search_category(self, category_group_code, x, y, radius=None, rect=None, page=None, size=None, sort=None):
        """
        06 카테고리 검색
        """
        params = {'category_group_code': f"{category_group_code}",
                  'x': f"{x}",
                  'y': f"{y}"}
        
        if radius != None:
            params['radius'] = f"{radius}"
        if rect != None:
            params['rect'] = f"{rect}"
        if page != None:
            params['page'] = f"{page}"
        if size != None:
            params['size'] = f"{size}"
        if sort != None:
            params['sort'] = f"{sort}"
            
        res = requests.get(self.URL_06, headers=self.headers, params=params)
        document = json.loads(res.text)
        
        return document
```
