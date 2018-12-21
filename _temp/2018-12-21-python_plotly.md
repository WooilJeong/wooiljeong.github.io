---
title: "플로틀리 튜토리얼(Plotly Tutorial)-파이썬 시각화를 위한 오픈소스 라이브러리"
date: 2018-12-21 20:00:00 -0400
categories: Tutorial
tags: Python Plotly
---
## 플로틀리(Plotly)

플로틀리(Plotly)는 캐나다 퀘벡 몬트리올에 본사를 두고있는 컴퓨팅 기술 회사로 온라인 데이터 분석 및 시각화 툴을 개발하고 있다. 플로틀리는 Python, R, MATLAB, Perl, Julia, Arduino 및 REST 용 과학 그래프 라이브러리 뿐만 아니라 개인 및 협업을 위한 온라인 그래프, 분석 및 통계 툴도 제공한다. 본 튜토리얼에서는 파이썬(Python) 환경에서 온라인 플로팅을 위한 [플로틀리 공식 사이트](https://plot.ly/python/getting-started/#initialization-for-online-plotting)의 설명을 한국어로 번역하고, 이를 실습해 볼 것이다.


## 설치

플로틀리 파이썬 패키지를 설치하려면, 다음과 같이 터미널에서 패키지 매니저 ```pip``` 명령어를 사용해야 한다.

```bash
pip install plotly
```

플로틀리 파이썬 패키지는 자주 업데이트 되기 때문에 다음 명령어를 사용해 최신 버전으로 업데이트해주도록 한다.

```bash
pip install plotly --upgrade
```


## 온라인 플로팅을 위한 초기화

플로틀리는 그래프 호스팅을 위한 웹 서비스를 제공한다. 이를 시작하기 위해서 먼저 무료로 계정을 만들어야 한다. 그래프는 온라인 플로틀리 계정에 저장되며, 개인정보를 설정할 수 있다. 공개 호스팅은 무료이며, 비공개 호스팅을 위해서는 유료 요금제를 확인해야한다.

플로틀리 패키지 설치가 완료되면, 파이썬에서 이를 사용해 볼 수 있다. 이번 튜토리얼에서는 주피터 노트북에서 실습해 볼 것이다. 주피터 노트북 상에서 다음과 같이 플로틀리 패키지를 임포트하고, 개인 계정 정보를 입력해야한다.

```python
import plotly
plotly.tools.set_credentials_file(username='DemoAccount', api_key='lr1c37zw81')
```

위 코드를 그대로 복사하여 실행하면 안되고, ```DemoAccount```와 ```lr1c37zw81```를 플로틀리 ```계정 이름```과 [API Key](https://plot.ly/settings/api#/)로 변경해주어야 한다.

초기화 과정을 진행하고 나면 ```.plotly/.credentials``` 파일이 홈 디렉토리에 생긴다.**(ex."C:\Users\username\.plotly\.credentials")** 이 파일의 내용은 다음과 같은 형태로 되어 있다.

```json
{
    "username": "DemoAccount",
    "api_key": "lr1c37zw81",
    "proxy_username": "",
    "proxy_password": "",
    "stream_ids": []
}
```


## 온라인 플롯 프라이버시

플롯은 ```공개```, ```비공개```, 또는 ```비밀``` 세 가지 유형의 프라이버시 타입을 설정할 수 있다.

* **공개** : 누구나 그래프를 볼 수 있다. 프로필에 나타나고, 검색 엔진에 노출될 수 있다. 그래프를 보기 위해 꼭 플로틀리에 로그인 할 필요가 없다.

* **비공개** : 본인만 그래프를 볼 수 있다. 플로틀리 피드, 프로필 또는 검색 엔진에 노출되지 않는다. 플로틀리 계정이 있는 사용자들과 개인적으로 그래프를 공유할 수 있다. 단, 그래프를 보기 위해서는 로그인을 해야한다.

* **비밀** : 비밀 링크를 가진 사람이라면 누구나 그래프를 볼 수 있다. 플로틀리 피드, 프로필 또는 검색 엔진에는 노출되지 않는다. 웹 페이지 또는 iPython 노트북(ex.주피터 노트북)에 그래프가 삽입 된 경우 페이지를 보고있는 사용자는 그래프를 볼 수 있다. 이 플롯을 보려면 로그인 할 필요가 없다.

## Special Instructions for Chart Studio Enterprise Users




## 플로팅 온라인 시작하기




## 오프라인 플로팅을 위한 초기 작업




## 추가 예제
