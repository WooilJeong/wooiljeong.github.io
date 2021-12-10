---
title: "Python Dropbox SDK 파일 업로드 및 다운로드 링크 반환"
categories: Python
tags: dropbox
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 만료기한 없는 Access Token 발급받기

## 배경

- Python으로 Dropbox SDK를 이용하여 파일을 업로드하고, 해당 파일의 다운로드 링크를 가져오는 기능을 구현하고자 함.
- Dropbox SDK를 사용하기 위해서는 Dropbox Developers에서 App을 만들고 파일 관련 Scopes를 설정을 한 후에 Access Token을 발급받아 이용해야 함.
- 문제는 발급받은 Access Token은 발급 직후 4시간 후 만료되어, 재발급받아야 함. 따라서 만료기한이 없는 Access Token을 발급받아 사용해야 하는 상황.

<br>

## 1. Dropbox 앱 만들기

[드롭박스 개발자](https://www.dropbox.com/developers/)에서 '앱 만들기'를 클릭한다.

![PNG](/assets/img/post_img/2021-12/dropbox_sdk_1.png){: .align-center}

<br>

다음과 같이 적절히 설정해주고 앱을 만들면 된다.

![PNG](/assets/img/post_img/2021-12/dropbox_sdk_2.png){: .align-center}

<br>

## 2. 앱 권한 설정

이후 '앱 콘솔'에 들어가면 앱이 하나 생긴 것을 확인할 수 있다. 해당 앱을 클릭한다. 이후 'Permissions' 탭에서 사용할 Scopes들을 적절히 체크해준다. 

![PNG](/assets/img/post_img/2021-12/dropbox_sdk_3.png){: .align-center}

<br>

## 3. 만료기한이 없는 Access Token 발급받기

앱 콘솔 - 앱 - settings에서 OAuth 2의 Redirect URIs 부분에 다음을 입력 후 'Add' 버튼을 클릭한다.

```
https://www.dropbox.com/1/oauth2/display_token
```

<br>

다음 문자열 끝에 있는 APPKEYHERE를 settings의 App key 값으로 대체해준다.

```
https://www.dropbox.com/oauth2/authorize?response_type=token&redirect_uri=https://www.dropbox.com/1/oauth2/display_token&client_id=APPKEYHERE
```

<br>

브라우저에서 Access Token을 원하는 계정에 로그인한 상태에서 위에서 바꿔준 URL로 접속하여 앱을 인증한다. 그러면 Dropbox의 만료기한이 없는 Access Token이 표시된 페이지로 Redirect된다.

![PNG](/assets/img/post_img/2021-12/dropbox_sdk_4.png){: .align-center}

<br>

위 방법을 적용해도 여전히 토큰이 만료되는 이슈가 있다면 다음을 시도해 볼 수 있다.

- 'seetings' 탭의 'OAuth 2'의 'Access token expiration' 드롭다운 옵션 설정

경우에 따라, 위 드롭다운 옵션이 보이지 않는 경우가 있다고 한다. 내 경우에도 해당하는 이슈이다. 이 경우 [개발자 문의](https://www.dropbox.com/developers/contact) 페이지에 앱의 인증 만료기한이 없는 장기 토큰을 발급해달라고 요청하면 된다고 한다. 아직 답장을 받지 못한 상황이어서 추후 경과를 본 후 업데이트하겠다.

<br>

## 4. Python Dropbox SDK

[dropbox API Python 파일 업로드 & 공유 링크 가져오기](https://junwe99.tistory.com/19)에 있는 내용을 바탕으로 아래와 같이 Python Code를 작성하여 사용하면 된다.

```bash
pip install dropbox
```

```python
import dropbox
 
class DropBoxManager:
    def __init__(self,token, filename,pathname):
        self.token = token
        self.fileName = filename
        self.pathName = pathname
 
    def UpLoadFile(self):
        dbx = dropbox.Dropbox(self.token,timeout=900)
        with open(self.fileName, "rb") as f:
            dbx.files_upload(f.read(), self.pathName, mode=dropbox.files.WriteMode.overwrite)
 
    def GetFileLink(self):
        dbx = dropbox.Dropbox(self.token,timeout=900)
        shared_URL = dbx.sharing_create_shared_link_with_settings(self.pathName).url
        modified_URL = shared_URL[:-1] + '1'
        return modified_URL
```

<br>

## References

- [드롭박스 개발자](https://www.dropbox.com/developers/)
- [dropbox API Python 파일 업로드 & 공유 링크 가져오기](https://junwe99.tistory.com/19)
- [만료기한이 없는 Access Token을 발급받을 수 있는 방법](https://www.dropboxforum.com/t5/Dropbox-API-Support-Feedback/Tokens-only-valid-for-4-hours-from-app-console/m-p/425358/highlight/true#M22718)