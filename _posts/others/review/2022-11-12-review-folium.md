---
title: "Python Folium 첫 오픈소스 기여 후기"
categories: review
tags: folium
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2
---

# Python Folium 첫 오픈소스 기여 후기

## Folium의 Contributor가 되다

leaflet.js 기반으로 지도를 시각화해주는 파이썬 라이브러리인 **Folium** 프로젝트에 오픈소스를 기여해봤다. Folium은 오래 전부터 줄곧 공간 데이터 시각화 작업을 할 때 즐겨 사용해왔던 라이브러리이다. 대체로 만족하며 사용해왔지만, 일부 아쉬운 점도 있었다. folium GitHub 저장소에서 소스코드를 살펴보니 간단하게 해결할 수 있을 것으로 판단했고 PR을 날려봤다. 몇 번의 리뷰 과정을 거쳐 최근 main 브랜치에 merge되었다. 결과적으로 내가 처음 작성한 소스코드는 모두 네덜란드의 **Frank([@Conengmo](https://github.com/Conengmo))**님의 코드로 대체되었지만, 어찌 되었든 문제는 해결되었다. 문제해결 과정에서 더 괜찮은 방식을 고민했던 경험을 복기해보고자 이 글을 적는다.


![PNG](/assets/img/post_img/2022-11-12-review-folium/0.png){: .align-center}


<br>

## 지도를 이미지로 저장하고 싶은데 잘 안 된다

주피터 노트북에서 folium을 이용해 지도를 출력하는 건 문제없이 잘 되지만, 출력된 결과를 png 파일과 같은 이미지로 저장하고 싶었다. 

![PNG](/assets/img/post_img/2022-11-12-review-folium/1.png){: .align-center}

`folium.folium.Map._to_png` 메서드를 이용하면 이미지 객체로 불러올 수 있다는 것을 알게 되었고, 윈도우 환경에서 바로 적용해보았다. 

![PNG](/assets/img/post_img/2022-11-12-review-folium/2.png){: .align-center}

그러나 아래와 같은 오류가 발생했다.

![PNG](/assets/img/post_img/2022-11-12-review-folium/3.png){: .align-center}

<br>

## 문제의 소스코드를 살펴봤다

보아하니, folium은 지도를 html 형식으로 만들기 때문에 이를 이미지로 변환하기 위해서는 Selenium을 이용하는 모양이었다. 즉, 브라우저로 지도 html을 열어 스크린샷을 찍는 방식을 사용하는 것 같았다. 문제는 이 기능은 웹 브라우저로 firefox를 사용할 경우에만 사용할 수 있다는 것이었다. Chrome으로 설정을 바꾸고 싶어도 방법이 없었다. 

![PNG](/assets/img/post_img/2022-11-12-review-folium/4.png){: .align-center}

<br>

## 크롬 드라이버를 사용할 수 있게 바꿔보았다

직접 기능을 수정하기로 마음 먹고 호기롭게 folium 프로젝트를 클론하여 소스코드를 일부 수정했다. `folium.folium.Map._to_png` 메서드에 `chrome` 항목을 추가하여 이 값이 True이면 기본값인 firefox 브라우저 대신 chrome 브라우저를 selenium의 웹 드라이버로 설정할 수 있도록 변경했다. 보통 chrome 브라우저를 웹 드라이버로 설정할 때, OS에 설치된 chrome의 버전과 일치하는 드라이버 파일을 다운받아 해당 파일의 경로를 selenium에서 사용하는데, 이 과정이 생각보다 꽤 번거로운 작업이다. 따라서 자동으로 설치된 chrome의 버전과 일치하는 웹드라이버를 다운로드받아 selenium을 이용할 수 있도록 도와주는 `webdriver_manager` 라는 파이썬 라이브러리를 활용하는 로직을 소스코드에 추가시켰다. 수정한 로직을 적용해 아래 코드를 실행해보니 정상적으로 이미지를 캡처할 수 있었다. 

```python
import io
import folium
from PIL import Image

m = folium.Map(location = [37.53, 127.054],
               zoom_start = 14)

img_data = m._to_png(delay=5,
                     chrome=True)
img = Image.open(io.BytesIO(img_data))
```

<br>

## (피드백1) 직관적이지 않은 것 같아

위에서 수정한 내용을 바탕으로 folium 프로젝트에 Pull Request를 날렸다. 얼마 뒤 어느 리뷰어에게 피드백을 받았다. folium 지도 이미지 캡처 기능에 문제가 있다는 점에는 동의하지만, 내가 작성한 소스코드에 대해서는 두 가지 사항이 마음에 들지 않는다는 것이었다. 

- chrome의 값으로 True, False를 주는 것이 직관적이지 않은 것 같다.
- webdriver_manager를 사용하는 건 좋은 생각이 아닌 것 같다.

생각해보니 chrome 브라우저를 사용하지 않겠다는 것이 firefox 브라우저를 사용하겠다는 것을 의미한다는 것이 부자연스럽긴 했다. 사실 selenium에서는 chrome과 firefox 말고도 edge, safari 같은 다른 브라우저들도 일부 지원하고 있기 때문에 사용자 입장에서 혼동할 여지가 있었다. 또, webdriver_manager의 경우 자동으로 웹 드라이버를 설치해준다는 점은 매우 편리하긴 하지만 설치 과정이 캡처 기능과 함께 실행된다는 점이 부담스러울 수도 있겠다는 생각이 들었다. 

<br>

## 웹 드라이버를 선택할 수 있도록 바꿔 보았다

리뷰어의 피드백을 받은 후 사용자가 사용하는 웹 드라이버를 선택할 수 있도록 `webdriver_type` 항목과 웹드라이버의 경로를 의미하는 `webdriver_path` 항목을 추가했다.  다만, 브라우저 마다 웹드라이버를 설정하는 방식이 조금씩 달라 chrome과 firefox 이외의 브라우저 선택 시 실행되는 로직은 다른 기여자의 목으로 남겨두기로 하였다. 변경한 방식대로 이미지를 캡처하는 예시는 다음과 같다.

```python
import io
import folium
from PIL import Image

m = folium.Map(location = [37.53, 127.054],
               zoom_start = 14)

img_data = m._to_png(delay=5,
                     webdriver_type='chrome',
                     webdriver_path='./chromedriver.exe')
img = Image.open(io.BytesIO(img_data))
```

<br>

## (피드백2) 드라이버 객체를 입력받도록 변경했어

코드 부담은 줄이고 모든 브라우저를 사용할 수 있게 바꿨어

얼마간 시간이 지난뒤 다시 리뷰어에게 피드백을 받았다. 아예 `driver` 항목 하나만 추가하여, 이 값이 None이면 기존 로직대로 firefox 브라우저 기반의 웹드라이버를 구동시키도록 수정하였다. firefox가 아닌 다른 브라우저 기반의 웹드라이버를 사용하기 위해서는 `driver` 항목에 메서드 밖에서 생성한 웹드라이버 객체 자체를 입력받도록 변경한 것이다. 이렇게 하면 사용자가 자신이 사용하는 브라우저에 맞는 selenium 웹드라이버 객체를 만들고 해당 객체만 넘겨주기만 하면 된다. 결과적으로 생각하면 정말 간단하지만 처음부터 이런 접근을 하지 못했던 것이 오히려 놀라웠다. 코드의 부담은 줄이고 selenium이 지원하는 모든 브라우저를 사용할 수 있게 되었다.

<br>

## 테스트를 해보자

현재 변경된 소스코드는 folium의 main 브랜치에 merge되었지만, 정식 버전이 배포되진 않은 상태이므로 GitHub 저장소에 공개된 개발 버전의 folium을 설치했다.

```bash
pip install git+https://github.com/python-visualization/folium.git
```

Windows 10에서 아래 버전의 Chrome을 사용하고 있으므로 그에 맞는 웹드라이버를 `C:/chromedriver/chromedriver.exe` 와 같이 다운로드 받았다.

- 버전 107.0.5304.107(공식 빌드) (64비트)

```bash
import io
import folium
from PIL import Image

# folium 지도 객체 생성
m = folium.Map(location = [37.53, 127.054],
               zoom_start = 14)

# 크롬 웹드라이버 객체 생성
from selenium import webdriver
options = webdriver.ChromeOptions()
options.add_argument('--headless')
driver_path = "C:/chromedriver/chromedriver.exe"
driver = webdriver.Chrome(driver_path, options=options)

# 이미지 캡처
img_data = m._to_png(5, driver=driver)
img = Image.open(io.BytesIO(img_data))
img
```

![PNG](/assets/img/post_img/2022-11-12-review-folium/5.png){: .align-center}

<br>

정답이 없는 상황에서 더 나은 방법을 찾아가는 과정이 굉장히 흥미로웠다. 무엇보다도 누군가가 내가 생각한 접근 방법에 대해 여과없이 피드백을 해주고 더 나은 방향을 함께 고민해준다는 사실이 마음에 들었다. 새로운 취미가 생긴 것 같아 기분이 좋다. 끝.

<br>

## 참고

- [folium GitHub](https://github.com/python-visualization/folium)
- [folium PR](https://github.com/python-visualization/folium/pull/1620)