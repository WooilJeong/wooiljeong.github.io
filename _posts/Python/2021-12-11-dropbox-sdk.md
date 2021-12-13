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

[드롭박스 개발자 앱 콘솔](https://www.dropbox.com/developers/apps)에서 앱을 선택 후 'App key'를 복사한다. 아래 Python 코드에서 APP_KEY의 값으로 복사한 'app key'를 값으로 붙여넣는다. 이후 코드를 실행하면 링크가 출력되는데 해당 링크에 접속해 액세스 코드를 복사해 Python 입력창에 입력한다. Successfully set up client! 문구가 출력된 것을 확인한다. 

```python
import dropbox
from dropbox import DropboxOAuth2FlowNoRedirect

APP_KEY = "fyiyyb32qahe2km"
auth_flow = DropboxOAuth2FlowNoRedirect(APP_KEY, use_pkce=True, token_access_type='offline')

authorize_url = auth_flow.start()
print("1. Go to: " + authorize_url)
print("2. Click \"Allow\" (you might have to log in first).")
print("3. Copy the authorization code.")
auth_code = input("Enter the authorization code here: ").strip()

try:
    oauth_result = auth_flow.finish(auth_code)
except Exception as e:
    print('Error: %s' % (e,))
    exit(1)

with dropbox.Dropbox(oauth2_refresh_token=oauth_result.refresh_token, app_key=APP_KEY) as dbx:
    dbx.users_get_current_account()
    print("Successfully set up client!")
```

<br>

다음을 실행하면 Refresh Token 문자열이 출력되는데 이 값을 복사해둔다. 

```python
print(oauth_result.refresh_token)
```

<br>


## 4. Python Dropbox SDK

Dropbox Python SDK를 다음과 같이 설치한다.

```bash
pip install dropbox
```

위에서 복사해둔 app key와 refresh token 문자열을 아래 항목에 입력하여 DropBoxManager 클래스를 만들어 적절히 사용하면 된다.

```python
import dropbox
 
class DropBoxManager:
    def __init__(self,token, filename,pathname):
        
        self.appkey = "<<app key>>"
        self.refreshtoekn = "<<refresh token>>"
        
        self.fileName = filename
        self.pathName = pathname
 
    def UpLoadFile(self):
        dbx = dropbox.Dropbox(oauth2_refresh_token=self.refreshtoken, app_key=self.appkey, timeout=900)
        with open(self.fileName, "rb") as f:
            dbx.files_upload(f.read(), self.pathName, mode=dropbox.files.WriteMode.overwrite)
 
    def GetFileLink(self):
        dbx = dropbox.Dropbox(oauth2_refresh_token=self.refreshtoken, app_key=self.appkey, timeout=900)
        shared_URL = dbx.sharing_create_shared_link_with_settings(self.pathName).url
        modified_URL = shared_URL[:-1] + '1'
        return modified_URL
```

<br>

## References

- [Dropbox Developers](https://www.dropbox.com/developers/)
- [Oauth2 refresh token question - what happens when the refresh token expires?](https://www.dropboxforum.com/t5/Dropbox-API-Support-Feedback/Oauth2-refresh-token-question-what-happens-when-the-refresh/m-p/486241)
