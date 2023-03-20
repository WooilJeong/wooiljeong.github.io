---
title: "Python으로 PDF를 이미지로 변환하기(pdf to image)"
categories: python
tags: pdf pymupdf pdf2image
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 파이썬으로 PDF파일을 이미지 파일(JPG, PNG)로 변환하는 방법

파이썬으로 PDF 파일을 JPG이나 PNG와 같은 이미지 파일로 변환하는 방법에 대해 알아보겠습니다. PyMuPDF, pdf2image와 같은 라이브러리를 사용하면 PDF 파일을 쉽게 이미지 파일로 변환할 수 있습니다.

<br>

## 예제 PDF 파일 준비하기

이미지 파일로 변환할 **예제 PDF 파일**(sample.pdf)를 준비합니다. 다운로드한 파일이 위치한 경로를 'PDF_FILE_PATH' 변수에 할당합니다.

- 예제 PDF 파일 출처: [국토교통부, [참고] 제주 제2공항 기본계획(안) 공개, 제주도와 협의 착수](https://www.molit.go.kr/USR/NEWS/m_72/dtl.jsp?lcmspage=1&id=95088010)


```python
# 예제 PDF 파일 경로
PDF_FILE_PATH = "./data/sample.pdf"
```

<br>

## PyMuPDF 설치하기

- 관련 사이트
  - [GitHub - PyMuPDF](https://github.com/pymupdf/PyMuPDF)
  - [PyPI - PyMuPDF](https://pypi.org/project/PyMuPDF/)
  - [Docs - PyMuPDF](https://pymupdf.readthedocs.io/en/latest/)
- 설치
```bash
pip install PyMuPDF
```

<br>

## PyMuPDF로 PDF를 PNG로 변환하기

```python
import fitz
path = "./data/sample.pdf"
doc = fitz.open(path)
for i, page in enumerate(doc):
    img = page.get_pixmap()
    img.save(f"./data/{i}.png")
```

![PNG](/assets/img/post_img/2023-03-04-pdf-to-image/6.png){: .align-center}

<br>

## pdf2image 설치하기

- 관련 사이트
    - [GitHub-pdf2image](https://github.com/Belval/pdf2image)
    - [PyPI-pdf2image](https://pypi.org/project/pdf2image/1.16.3/)
    - [Docs-pdf2image](https://pdf2image.readthedocs.io/en/latest/index.html)


<br>

### Windows

윈도우 환경에서는 윈도우용 poppler를 빌드하거나 다운로드해야 합니다. 가장 최신 버전을 사용하는 것을 권장합니다. [@oschwartz10612 version](https://github.com/oschwartz10612/poppler-windows/releases/)에 접속 후 '**Release-23.01.0-0.zip**'를 클릭해 다운로드합니다.

![PNG](/assets/img/post_img/2023-03-04-pdf-to-image/1.png){: .align-center}

다운로드한 파일('Release-23.01.0-0.zip')을 C드라이브 하위에 압축해제('C:/poppler-23.01.0')합니다. 

그 다음 윈도우 키를 누르고 '시스템 환경 변수 편집'을 검색 후 실행합니다. '시스템 속성' 창이 열리면 '고급' 탭에서 '환경 변수' 버튼을 클릭합니다.

![PNG](/assets/img/post_img/2023-03-04-pdf-to-image/2.png){: .align-center}

사용자 변수에서 'Path' 항목을 찾아 클릭 후 '편집' 버튼을 클릭합니다.

![PNG](/assets/img/post_img/2023-03-04-pdf-to-image/3.png){: .align-center}

'새로 만들기' 버튼을 클릭 후 'C:/poppler-23.01.0/Library/bin'를 입력후 '확인' 버튼을 클릭합니다.

![PNG](/assets/img/post_img/2023-03-04-pdf-to-image/4.png){: .align-center}

poppler 설치와 설정이 끝났다면 명령 프롬프트(CMD)를 열어 다음 명령어를 입력해 pdf2image를 설치합니다.

```bash
pip install pdf2image
```

<br>

### Mac

맥 환경에서는 brew로 poppler를 먼저 설치해야합니다. 터미널을 열어 아래 명령어를 순서대로 입력해 poppler와 pdf2image를 설치합니다.

```bash
brew install poppler
pip install pdf2image
```

<br>

### Linux

대부분의 리눅스 배포판은 pdftoppm와 pdftocairo와 함께 제공됩니다. 설치되지 않은 경우 패키지 관리자를 참조해 poppler-utils를 설치해야 합니다. poppler-utils 설치 후 다음 명령어를 입력해 pdf2image를 설치합니다.

```bash
pip install pdf2image
```

<br>

## pdf2image로 PDF를 JPG로 변환하기

다음은 pdf2image 라이브러리를 사용해 PDF 파일의 각 페이지를 이미지로 변환하는 코드입니다.

먼저 pdf2image 라이브러리에서 convert_from_path 함수를 불러옵니다. PDF 파일 경로를 인자로 넣어 convert_from_path() 함수를 호출해 PDF 파일의 모든 페이지를 이미지 형식으로 변환합니다. 이때 변환된 이미지들은 pages라는 변수에 리스트 형태로 저장됩니다.

그 다음 반복문을 이용해 pages 리스트에 있는 이미지들을 하나씩 꺼내 해당 이미지를 JPEG 형식으로 저장합니다. 이때 파일명은 페이지 번호로 지정됩니다. 예를 들어 첫 번째 페이지는 './data/0.jpg'의 파일명으로 저장됩니다.


```python
from pdf2image import convert_from_path
pages = convert_from_path(PDF_FILE_PATH)
for i, page in enumerate(pages):
    page.save(f"./data/{str(i)}.jpg", "JPEG")
```

![PNG](/assets/img/post_img/2023-03-04-pdf-to-image/5.png){: .align-center}

<br>

## 참고

- 예제 PDF 파일 출처: [국토교통부, [참고] 제주 제2공항 기본계획(안) 공개, 제주도와 협의 착수](https://www.molit.go.kr/USR/NEWS/m_72/dtl.jsp?lcmspage=1&id=95088010)
