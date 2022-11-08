---
title: "ScraperAPI - 파이썬 웹 스크래핑을 위한 프록시 API"
categories: python
tags: proxy
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---
# proxy api를 이용한 스크래핑 차단 우회하기

![PNG](/assets/img/post_img/2022-07-12-scraperapi/scraperapi_logo.png){: .align-center}


[ScraperAPI](https://www.scraperapi.com/)는 스크래핑하고자 하는 URL을 보내기만 하면 HTML 응답을 반환해주는 Proxy API이다. 특정 사이트에 지속적인 http 요청을 보낼 경우 요청 ip의 서버 요청을 차단하거나 Captcha 보안 문자를 보여주거나 하는 등의 조치로 인해 스크래핑이 정상적으로 동작하지 않을 때가 있다. 이 때, ScraperAPI를 이용하면 이러한 문제를 피할 수 있다.

아래 네 가지 방법 중 하나로 ScraperAPI에 GET 요청을 보낼 수 있다. API 엔드포인트를 이용해서 프록시 API를 사용해보자.

- 비동기 스크래퍼 서비스: http://async.scraperapi.com
- API 엔드포인트: http://api.scraperapi.com?
- [Python SDK](https://pypi.org/project/scraperapi-sdk/)
- 프록시 포트: http://scraperapi:APIKEY@proxy-server.scraperapi.com:8001

<br>

## API Key & 인증

[ScraperAPI](https://www.scraperapi.com/)에 가입 후 로그인을 하면 자동으로 API Key가 발급된다. API Key가 발급되면 바로 프록시 API를 무료로 이용할 수 있다. 단, 무료 플랜의 경우 매월 5,000건의 프록시 API 요청이 가능하고, 최대 5개의 동시 요청이 허용된다. 프록시 API 요청 건수와 동시 요청 수를 늘리려면 [유료 플랜](https://www.scraperapi.com/pricing/)을 이용하면 된다. 

![PNG](/assets/img/post_img/2022-07-12-scraperapi/scraperapi_pricing.png){: .align-center}

<br>

## 프록시 API를 사용하지 않은 요청 IP 확인

'http://jsonip.com' 에 GET 요청을 하면 http 요청 ip를 반환해준다. 프록시 API를 사용하지 않고 http 요청 ip를 반환해주는 API를 여러번 호출하면 당연히 매번 같은 ip가 반환되는 것을 알 수 있다. 이러한 이유로 스크래핑 시 동일 ip에 대해 스크래핑 로직이 막히는 경우가 생기기도 한다. 


```python
import requests

def func():
    return requests.get("http://jsonip.com").json()['ip']

task = [func() for i in range(3)]
task
```




    ['32.82.xxx.xxx', '32.82.xxx.xxx', '32.82.xxx.xxx']


<br>

## 프록시 API를 사용한 요청 IP 확인

그럼 매 http 요청 시 마다 요청 ip를 바꿔서 스크래핑 하고자 하는 서버에서 스크래핑 로직을 막지 못하도록 우회할 수 있을까? ScraperAPI를 이용하면 가능하다. http 요청 ip를 반환하는 API를 ScraperAPI를 통해 여러번 호출하면 서로 다른 ip가 반환되는 것을 알 수 있다.


```python
import requests

def func():
    payload = {
        "api_key": "APIKEY",
        "url": "http://jsonip.com",
    }
    return requests.get("http://api.scraperapi.com", params=payload).json()['ip']

task = [func() for i in range(3)]
task
```




    ['194.53.140.21', '107.172.1.55', '107.165.192.52']


