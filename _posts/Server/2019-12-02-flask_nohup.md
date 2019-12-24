---
title: "Flask - nohup으로 백그라운드 실행하기"
categories: Server
tags: Flask
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# nohup과 &로 파이썬 플라스크 웹 서버를 백그라운드로 실행하기

AWS와 같은 클라우드 인스턴스에서 웹 서버를 구동할 때, 터미널을 종료해도 프로그램은 계속 실행되도록 해야할 경우가 있습니다. 이때, `nohup`과 `&` 명령어를 사용하면 터미널 종료 후에도 백그라운드로 웹 서버를 구동시킬 수 있습니다. Ubuntu에서 Python Flask App을 백그라운드로 실행하는 방법을 알아보겠습니다.

<br>

## Flask 백그라운드 실행

```bash
$ nohup python -u flask_app.py &
```

- `-u` : 터미널에서 웹 서버 실행 결과를 실시간으로 보고자 할 경우 사용합니다.(아래 nohup.out 설명 참조)  
- `&` : 프로그램을 백그라운드로 실행시켜줍니다. 단, nohup을 사용하지 않으면 터미널 종료 시 프로그램도 함께 종료됩니다.

<br>

## Flask 로그 확인

`nohup`을 이용해 백그라운드로 Flask App을 실행하게 되면, `nohup.out`이라는 로그 파일이 생성됩니다. 다음과 같이 로그를 확인할 수 있습니다.

```bash
$ tail -f nohup.out
```

<br>

## 백그라운드로 실행되고 있는 Flask App 종료

Flask App을 종료하기 위해서는 설정해둔 포트(ex.Flask 기본 포트:5000)를 조회하여 프로세스를 종료할 수 있습니다.

```bash
$ lsof -i :5000
```

```bash
COMMAND   PID  USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
python3 32258 user    3u  IPv4 5575349      0t0  TCP *:5000 (LISTEN)
python3 32260 user    3u  IPv4 5575349      0t0  TCP *:5000 (LISTEN)
python3 32260 user    4u  IPv4 5575349      0t0  TCP *:5000 (LISTEN)
```

<br>

위와 같이 `PID`값을 확인한 뒤, 아래와 같이 종료해주면 Flask App이 종료됩니다.

```bash
$ sudo kill -9 32258
$ sudo kill -9 32260
```
