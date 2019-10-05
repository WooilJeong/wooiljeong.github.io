---
layout: post
title: "플로틀리(Plotly) - 파이썬 데이터 시각화"
categories: Python
tags: Plotly
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---
> # 인터렉티브 데이터 시각화 툴 플로틀리(Plotly)

![PNG](/assets/img/post_img/2019-09-30-python_plotly/img_plotly_logo.PNG){: .align-center height="300px" width="300px"}


플로틀리(Plotly)는 캐나다 퀘벡 몬트리올에 본사를 두고있는 컴퓨팅 기술 회사로 온라인 데이터 분석 및 시각화 툴을 개발하고 있습니다. 플로틀리는 Python, R, MATLAB, Perl, Julia, Arduino 및 REST 용 과학 그래프 라이브러리 뿐만 아니라 개인 및 협업을 위한 온라인 그래프, 분석 및 통계 툴도 제공하고 있다고 합니다. 이번 포스팅에서는 [플로틀리 공식 사이트](https://plot.ly/python/getting-started/#initialization-for-online-plotting)의 설명을 토대로 파이썬(Python) 환경에서의 온라인 플로팅 방법에 대해 알아보도록 하겠습니다.

<br>

## 설치

다음과 같이 터미널에서 ```pip``` 명령어를 통해 플로틀리 패키지를 설치합니다.
(아나콘다와 같은 파이썬 환경이 설치되어 있는 것을 가정하고 설명을 진행하겠습니다.)

```bash
pip install plotly
```

플로틀리 파이썬 패키지는 자주 업데이트 되기 때문에 다음 명령어를 사용해 최신 버전으로 업데이트해주도록 합니다.

```bash
pip install plotly --upgrade
```

<br>

## 온라인 플로팅을 위한 초기화

플로틀리는 그래프 호스팅을 위한 웹 서비스를 제공합니다. 이를 시작하기 위해서 먼저 무료로 계정을 만들어야 합니다. 그래프는 온라인 플로틀리 계정에 저장되며, 개인정보를 설정할 수 있습니다. 공개 호스팅은 무료이며, 비공개 호스팅을 위해서는 유료 요금제를 확인해야합니다.

<br>

플로틀리 패키지 설치가 완료되면, 파이썬에서 바로 사용 가능합니다. 주피터 노트북에서 실습해보도록 하겠습니다. 주피터 노트북 상에서 다음과 같이 플로틀리 패키지를 임포트하고, 개인 계정 정보를 입력해야합니다.

```python
import plotly
plotly.tools.set_credentials_file(username='DemoAccount', api_key='lr1c37zw81')
```

위 코드를 그대로 복사하여 실행하면 안되고, ```DemoAccount```와 ```lr1c37zw81```를 플로틀리 ```계정 이름```과 [API Key](https://plot.ly/settings/api#/)로 변경해주어야 합니다.

초기화 과정을 진행하고 나면 ```.plotly/.credentials``` 파일이 홈 디렉토리에 생긴다.**(ex."C:\Users\username\.plotly\.credentials")** 이 파일의 내용은 다음과 같은 ```json``` 형식으로 되어 있습니다.

```json
{
    "username": "DemoAccount",
    "api_key": "lr1c37zw81",
    "proxy_username": "",
    "proxy_password": "",
    "stream_ids": []
}
```

<br>

## 온라인 플롯 프라이버시

플롯은 ```공개```, ```비공개```, 또는 ```비밀``` 세 가지 유형의 프라이버시 타입을 설정할 수 있습니다.

* **공개** : 누구나 그래프를 볼 수 있습니다. 프로필에 나타나고, 검색 엔진에 노출될 수 있습니다. 그래프를 보기 위해 꼭 플로틀리에 로그인 할 필요가 없습니다.

* **비공개** : 본인만 그래프를 볼 수 있습니다. 플로틀리 피드, 프로필 또는 검색 엔진에 노출되지 않습니다. 플로틀리 계정이 있는 사용자들과 개인적으로 그래프를 공유할 수 있습니다. 단, 그래프를 보기 위해서는 로그인을 해야합니다.

* **비밀** : 비밀 링크를 가진 사람이라면 누구나 그래프를 볼 수 있습니다. 플로틀리 피드, 프로필 또는 검색 엔진에는 노출되지 않습니다. 웹 페이지 또는 iPython 노트북(ex.주피터 노트북)에 그래프가 삽입 된 경우 페이지를 보고있는 사용자는 그래프를 볼 수 있습니다. 이 플롯을 보려면 로그인 할 필요가 없습니다.

기본적으로 모든 플롯은 ```공개```로 설정됩니다. 무료 계정을 가진 사용자는 ```비공개``` 플롯 한 개를 가질 수 있습니다. ```비공개``` 플롯을 저장해야 하는 경우 [프로 계정으로 업그레이드](https://plot.ly/products/cloud/)를 해야합니다. [개인 또는 전문 사용자](https://plot.ly/settings/subscription/?modal=true&utm_source=api-docs&utm_medium=support-oss#/)이고 플롯의 기본 설정을 ```비공개```로 설정하려는 경우 플로틀리 구성을 편집할 수 있습니다.

```python
import plotly
plotly.tools.set_config_file(world_readable=False,
                             sharing='private')
```

프라이버시 설정에 대한 더 많은 예제는 [Python privacy documentation](https://plot.ly/python/privacy/)을 참고하시면 됩니다.

<br>

## 플로팅 온라인 시작하기

온라인으로 플로팅하면 플롯과 데이터가 클라우드 계정에 저장됩니다. 온라인 플로팅에는 ```py.plot()```과 ```py.iplot()``` 두 가지 방법이 있습니다. 두 옵션 모두 플롯에 대한 고유한 URL을 만들어 플로틀리 계정에 저장합니다.

- ```py.plot()```을 사용해 고유한 URL을 반환하고 선택적으로 url을 엽니다.
- ```py.iplot()```을 사용해 주피터 노트북에 플롯을 표시합니다.

플로틀리 파이썬 라이브러리를 사용하여 처음으로 호스팅 된 플로틀리 그래프를 생성하려면 다음 예제 중 하나를 복사해서 붙여 넣으면 됩니다.

```python
import plotly.plotly as py
import plotly.graph_objs as go

trace0 = go.Scatter(
    x=[1, 2, 3, 4],
    y=[10, 15, 13, 17]
)
trace1 = go.Scatter(
    x=[1, 2, 3, 4],
    y=[16, 5, 11, 9]
)
data = [trace0, trace1]

py.plot(data, filename = 'basic-line', auto_open=True)

## 그래프를 새 창에서 출력
```


```python
import plotly.plotly as py
help(py.plot)
```

```python
import plotly.plotly as py
import plotly.graph_objs as go

trace0 = go.Scatter(
    x=[1, 2, 3, 4],
    y=[10, 15, 13, 17]
)
trace1 = go.Scatter(
    x=[1, 2, 3, 4],
    y=[16, 5, 11, 9]
)
data = [trace0, trace1]

py.iplot(data, filename = 'basic-line')

# 그래프를 주피터 노트북 상에 출력
```

[iPython 노트북 문서](https://plot.ly/ipython-notebooks/)에서 더 많은 예제를 확인할 수 있고, 자세한 정보는 ```py.iplot()``` docstring을 확인하면 됩니다.

```python
import plotly.plotly as py
help(py.iplot)
```

* 그래프 출력 예시

<iframe width="600" height="400" frameborder="0" scrolling="no" src="//plot.ly/~WooilJeong/0.embed"></iframe>{: .align-center}
<br>

**matplotlib** 구문을 사용하여 플롯 그래프를 만들 수도 있습니다. [matplotlib 문서](https://plot.ly/matplotlib/)에서 더 자세한 사항을 확인 할 수 있습니다.

<br>

## 오프라인 플로팅을 위한 초기 작업

플로틀리 오프라인을 사용하면 그래프를 오프라인으로 생성하고 로컬로 저장할 수 있습니다. 오프라인으로 플로팅하는 방법에는 ```plotly.offline.plot()```과 ```plotly.offline.iplot()``` 두 가지 방법이 있습니다.

- ```plotly.offline.plot()```을 사용해 로컬에 저장되고 웹 브라우저에서 열리는 HTML을 만들고 독립 실행 형태로 만듭니다.
- 주피터 노트북에서 오프라인으로 작업 할 때는 ```plot.offline.iplot()```을 사용하여 노트북에 플롯을 표시합니다.

오프라인 플로팅에는 플로틀리 버전 1.9.4 이상이 필요합니다.

```python
import plotly
plotly.__version__
```

다음 예제 중 하나를 복사하여 붙여 넣어 플로틀리 파이썬 라이브러리를 사용하여 첫 번째 오프라인 프로틀리 그래프를 만듭니다.

```python
import plotly
import plotly.graph_objs as go

plotly.offline.plot({
    "data": [go.Scatter(x=[1, 2, 3, 4], y=[4, 3, 2, 1])],
    "layout": go.Layout(title="hello world")
}, auto_open=True)
```

더 자세한 사항은 ```help()``` 명령어를 통해 다음과 같이 알아 볼 수 있습니다.

```python
import plotly
help(plotly.offline.plot)
```

주피터 노트북에서 오프라인으로 플로팅하기 위해 ```plotly.offline.iplot```을 사용할 때 각 노트북 세션의 시작 부분에 ```plotly.offline.init_notebook_mode()```를 실행하는 추가 초기화 단계가 있습니다. 아래 예시를 참조하면 됩니다.

```python
import plotly
import plotly.graph_objs as go

plotly.offline.init_notebook_mode(connected=True)

plotly.offline.iplot({
    "data": [go.Scatter(x=[1, 2, 3, 4], y=[4, 3, 2, 1])],
    "layout": go.Layout(title="hello world")
})
```

```python
import plotly
help(plotly.offline.iplot)
```

* 그래프 출력 예시

<iframe width="600" height="400" frameborder="0" scrolling="no" src="//plot.ly/~WooilJeong/2.embed"></iframe>{: .align-center}
<br>

플로팅 오프라인으로 파이썬 플로틀리를 사용하는 더 많은 예제는 [오프라인 문서](https://plot.ly/python/offline/)를 참조하시면 됩니다.

<br>

## 추가 예제

플로틀리를 파이썬으로 사용하기위한 예제와 튜토리얼은 다음 [링크](https://plot.ly/python/)를 확인하면 됩니다.
