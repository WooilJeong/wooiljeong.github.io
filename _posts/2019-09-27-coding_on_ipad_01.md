---
title: "아이패드로 주피터 노트북 사용하기 01"
date: 2019-09-27 00:00:00 -0000
categories: Tutorial
tags: jupyter notebook
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## 아이패드로 주피터 노트북 사용하기

아이패드에 주피터 노트북을 직접 설치하지 않고, 주피터 서버에 원격 접속하여 파이썬 코딩 등을 할 수 있는 방법을 소개해드리겠습니다. 이 방법은 아이패드 뿐 아니라 노트북에도 동일하게 적용 가능합니다. 종종 팀 프로젝트나 해커톤 등에 참여할 때에도 유용하게 활용하고 있습니다.

<br>

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_01/img_logo.PNG){: .align-center height="300px" width="300px"}

<br>

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_01/img_01.PNG){: .align-center}

<br>


> 필요한 준비물은 다음과 같습니다.

- Server-Anaconda, IPTIME 공유기 설치된 PC(Desktop PC(Win 10 Pro) 사용)
- Client-아이패드 프로 10.5(일반 노트북 및 스마트폰도 가능)

<br><br>

## 주피터 서버 구축하기

명령 프롬프트 ('```CTRL``` + ```R``` - ```cmd```) 실행 후  다음 명령어를 입력합니다.

```
> jupyter notebook --generate-config
```

명령어를 실행하면 ```C:\Users\(username)\.jupyter``` 폴더 안에 ```jupyter_notebook_config.py``` 파일이 생성됩니다.

<br>

이어서 주피터 서버에 설정할 비밀번호를 생성하기 위해 다음 명령어를 입력합니다.

```
> iPython
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter passwd: (사용할 비밀번호 입력(예:wooil))
Verify passwd: (사용할 비밀번호 입력(예:wooil))
```

<br>

위와 같이 명령어를 차례대로 입력하면 다음과 같이 사용할 비밀번호가 암호화 됩니다. 따옴표 안의 암호화된 비밀번호를 복사합니다.

```
Out[2]: 'sha1:675e46664694:2917c374febf1fa1091bd6ddaf9029fbefc229cf'
```

<br>

이전에 생성한 ```jupyter_notebook_config.py``` 파일을 텍스트 편집기(예:메모장)에서 열어줍니다. 다음의 부분을 찾아 주석 처리 ```#``` 를 지우고 알맞게 설정해줍니다.

```c.NotebookApp.password = ``` 부분에 복사한 암호를 붙여넣습니다.  
```c.NotebookApp.ip = ``` 부분 뒤에 나오는 ip 주소를 확인합니다. 예:'192.168.0.3'  
```c.NotebookApp.port = ``` 부분 뒤에 나오는 포트번호를 확인합니다. 예:'8888'  
```c.NotebookApp.notebook_dir = ``` 부분 뒤에 주피터 노트북을 실행할 경로를 설정합니다. 예:'C:/project'  

<br>

여기까지 설정을 마쳤다면 기본적인 주피터 서버 설정이 완료된 것입니다. 이제 명령 프롬프트에서 주피터 서버를 실행해봅시다.

```
> jupyter notebook
```
![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_01/img_02.PNG)

<br>

> 아이패드에 주피터 서버 접속 예시

![PNG](/assets/img/post_img/2019-09-27-coding_on_ipad_01/img_03.PNG)

<br>

이제 아이패드와 같은 클라이언트가 서버와 같은 인터넷 망에 연결되어 있다면, 웹 브라우저 상에서 위에서 설정한 ```ip주소:포트번호 (예:https://192.168.0.3:8888)```로 접속이 가능합니다. 만약, 내부 망에서만 접속할 것이 아니라 카페 등 외부에서도 원격지의 주피터 서버에 접속하여 작업하고 싶다면 원격 접속 설정을 진행하시면 됩니다. 관련 내용은 '[아이패드로 주피터 노트북 사용하기 02](https://wooiljeong.github.io/tutorial/coding_on_ipad_02/)' 에서 확인하실 수 있습니다.
