---
title: "Windows 10에 Flutter 설치하기"
categories: etc
tags: Flutter
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---


## 환경

- OS: Windows 10 Pro

<br>

## Flutter SDK 다운로드

- [https://flutter.dev/docs/get-started/install/windows](https://flutter.dev/docs/get-started/install/windows) 접속 후 `flutter_windows_X.X.X-stable.zip` 파란색 버튼 클릭하여 다운로드

- `C:/src` 경로를 생성 후 다운로드 받은 파일을 이 경로에서 압축 해제

<br>

## 환경변수 Path 설정

- `Win` + `E` 키 입력 - 내 PC 우클릭 - 속성 - 고급 시스템 설정 - 고급 - 환경 변수
- 사용자 변수의 Path에 `C:\src\flutter\bin` 추가

<br>

## Flutter 설치 확인

- 설치된 Flutter의 버전 정보가 아래와 같이 출력되면 정상

```bash
# 버전 확인
flutter --version
```

```bash
Flutter 2.5.0 • channel stable • https://github.com/flutter/flutter.git
Framework • revision 4cc385b4b8 (2 days ago) • 2021-09-07 23:01:49 -0700
Engine • revision f0826da7ef
Tools • Dart 2.14.0
```

<br>

## Flutter 설정 완료를 위한 의존성 확인

- 추가 설치가 필요한 항목 확인

```bash
# 설정을 완료하는 데 필요한 플랫폼 의존성이 있는지 확인
flutter doctor
```

```bash
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 2.5.0, on Microsoft Windows [Version 10.0.19042.1165], locale ko-KR)
[X] Android toolchain - develop for Android devices
    X Unable to locate Android SDK.
      Install Android Studio from: https://developer.android.com/studio/index.html
      On first launch it will assist you in installing the Android SDK components.
      (or visit https://flutter.dev/docs/get-started/install/windows#android-setup for detailed instructions).
      If the Android SDK has been installed to a custom location, please use
      `flutter config --android-sdk` to update to that location.

[√] Chrome - develop for the web
[!] Android Studio (not installed)
[√] VS Code (version 1.60.0)
[√] Connected device (2 available)

! Doctor found issues in 2 categories.
```

<br>

## Android Studio 및 Flutter Plugin 설치

- Android Studio: [https://developer.android.com/studio/install?hl=ko](https://developer.android.com/studio/install?hl=ko)
- Flutter Plugin: 
- Android Studio - Plugins - Fluuter 검색 및 설치
- Android Studio - More Actions - SDK Manager - SDK Tools - `Android SDK Command-line Tools (latest)` 체크 후 Apply

```bash
# 안드로이드 라이센스 동의
flutter doctor --android-licenses
```

<br>

## Flutter Doctor 재확인

```bash
flutter doctor
```

```bash
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 2.5.0, on Microsoft Windows [Version 10.0.19042.1165], locale ko-KR)
[√] Android toolchain - develop for Android devices (Android SDK version 31.0.0)
[√] Chrome - develop for the web
[√] Android Studio (version 2020.3)
[√] VS Code (version 1.60.0)
[√] Connected device (2 available)

• No issues found!
```