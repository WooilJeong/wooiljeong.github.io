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

<br>

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_logo.PNG){: .align-center height="300px" width="300px"}

<br>

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_intro.PNG){: .align-center}

<br><br>

## IPTIME 공유기 설정 - 포트 포워딩(Port Forwarding)

포트 포워딩(Port Forwarding)은 쉽게 말하면 외부에서 공유기 내부의 컴퓨터에 접속하기 위한 설정을 해주는 것을 의미합니다. 즉, 아이패드로 카페 와이파이 같은 외부망을 이용할때에도 원격지에 있는 주피터 서버에 접속할 수 있도록 주피터 서버의 포트 번호를 개방해주는 것입니다.

<br>

포트 포워딩을 위해서는 우선 주피터 서버가 설치된 PC가 IPTIME 공유기에 반드시 연결되어 있어야 합니다. 우선 서버PC의 내부망 IP 주소를 확인해봅시다. 명령 프롬프트(```CTRL``` + ```R``` -  ```cmd```)를 실행하고 다음 명령어를 입력합니다.

```
> ipconfig
```

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_01.PNG){: .align-center}

제 PC의 경우 ```IPv4주소```는 192.168.0.3 이고, ```기본 게이트웨이```는 192.168.0.1 임을 확인했습니다.

<br>

IPTIME의 기본 설정 페이지 접속 주소는 위 기본 게이트웨이 주소와 같습니다. 크롬과 같은 웹 브라우저를 실행 후 해당 주소(http://192.168.0.1)를 입력하여 접속하면 설정 페이지가 출력됩니다.

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_02.PNG){: .align-center}

IPTIME 공유기 초기 로그인 이름은 ```admin```이고, 로그인 암호도 ```admin```입니다. 만약, 계정 정보가 기억나지 않으신다면 IPTIME 초기화 후 다음 과정을 진행하시면 됩니다.

<br>

로그인 후 ```관리도구```를 클릭합니다.

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_03.PNG){: .align-center}

<br>

왼쪽 메뉴에서 ```고급설정``` - ```NAT/라우터 관리``` - ```포트포워드 설정```으로 이동합니다.

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_04.PNG){: .align-center}

<br>

오른쪽 창에 ```새규칙 추가```를 클릭 후 하단 설정창에서 이전에 구축한 주피터 서버 아이피 및 포트번호를 입력하여 추가합니다.

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_05.PNG){: .align-center}


<br><br>


## IPTIME 공유기 설정 - DDNS(Dynamic DNS)

DDNS(Dynamic DNS)는 쉽게 말해 유동적으로 변하는 서버의 IP주소를 문자로 된 도메인 주소에 연결시켜주는 것을 말합니다. 문자로 된 도메인은 변하지 않기 때문에 한 번 설정해두면 IP주소가 변경된다고 하더라도 동일한 문자 도메인으로 주피터 서버에 쉽게 접속할 수 있습니다.

<br>

IPTIME ```관리도구```에서 ```고급 설정``` - ```특수기능``` - ```DDNS 설정```으로 이동합니다.

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_06.PNG){: .align-center}

<br>


호스트 이름은 ```hostname.iptime.org``` 형식으로 임의로 정하면 됩니다. ID는 이메일 주소 형식으로 입력하면 됩니다. DDNS 등록을 마치면 정의한 호스트 이름으로 외부에서도 접속할 수 있습니다.

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_07.PNG){: .align-center}


이제 주피터 서버를 실행해둔 후에 카페와 같은 외부에서 위에서 정의한 ```호스트 이름:포트번호 예: http://hostname.iptime.org:8888``` 로 접속할 수 있습니다.


## 방화벽 해제

포트 포워딩, DDNS 설정을 완료했음에도 불구하고, 주피터 서버 접속이 어려운 경우에는 방화벽이 문제일 수 있습니다. 이 경우 ```cmd```를 실행시킨 후 ```WF.msc```를 입력하여 방화벽 고급 설정을 열어줍니다.

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_08.PNG){: .align-center}

<br>

왼쪽 메뉴에서 ```인바운드 규칙```을 클릭합니다. 이어서 오른쪽 메뉴에서 ```새 규칙```을 클릭합니다. 이어서 다음과 같이 설정을 진행합니다.

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_09.PNG){: .align-center}

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_10.PNG){: .align-center}

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_11.PNG){: .align-center}

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_12.PNG){: .align-center}

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_02/img_13.PNG){: .align-center}

<br>

여기까지 성공하셨다면 이제 인터넷이 가능한 외부 장소 어디에서도 아이패드를 통해 원격지의 주피터 서버에 접속하여 데이터 분석 등 파이썬 코딩을 하실 수 있습니다.



















<br><br>
