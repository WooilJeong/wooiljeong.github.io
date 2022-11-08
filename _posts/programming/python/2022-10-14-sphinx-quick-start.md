---
title: Python Sphinx로 개발 문서 페이지 만들기 (feat.GitHub Pages)
categories: python
tags: sphinx
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

Sphinx를 이용하면 Python으로 작성한 모듈, 클래스 그리고 함수 등의 소스코드에 대한 정적 페이지(HTML)을 자동으로 만들 수 있다. 

<br>

## Sphinx와 GitHub Pages를 이용한 개발 문서 배포

이렇게 만든 페이지를 GitHub Pages를 이용해 무료로 웹에 배포할 수도 있다. 구글링을 하면 Sphinx를 이용해 문서를 만들어 웹에 배포하는 방법에 대해 자세히 다루는 글들이 꽤 나온다. 그러나 Sphinx 최신 버전과 맞지 않는 부분이 있었고, 이미 GitHub Page를 이용해 Jekyll 블로그를 이용하고 있는 경우 문서 배포 시 테마가 적용되지 않는 경우도 있었다. 이러한 문제를 일부 수정했다. 그럼 Sphinx를 이용해서 미리 작성한 Python 소스코드에 대한 문서 페이지(HTML)를 만드는 방법을 살펴보자. 그다음 문서를 GitHub 저장소에 올린 뒤, 매우 간단한 설정으로 웹에 문서를 배포하는 방법도 알아보자. Sphinx에 대한 보다 자세한 내용은 Sphinx의 [공식문서](https://www.sphinx-doc.org/en/master/)에서 확인할 수 있다.

![PNG](/assets/img/post_img/2022-10-14-sphinx-quick-start/sphinx-logo.png){: .align-center}

<br>

## 주요 내용

- Sphinx 설치하기
- Python 소스코드 관련 문서 만들기
- GitHub Pages로 문서 웹에 배포하기

<br>

## Sphinx 설치하기

Shell 명령으로 sphinx와 sphinx 테마 관련 라이브러리를 설치한다.

```bash
# Sphinx 설치
pip install sphinx
# Sphinx 테마 설치
pip install sphinx-rtd-theme
```

<br>

## Python 소스코드 관련 문서 만들기

### docs/ 만들기

다음과 같은 shell 명령을 이용해 Python 프로젝트 루트 경로 하위에 docs 경로를 생성한다.

```bash
sphinx-quickstart docs
```

위 명령을 실행하면 아래와 같이 몇 가지 옵션을 선택하는 과정이 실행된다. 우선 아래와 같이 간단하게 옵션을 선택해주면 프로젝트 루트 아래에 docs 경로가 만들어진다.

```
Sphinx 5.2.3 빠른 시작 유틸리티에 오신 것을 환영합니다.

다음 설정에 대한 값을 입력하십시오 (대괄호로 묶여 있는 기본값이 존재하고
이 값을 사용하려면 바로 Enter를 누릅니다).

선택한 루트 경로: docs

Sphinx 출력을 위한 빌드 디렉토리를 배치하는 두 가지 옵션이 있습니다.    
루트 경로 내에서 "_build" 디렉토리를 사용하거나, 루트 경로 내에서       
"source"와 "build" 디렉토리로 분리합니다.
> 원본과 빌드 디렉토리 분리 (y/n) [n]: y

프로젝트 이름은 빌드 된 문서의 여러 위치에 표시됩니다.
> 프로젝트 이름: sphinx test
> 작성자 이름: wooil
> 프로젝트 릴리스 []: 

문서를 영어 이외의 언어로 작성하려는 경우, 여기에서 해당 언어 코드로 언어를
선택할 수 있습니다. 그러면 Sphinx가 생성한 텍스트를 해당 언어로 번역합니다.

지원되는 코드 목록은
https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-language
를 참조하십시오.
> 프로젝트 언어 [en]: 

D:\github\sphinx-test\docs\source\conf.py 파일을 만드는 중입니다.
D:\github\sphinx-test\docs\source\index.rst 파일을 만드는 중입니다.
D:\github\sphinx-test\docs\Makefile 파일을 만드는 중입니다.
D:\github\sphinx-test\docs\make.bat 파일을 만드는 중입니다.

완료됨: 초기 디렉토리 구조가 생성되었습니다.

이제 마스터 파일 D:\github\sphinx-test\docs\source\index.rst을(를) 채우고 다른 원본 문서 파일을 만들어야 합니다. Makefile을 사용하여 다음과 같이 문서를 빌드하십시오:
   make builder
여기서 "builder"는 지원되는 빌더 중 하나(예: html, latex, linkcheck)입니다.
```

<br>

### src/my_package 만들기

프로젝트 루트 경로에 src라는 폴더를 하나 만들고, 그 아래에 Python 예제 모듈인 my_package를 만든다. 문서 생성과 웹 배포까지 정상적으로 테스트한 뒤에는 예제 모듈 대신 실제 문서화할 비즈니스 로직으로 대체하면 된다. 예제 모듈은 임의로 작성하면 되지만, [예제 GitHub Repo](https://github.com/WooilJeong/sphinx-test) 에 접속해 code - Download ZIP 으로 파일을 다운받아 사용해도 된다. 

<br>

### docs/source/conf.py 수정하기

conf.py 파일을 열면 아래 `# before` 와 같이 extensions는 빈 리스트, html_theme은 alabaster로 작성되어 있는 것을 확인할 수 있다. 이 값을 `# after` 에 적혀 있는 값으로 대체한다. Sphinx의 확장 기능과 문서 페이지의 테마를 설정해주는 부분이다. 확장 기능을 사용하지 않았을 때에는 빌드 오류가 발생할 수도 있다. 참고로 `sphinx.ext.napoleon` 도 extensions에 추가하면 NumPy 및 Google Style Docstring을 지원해준다.

```python
# Before
extensions = []
html_theme = 'alabaster'

# After
extensions = ['sphinx.ext.autodoc']
html_theme = 'sphinx_rtd_theme'
```

conf.py 파일 가장 하단에 아래 내용을 추가해준다. Python 소스코드의 경로를 인식할 수 있도록 설정해준다고 이해하면 된다.

```python
## 추가
import os
import sys
sys.path.insert(0, os.path.abspath('../../src/my_package'))
```

<br>

### rst(Re-Structured-Text) 파일 만들기

프로젝트 루트 경로에서 다음 shell 명령을 실행하면 docs/source/ 경로 하위에 rst 파일들이 만들어진다.

```bash
sphinx-apidoc -f -o docs/source/ src/my_package
```

```bash
docs/source/base.rst 파일을 만드는 중입니다.
docs/source/utils.rst 파일을 만드는 중입니다.
docs/source/modules.rst 파일을 만드는 중입니다.
```

<br>

### docs/source/index.rst 파일 수정하기

모든 Python 모듈을 포함하기 위해서 index.rst에 modules를 아래와 같이 추가한다.

```bash
.. sphinx test documentation master file, created by
   sphinx-quickstart on Thu Oct 13 16:38:27 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to sphinx test's documentation!
=======================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   modules

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
```

<br>

### 문서 페이지 만들기

이제 docs 경로에서 shell 명령을 이용해 HTML 형식으로 구성된 문서 페이지들을 만든다. 참고로 Windows 환경에서 VSCode로 위 과정을 문제 없이 진행했었는데, make html 명령만 동작하지 않았다. 이 경우 CMD(명령 프롬프트)를 열어 실행하니 정상적으로 문서 페이지를 만들 수 있었다.

```bash
cd docs
make html
```

```bash
Running Sphinx v5.2.3
loading translations [ko]... done
loading pickled environment... done
building [mo]: targets for 0 po files that are out of date
building [html]: targets for 4 source files that are out of date
updating environment: [extensions changed ('sphinx.ext.autodoc')] 4 added, 0 changed, 0 removed
reading sources... [100%] utils
looking for now-outdated files... none found
pickling environment... done
checking consistency... done
preparing documents... done
writing output... [100%] utils
generating indices... genindex py-modindex done
writing additional pages... search done
copying static files... done
copying extra files... done
dumping search index in English (code: en)... done
dumping object inventory... done
build succeeded.

The HTML pages are in build\html.
```

<br>

### docs/build/html 내 모든 파일을 복사해 docs/ 에 붙여넣기

이제 docs/build/html 경로에 존재하는 모든 파일을 모두 복사해 docs/ 경로 하위에 붙여넣어 준다. 뒤에서 GitHub Pages를 이용해 웹에 문서를 배포할 때 이와 같은 구성으로 되어 있어야 인식이 잘 된다. 즉, docs/ 경로 하위에 index.html 파일이 있어야 하는 것 같다.

<br>

### (선택) docs/.nojekyll 파일 만들기

GitHub 계정으로 이미 Jekyll 블로그를 사용하고 있는 경우가 아니라면 이 과정은 패스하면 된다. 만약, 이미 Jekyll을 이용해 블로그를 운영중이라면 docs/ 경로 하위에 .nojekyll 이라는 아무 내용도 없는 빈 파일을 만들어주면 웹 배포 후 테마가 적용되지 않는 문제를 해결할 수 있다.

<br>

## GitHub Pages로 문서 웹에 배포하기

### GitHub 저장소에 Push하기

GitHub 저장소를 하나 만들고, 프로젝트를 Push한다.

```bash
git add .
git commit -m 'init'
git push
```

<br>

### GitHub Page 만들기

GitHub 저장소에 접속한 후 아래와 같이 GitHub Pages 설정을 마치면 곧 웹에서 작성한 문서 페이지를 확인할 수 있다.

- Settings → Pages → Branch 영역에서 브랜치와 /docs 경로를 설정 후 저장
- 일정 시간이 지나면 GitHub 페이지 접속이 가능해진다.

![PNG](/assets/img/post_img/2022-10-14-sphinx-quick-start/1.png){: .align-center}

문서 페이지에 접속하면 다음과 같이 잘 만들어진 것을 확인할 수 있다.

![PNG](/assets/img/post_img/2022-10-14-sphinx-quick-start/2.png){: .align-center}

<br>

## 참고

[https://www.sphinx-doc.org/en/master/](https://www.sphinx-doc.org/en/master/)

[https://data-newbie.tistory.com/818](https://data-newbie.tistory.com/818)