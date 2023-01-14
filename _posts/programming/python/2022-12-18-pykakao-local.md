---
title: "PyKakao - Python으로 카카오 로컬 API 사용하기"
categories: python
tags: PyKakao
header:
  overlay_image: /assets/img/logo/PyKakao.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 카카오 API를 사용하기 위한 오픈소스 파이썬 라이브러리

<div align="center">
  <img src="https://github.com/WooilJeong/PyKakao/blob/main/assets/img/logo.png?raw=true" width="300"/>
</div>

<br>

## PyKakao

[**PyKakao**](https://github.com/WooilJeong/PyKakao) 라이브러리를 사용하면 [Kakao Developers](https://developers.kakao.com/)에서 제공하는 여러 종류의 카카오 API를 파이썬으로 쉽게 사용할 수 있습니다. 예를 들어, [Daum 검색 API](https://developers.kakao.com/docs/latest/ko/daum-search/dev-guide)를 이용해서 웹에서 정보를 검색할 수 있고, [메시지 API](https://developers.kakao.com/docs/latest/ko/message/rest-api)를 사용해서 카카오톡 메시지를 전송할 수 있습니다. 또한, [로컬 API](https://developers.kakao.com/docs/latest/ko/local/dev-guide)를 통해 주변 정보를 조회할 수 있고, [KoGPT API](https://developers.kakao.com/docs/latest/ko/kogpt/rest-api)와 [Karlo API](https://developers.kakao.com/docs/latest/ko/karlo/rest-api)를 이용해 자연어 처리를 하거나 생성형 인공지능으로 새로운 이미지를 만들어 볼 수도 있습니다.

- [PyKakao 깃허브 저장소](https://github.com/WooilJeong/PyKakao)
- [PyKakao 사용자 모임 (카카오톡)](https://open.kakao.com/o/gh1N1kJe)
- [PyKakao - Python으로 다음 검색 API 사용하기](https://wooiljeong.github.io/python/pykakao-daum/)
- [PyKakao - Python으로 카카오 KoGPT API 사용하기](https://wooiljeong.github.io/python/pykakao-kogpt/)
- [PyKakao - Python으로 카카오 Karlo API 사용하기](https://wooiljeong.github.io/python/pykakao-karlo/)
- [PyKakao - Python으로 카카오 로컬 API 사용하기](https://wooiljeong.github.io/python/pykakao-local/)
- [PyKakao - Python으로 카카오톡 메시지 API 사용하기](https://wooiljeong.github.io/python/pykakao-message/)

<br>

## PyKakao 설치하기

1. 운영체제(OS)에 따라 아래 중 하나를 선택합니다.

- Windows: CMD(명령 프롬프트) 실행
- Mac: Terminal(터미널) 실행

2. 아래 Shell 명령어를 입력 후 실행합니다.

```bash
pip install PyKakao --upgrade
```

<br>

## REST API 키 발급받기

PyKakao 라이브러리로 카카오 API를 사용하기 위해서는 [Kakao Developers](https://developers.kakao.com/)에 가입해야 합니다. 가입 후 로그인한 상태에서 상단 메뉴의 [내 애플리케이션](https://developers.kakao.com/console/app)을 선택합니다. '애플리케이션 추가하기'를 눌러 팝업창이 뜨면 '앱 이름', '사업자명'을 입력하고, 운영정책에 동의 후 '저장'을 선택합니다. 추가한 애플리케이션을 선택하면 '앱 키' 아래에 '**REST API 키**'가 생성된 것을 확인할 수 있습니다.

<br>

## 카카오 로컬 API 사용하기

로컬(local) API는 키워드로 특정 장소 정보를 조회하거나, 좌표를 주소 또는 행정구역으로 변환하는 등 장소에 대한 정보를 제공합니다. 특정 카테고리로 장소를 검색하는 등 폭넓은 활용이 가능하며, 지번 주소와 도로명 주소 체계를 모두 지원합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/local/common#intro)

### 카카오 로컬 API 클래스 임포트하기

PyKakao 라이브러리의 `Local` 클래스를 임포트합니다. `Local`의 인자로 위에서 발급받은 '**REST API 키**'를 입력하여 `api` 인스턴스를 생성합니다.

```python
from PyKakao import Local
api = Local(service_key = "REST API 키")
```

<br>

### 주소 검색하기

주소를 지도 위에 정확하게 표시하기 위해 해당 주소의 좌표 정보를 제공하는 API입니다. 주소에 해당하는 지번 주소, 도로명 주소, 좌표, 우편번호, 빌딩명 등의 다양한 정보를 함께 제공합니다. 이 API는 지번 주소, 도로명 주소 모두 지원합니다.

`api.search_address`의 인자로 `검색을 원하는 질의어	`를 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/local/dev-guide#address-coord)

```python
df =  api.search_address("백현동", dataframe=True)
```

<br>

### 좌표로 행정구역정보 받기

다양한 좌표계에 대한 좌표값을 받아 해당 좌표에 부합하는 행정동, 법정동을 얻는 API입니다. 대략적인 지역 정보를 제공하여 해당 위치에 맞는 다른 서비스(맛집, 날씨 등등)를 연계하는데 활용 가능합니다.

`api.geo_coord2regioncode`의 인자로 `X 좌표값, 경위도인 경우 경도(longitude)`, `Y 좌표값, 경위도인 경우 위도(latitude)`을 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/local/dev-guide#coord-to-district)

```python
df =  api.geo_coord2regioncode(127.110871319215, 37.3885490672089, dataframe=True)
```

<br>

### 좌표로 주소 변환하기

좌표 정보의 지번 주소와 도로명 주소 정보를 반환하는 API입니다. 도로명 주소는 좌표에 따라 반환되지 않을 수 있습니다.

`api.geo_coord2address`의 인자로 `X 좌표값, 경위도인 경우 경도(longitude)`, `Y 좌표값, 경위도인 경우 위도(latitude)`을 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/local/dev-guide#coord-to-address)

```python
df =  api.geo_coord2address(127.110871319215, 37.3885490672089, dataframe=True)
```

<br>

### 좌표계 변환하기

x, y 값과 입력 및 출력 좌표계를 지정해 변환된 좌표 값을 구해, 서로 다른 좌표계간 데이터 호환이 가능하도록 합니다.

`api.geo_coord2address`의 인자로 `X 좌표값, 경위도인 경우 경도(longitude)`, `Y 좌표값, 경위도인 경우 위도(latitude)`, `변환할 좌표계`을 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/local/dev-guide#trans-coord)

```python
df =  api.geo_transcoord(127.110871319215, 37.3885490672089, "WCONGNAMUL", dataframe=True)
```

### 키워드로 장소 검색하기

질의어에 매칭된 장소 검색 결과를 지정된 정렬 기준에 따라 제공합니다. 현재 위치 좌표, 반경 제한, 정렬 옵션, 페이징 등의 기능을 통해 원하는 결과를 요청 할 수 있습니다.

`api.search_keyword`의 인자로 `검색을 원하는 질의어`를 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/local/dev-guide#search-by-keyword)

```python
df =  api.search_keyword("판교역", dataframe=True)
```

<br>

### 카테고리로 장소 검색하기

미리 정의된 카테고리 코드에 해당하는 장소 검색 결과를 지정된 정렬 기준에 따라 제공합니다.

`api.search_keyword`의 인자로 [카테고리 코드](https://developers.kakao.com/docs/latest/ko/local/dev-guide#search-by-category-request-category-group-code)를 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/local/dev-guide#search-by-category)

```python
df =  api.search_category("MT1", x=127.110871319215, y=37.3885490672089, radius=500, dataframe=True)
```

<br>

## 참고

- [PyKakao 깃허브 저장소](https://github.com/WooilJeong/PyKakao)
- [PyKakao 사용자 모임 (카카오톡)](https://open.kakao.com/o/gh1N1kJe)
- [PyKakao - Python으로 다음 검색 API 사용하기](https://wooiljeong.github.io/python/pykakao-daum/)
- [PyKakao - Python으로 카카오 KoGPT API 사용하기](https://wooiljeong.github.io/python/pykakao-kogpt/)
- [PyKakao - Python으로 카카오 Karlo API 사용하기](https://wooiljeong.github.io/python/pykakao-karlo/)
- [PyKakao - Python으로 카카오 로컬 API 사용하기](https://wooiljeong.github.io/python/pykakao-local/)
- [PyKakao - Python으로 카카오톡 메시지 API 사용하기](https://wooiljeong.github.io/python/pykakao-message/)