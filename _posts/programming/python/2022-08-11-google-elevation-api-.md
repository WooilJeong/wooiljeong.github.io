---
title: "Google API - 위경도 좌표로 해발고도 구하기"
categories: python
tags: googleapi
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## Google Elevation API

- 위경도 값으로 고도 요청


```python
import requests

# 위도, 경도
lat, lon = 48.857864549450355, 2.2951934711297723

# 구글 API 키
google_api_key = "API키"

# URL
url = f"https://maps.googleapis.com/maps/api/elevation/json?locations={lat},{lon}&key={google_api_key}"

payload = {}
headers = {}
response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)
```

    {
       "results" : [
          {
             "elevation" : 33.51752090454102,
             "location" : {
                "lat" : 48.85786454945035,
                "lng" : 2.295193471129772
             },
             "resolution" : 9.543951988220215
          }
       ],
       "status" : "OK"
    }


<br>

## References

- https://developers.google.com/maps/documentation/elevation/overview
