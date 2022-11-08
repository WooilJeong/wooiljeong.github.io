---
title: "Python 상위 경로 제외 파일 압축"
categories: python
tags: zipfile
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# Python 상위 경로 제외 파일 압축

## 배경

- 특정 폴더 내 파일들을 하나의 압축 파일로 만들어야 하는 상황.
- 압축 파일 내에 상위 경로 폴더는 제외하고 싶은 상황.

<br>

## 방법

```python
import zipfile
from pathlib import Path

# 압축할 파일이 존재하는 경로 지정
files_path = "D:/Target"

# 압축 파일 저장 경로 지정
zip_file_path = "D:/Target.zip"

# 압축파일 생성 핸들러 정의
ziph = zipfile.ZipFile(zip_files_path, "w")

# 압축할 파일 경로 루프돌며 압축파일에 삽입
for file_path in Path(files_path).rglob("*"):
    ziph.write(file_path, file_path.name)

# 압축파일 생성 핸들러 종료
ziph.close()
```