---
title: "PyKakao - Python으로 다음 검색 API 사용하기"
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

## Daum 검색 API 사용하기

Daum 검색 API는 포털 사이트 Daum에서 방대한 웹 문서, 동영상, 이미지, 블로그, 책, 카페를 검색하는 기능을 제공합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/daum-search/common#intro)


### 카카오 Daum 검색 API 클래스 임포트하기

PyKakao 라이브러리의 `DaumSearch` 클래스를 임포트합니다. `DaumSearch`의 인자로 위에서 발급받은 '**REST API 키**'를 입력하여 `api` 인스턴스를 생성합니다.

```python
from PyKakao import DaumSearch
api = DaumSearch(service_key = "REST API 키")
```


<br>

### 웹문서 검색

다음 검색 서비스에서 질의어로 웹 문서를 검색합니다. 원하는 검색어와 함께 결과 형식 파라미터를 선택적으로 추가할 수 있습니다.

`api.search_web`의 인자로 `검색을 원하는 질의어	`를 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/daum-search/dev-guide#search-doc)

```python
df = api.search_web("파이썬", dataframe=True)
```

<br>


### 동영상 검색

카카오 TV, 유투브(Youtube) 등 서비스에서 질의어로 동영상을 검색합니다. 원하는 검색어와 함께 결과 형식 파라미터를 선택적으로 추가할 수 있습니다.

`api.search_vclip`의 인자로 `검색을 원하는 질의어	`를 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/daum-search/dev-guide#search-video)

```python
df = api.search_vclip("파이썬", dataframe=True)
```

<br>

### 이미지 검색

다음 검색 서비스에서 질의어로 이미지를 검색합니다. REST API 키를 헤더에 담아 GET으로 요청합니다. 원하는 검색어와 함께 결과 형식 파라미터를 선택적으로 추가할 수 있습니다.

`api.search_image`의 인자로 `검색을 원하는 질의어	`를 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/daum-search/dev-guide#search-image)

```python
df = api.search_image("파이썬", dataframe=True)
```

<br>

### 블로그 검색

다음 블로그 서비스에서 질의어로 게시물을 검색합니다. 원하는 검색어와 함께 결과 형식 파라미터를 선택적으로 추가할 수 있습니다. 

`api.search_blog`의 인자로 `검색을 원하는 질의어	`를 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/daum-search/dev-guide#search-blog)

```python
df = api.search_blog("파이썬", dataframe=True)
```
<br>

### 책 검색

다음 책 서비스에서 질의어로 도서 정보를 검색합니다. 원하는 검색어와 함께 결과 형식 파라미터를 선택적으로 추가할 수 있습니다.

`api.search_book`의 인자로 `검색을 원하는 질의어	`를 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/daum-search/dev-guide#search-book)

```python
df = api.search_book("파이썬", dataframe=True)
```
<br>

### 카페 검색

다음 카페 서비스에서 질의어로 게시물을 검색합니다. 원하는 검색어와 함께 결과 형식 파라미터를 선택적으로 추가할 수 있습니다.

`api.search_cafe`의 인자로 `검색을 원하는 질의어	`를 입력합니다. `dataframe` 인자의 값을 `True`로 입력하면 결과를 판다스 데이터프레임 형식으로 반환하고, 이 값을 설정하지 않거나 `False`로 입력하면 결과를 딕셔너리 형식으로 반환합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/daum-search/dev-guide#search-cafe)

```python
df = api.search_cafe("파이썬", dataframe=True)
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