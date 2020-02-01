---
title: "신종 코로나바이러스의 지역 별 진행 상황"
categories: ETC
tags: Analysis
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# Chinese New Coronavirus Chart

This article is a visual representation of the progress of the new coronavirus that is spreading worldwide. I used data that is constantly updated in ‘JHU CSSE’. This analysis was conducted for personal research purposes and may be in error. I sincerely hope that the virus will disappear completely in all countries as soon as possible. Thank you.

이 글은 전 세계적으로 퍼지고 있는 신종 코로나 바이러스의 진행 상황을 시각적으로 표현한 자료입니다. ‘JHU CSSE’에서 지속적으로 업데이트하고 있는 데이터를 사용하였습니다. 이 분석은 개인적인 연구 목적으로 수행되었으며, 오류가 있을 수 있습니다. 가능한 빨리 모든 국가에서 바이러스가 완전히 사라지기를 진심으로 바랍니다. 감사합니다.

- Data Source  
[Dash Board](https://gisanddata.maps.arcgis.com/apps/opsdashboard/index.html#/bda7594740fd40299423467b48e9ecf6)  
[Data Sheets](https://docs.google.com/spreadsheets/d/1yZv9w9zRKwrGTaR-YzmAqMefw4wMlaXocejdxZaTs6w/htmlview?usp=sharing&sle=true#)

- Github Repo  
[novel_coronavirus](https://github.com/WooilJeong/novel_coronavirus)

# Interactive Chart

This chart shows confirmed, deaths, and recovered of new coronaviruses. The x-axis represents the number of recovered and the y-axis represents the number of deaths. Each circle represents the area where the disease occurred in China, and the size of the circle is proportional to the size of the number of confirmed patients in that area.

이 차트는 신종 코로나바이러스 확진자, 사망자 및 회복자를 표현한 것입니다. x-축은 회복자 수, y-축은 사망자 수를 의미합니다. 각각의 원은 중국 내 질병이 발생한 지역을 의미하고, 원의 크기는 확진자 수의 크기에 비례합니다.

<iframe width="900" height="800" frameborder="0" scrolling="no" src="//plot.ly/~coronavirus/7.embed"></iframe>
