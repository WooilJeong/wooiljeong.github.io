---
title: GitHub Copilot으로 Python 코드 자동 작성하기
categories: etc
tags: copilot
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

깃허브 코파일럿(GitHub Copilot)을 이용해 자동으로 파이썬(Python) 코드를 작성하는 방법에 대해 알아보자. 

<br>

## GitHub Copilot이란?

![PNG](/assets/img/post_img/2022-10-16-github-copilot/logo.jpeg){: .align-center}


깃허브 코파일럿(GitHub Copilot)은 2021년 GitHub에서 출시한 자동 코드 완성 인공지능이다. OpenAI의 GPT-3 모델을 이용하여 깃허브의 수많은 레포지토리들을 학습시키는 방식으로 개발되었다. 주석이나 함수 이름에 담긴 의미를 파악하여 코드를 자동 완성해, 단순하고 번거로운 작업을 자동화한다는 점이 특징이다. 최근 변경된 정책에 의하면 계정당 1회 60일간 무료체험 후 월 10 USD나 연 100 USD 정액제로 전환하여 유료로 이용 가능하다고 한다. 다만, 학생 인증 계정이나 오픈소스 메인테이너는 무료로 이용할 수 있다고 한다.

<br>

## Copilot 무료체험 신청하기

위에서 언급한 무료체험이 가능한 계정을 소유했다면, Copilot 무료체험 신청을 진행해보자. 우선 [GitHub Copilot](https://github.com/features/copilot) 접속하여 `Start my free trial` 을 누른다. 

![PNG](/assets/img/post_img/2022-10-16-github-copilot/0.png){: .align-center}


<br>

하단의 버튼을 클릭해준다.

![PNG](/assets/img/post_img/2022-10-16-github-copilot/1.png){: .align-center}

아래와 같이 `Suggestions matching public code` 를 허용해주고, GitHub이 내 코드 스니펫을 사용하겠다는 내용의 체크박스는 우선 해제한다. 그 다음 하단의 버튼을 클릭한다.

![PNG](/assets/img/post_img/2022-10-16-github-copilot/2.png){: .align-center}

<br>

아래와 같은 화면이 출력되면 신청이 완료된 것이다.

![PNG](/assets/img/post_img/2022-10-16-github-copilot/3.png){: .align-center}


<br>

## VSCode에 GitHub Copilot 확장 기능 설치하기

이제 VSCode를 열고 아래와 같이 GitHub Copilot 확장 기능을 설치한다.

![PNG](/assets/img/post_img/2022-10-16-github-copilot/4.png){: .align-center}


<br>

GitHub Copilot에 접근하기 위해 로그인하라는 알림이 뜨면, `Sign in to GitHub` 버튼을 누른다.

![PNG](/assets/img/post_img/2022-10-16-github-copilot/5.png){: .align-center}


<br>

GitHub 로그인 관련 알림이 뜨면 `허용` 버튼을 누른다.

![PNG](/assets/img/post_img/2022-10-16-github-copilot/6.png){: .align-center}


<br>

웹 브라우저가 실행되고, 아래와 같이 알림이 뜨면 `Visual Studio Code 열기` 를 누른다.

![PNG](/assets/img/post_img/2022-10-16-github-copilot/7.png){: .align-center}


<br>

VSCode 창에 다음과 같은 알림이 뜨면 `열기` 를 누른다.

![PNG](/assets/img/post_img/2022-10-16-github-copilot/8.png){: .align-center}


<br>

## Copilot 모드로 Python 코드 자동으로 작성하기

VSCode로 .py 파일을 하나 만든 뒤 아래와 같이 Python 코드를 작성하기 시작하면 다음에 작성할 코드가 자동으로 추천된다. 추천한 코드를 사용하려면 코드 작성 중에 Tab 키를 누르기만 하면 된다. 

![PNG](/assets/img/post_img/2022-10-16-github-copilot/9.png){: .align-center}


<br>

예시로 pandas를 임포트하는 코드를 작성하니 다음 줄에 이어서 numpy를 임포트 하는 코드가 추천된다. 

![PNG](/assets/img/post_img/2022-10-16-github-copilot/10.png){: .align-center}


<br>

다음은 위 과정을 반복해 계속해서 Copilot이 추천해준 코드로만 작성한 결과이다. pandas를 불러오는 코드 한 줄만 작성했을 뿐인데, 이어서 연관된 라이브러리들을 순차적으로 추천해준다. 실제 주피터 노트북 환경에서 데이터 분석을 하는 경우 warnings 모듈로 오류 메세지를 출력하지 않도록 설정하고 있는데, 이 점이 잘 반영된 것 같다. 그 이후에는 Scikit Learn 라이브러리의 여러 하위 기능을 끝없이 임포트하는 코드만 추천하는 걸 확인했다.

![PNG](/assets/img/post_img/2022-10-16-github-copilot/11.png){: .align-center}


<br>

이번에는 warnings 관련 코드 이후에 Scikit Learn 관련 모듈을 임포트하는 코드를 사용하지 않고, 한 줄을 띄워 `# URL` 이라고 주석을 적었다. 그 이후 Copilot에 의해 추천되는 코드로만 코딩한 결과, 임의의 CSV 파일 주소를 변수에 할당하고, 이어서 데이터프레임으로 파일을 읽어 데이터 구조를 확인하는 과정이 이어졌다. 

![PNG](/assets/img/post_img/2022-10-16-github-copilot/12.png){: .align-center}


<br>

## 첫인상

아주 간략하게 GitHub Copilot을 이용해본 결과, 생각보다 합리적으로 코드를 추천해준다는 느낌을 받았다. 또, 처음에 무지성으로 코드를 작성했을 때에는 라이브러리를 임포트하는 코드만 일관되게 추천해줬지만, 적당한 위치에서 코드 작성에 개입을 하니, 그에 맞는 방향으로 추천을 바꿔주는 점이 매우 인상적이었다. 언어, 프레임워크 그리고 라이브러리 등에 대한 숙련도와 풀고자 하는 도메인에 대한 이해도가 높다면 GitHub Copilot은 생산성 높은 도구가 될 수도 있을 것 같다.

<br>

## 참조

[https://github.com/pricing](https://github.com/pricing)