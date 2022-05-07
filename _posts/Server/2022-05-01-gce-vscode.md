---
title: "VS Code로 GCP Compute Engine 원격 접속하기"
categories: Server
tags: ssh
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## VS Code로 GCE 원격 접속하기

![PNG](/assets/img/post_img/2022-05-01-gce-vscode/gce_vscode.png){: .align-center}

<br>

## 준비물

- Windows 10 PC 및 VS Code
- GCP Compute Engine - Ubuntu 18.04 LTS VM

<br>

## 1. Windows PowerShell로 ssh, public key 생성

```powershell
ssh-keygen -t rsa -f [SSH KEY 파일 명] -C [GCP VM LINUX 접속 계정]
```

- `SSH KEY 파일 명`: 저장할 SSH KEY 파일 이름. ex. “openssh”
- `GCP VM LINUX 접속 계정`: GCP Compute Engine VM 접속 계정. ex. “user”

```powershell
# 예시
ssh-keygen -t rsa -f openssh -C user
```

![PNG](/assets/img/post_img/2022-05-01-gce-vscode/Untitled.png){: .align-center}

- 아래와 같이 두 파일이 생성 됨.
    - ./openssh
    - ./openssh.pub

![PNG](/assets/img/post_img/2022-05-01-gce-vscode/Untitled1.png){: .align-center}

- .pub 으로 끝나는 파일을 메모장으로 열어 다음과 같은 문자열을 모두 복사한다.

```powershell
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDSUB85PokmC0ZNcHMRNf1TUqfVHU94UWXkwz91uJSOQeAc0mR2WngU/QtwF9pvmx3pFmarFO/31V8ZzVq9eNwnyV+bHMfqc3vdsF6yh4zc8/fUu5zFWtMP6SpQpGwYY/4m0dTGXpCvbqf+yyAPswZ4Ks/pwzNhG03xz5n0qzbQYnOC27N/zxBoV0h8Z5Fc2d1fXJOsnLYhD8dy8+BbuaR/8nx3MI6J1NfbTVpzEivSrJ0BYYF3NJyYe4fpVBeITMmQ6mvJJPuMnKFjcJQHdWjFdk55XyxfM+CSMGpPDbwS6pe3wV5L6jf38W+ZeAYQU73vujjwva71iFMcKq4JBtzSgSrSH5D6cEmEo3fIcJJMqHZYUjwSs4WDhYAcPTESeXpQLyFiL8GYaSZ58fnOa8FjZs7SLPNYW0/z8bbj+hG9g2LBj4BbBFeJEPvKEScGZfMD+3858LknawAc8BxH+iokZjA0tYRe8NgE1YgB11bYFk0+tThbdncK3dclLYHQ5Ms= user
```

<br>

## 2. GCP Compute Engine VM 콘솔에서 public key 항목 추가

- **GCP → Compute Engine → VM 선택 → 세부정보 → 수정 → SSH 키** 부분으로 이동한다.
- `+ 항목 추가` 버튼 클릭하여 SSH 키를 입력할 수 있는 공란을 하나 추가한다.
- 추가한 항목 란에 위에서 복사해둔 문자열을 붙여넣고 `저장` 버튼을 누른다.

<br>

## 3. VS Code - 원격 SSH 접속

- VS Code를 열어 `Remote SSH` 플러그인을 설치한다.

![PNG](/assets/img/post_img/2022-05-01-gce-vscode/Untitled2.png){: .align-center}

- 좌측 메뉴의 가장 아래에 새로 생긴 **‘Remote SSH 탭’**으로 이동 후 **‘+’** 를 클릭한다.

![PNG](/assets/img/post_img/2022-05-01-gce-vscode/Untitled3.png){: .align-center}

- `SSH Connection Command` 에 다음을 입력한다.

```bash
ssh -i [OPEN SSH KEY 파일 경로] [VM Linux 계정 명]@[VM 공개 IP]
```

- `OPEN SSH KEY 파일 경로`: 위에서 생성한 OPEN SSH KEY 파일의 경로
- `VM Linux 계정 명`: VM Linux에서 사용하는 계정 이름
- `VM 공개 IP`: VM의 Public IP 주소

```bash
ssh -i D:\\openssh user@34.XX.XX.XX
```

![PNG](/assets/img/post_img/2022-05-01-gce-vscode/Untitled4.png){: .align-center}

- `C:\users\my\.ssh\config` 를 선택한다.

![PNG](/assets/img/post_img/2022-05-01-gce-vscode/Untitled5.png){: .align-center}

- SSH TARGETS 목록에 항목이 추가되면 항목명 우측에 있는 아이콘을 클릭한다.

![PNG](/assets/img/post_img/2022-05-01-gce-vscode/Untitled6.png){: .align-center}

- 아래와 같이 새 VS Code 창이 뜨는데, Linux 항목을 누르면 원격 접속이 완료된다.

![PNG](/assets/img/post_img/2022-05-01-gce-vscode/Untitled7.png){: .align-center}

<br>

## 참고

- [https://faun.pub/connect-gcps-vm-with-vs-code-on-windows-10-b4fc7e73afea](https://faun.pub/connect-gcps-vm-with-vs-code-on-windows-10-b4fc7e73afea)