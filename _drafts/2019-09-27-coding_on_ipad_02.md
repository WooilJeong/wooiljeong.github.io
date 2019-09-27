---
title: "아이패드로 주피터 노트북 사용하기 02"
date: 2019-09-27 00:00:00 -0000
categories: Tutorial
tags: jupyter notebook
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## 카페에서 아이패드로 주피터 노트북 사용하기

'[아이패드로 주피터 노트북 사용하기 01](https://wooiljeong.github.io/tutorial/coding_on_ipad_01/)'에서는 서버PC에 주피터 서버를 구축하고 내부 ip주소와 포트를 통해 아이패드로 주피터 노트북을 실행하는 방법에 대해 알아보았습니다. 이번에는 같은 공유기 망 뿐만 아니라 카페와 같이 외부에서도 서버에 접속할 수 있는 방법을 알려드리겠습니다. 여기에서는 IPTIME 공유기를 사용하는 서버를 가정하겠습니다.


<br><br>

## IPTIME 공유기 포트 포워딩(Port Forwarding)

포트 포워딩(Port Forwarding)은 쉽게 말하면 외부에서 공유기 내부의 컴퓨터에 접속하기 위한 설정을 해주는 것을 의미합니다. 즉, 아이패드로 카페 와이파이 같은 외부망을 이용할때에도 원격지에 있는 주피터 서버에 접속할 수 있도록 주피터 서버의 포트 번호를 개방해주는 것입니다.

<br>

포트 포워딩을 위해서는 우선 주피터 서버가 설치된 PC가 IPTIME 공유기에 반드시 연결되어 있어야 합니다. 우선 서버PC의 내부망 IP 주소를 확인해봅시다. 명령 프롬프트(```CTRL``` + ```R``` -  ```cmd```)를 실행하고 다음 명령어를 입력합니다.

```
> ipconfig
```

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_01.PNG)

제 PC의 경우 ```IPv4주소```는 192.168.0.3 이고, ```기본 게이트웨이```는 192.168.0.1 임을 확인했습니다.

<br>

IPTIME의 기본 설정 페이지 접속 주소는 위 기본 게이트웨이 주소와 같습니다. 크롬과 같은 웹 브라우저를 실행 후 해당 주소(http://192.168.0.1)를 입력하여 접속하면 설정 페이지가 출력됩니다.

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_02.PNG)



<br><br>
