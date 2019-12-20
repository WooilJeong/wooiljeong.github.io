---
title: "python 표준 라이브러리 소개"
categories: Python
tags: Python
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 파이썬 표준 라이브러리 소개

## os

- 시스템 환경벼수 확인, 디렉토리 설정, 시스템 명령어 실행 등


```python
import os
```

### os.environ - 시스템 환경 변수값 확인


```python
os.environ['PATH']
```




    '/Users/wooil/opt/anaconda3/envs/wooil/bin:/Users/wooil/opt/anaconda3/condabin:/Users/wooil/opt/anaconda3/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Applications/Visual Studio Code.app/Contents/Resources/app/bin'



### os.getcwd - 현재 디렉토리


```python
os.getcwd()
```




    '/Users/wooil/github'



### os.chdir - 디렉토리 변경


```python
os.chdir("/Users/wooil")
```

### os.system - 시스템 명령어 실행


```python
os.system('touch test.txt')
```




    0



### os.popen - 시스템 명령어 실행 결과 반환


```python
r = os.popen('python --version')
r.read()
```




    'Python 3.7.5\n'



### os.mkdir - 디렉토리 생성


```python
os.mkdir('mkdir_test')
```

### os.rmdir - 디렉토리 삭제


```python
os.rmdir('mkdir_test')
```

### os.unlink - 파일 삭제

- 위에서 `os.system()`으로 생성한 `test.txt`파일 삭제
- `os.system('rm -rf test.txt')` 명령어도 가능


```python
os.unlink('test.txt')
```

### os.rename(old_nm, new_nm)

- `old_nm`라는 이름의 파일을 `new_nm`라는 이름으로 변경


```python
os.system('touch old_nm.csv')
os.rename('old_nm.csv', 'new_nm.csv')
```

## shutil

- 파일 복사 기능


```python
import shutil
```

### shutil.copy - 파일 복사


```python
shutil.copy('new_nm.csv', 'new_nm_copy.csv')
```




    'new_nm_copy.csv'



## glob

- 디렉토리 내 파일 목록 확인 기능


```python
import glob
```

### glob.glob - 디렉토리 내 모든 파일 리스트 출력


```python
file_list = glob.glob("/Users/wooil/github/test/Blog/Data/*")
file_list
```




    ['/Users/wooil/github/test/Blog/Data/data_01.csv',
     '/Users/wooil/github/test/Blog/Data/data_02.csv',
     '/Users/wooil/github/test/Blog/Data/data_03.csv']



## sys


```python
import sys
```

### sys.path - 파이썬 모듈 저장 위치

- sys.path.append("DIR") - 사용할 파이썬 모듈 경로 추가


```python
sys.path
```




    ['/Users/wooil/github/test/Blog',
     '/Users/wooil/opt/anaconda3/envs/wooil/lib/python37.zip',
     '/Users/wooil/opt/anaconda3/envs/wooil/lib/python3.7',
     '/Users/wooil/opt/anaconda3/envs/wooil/lib/python3.7/lib-dynload',
     '',
     '/Users/wooil/.local/lib/python3.7/site-packages',
     '/Users/wooil/opt/anaconda3/envs/wooil/lib/python3.7/site-packages',
     '/Users/wooil/opt/anaconda3/envs/wooil/lib/python3.7/site-packages/aeosa',
     '/Users/wooil/opt/anaconda3/envs/wooil/lib/python3.7/site-packages/IPython/extensions',
     '/Users/wooil/.ipython']



### sys.argv - 시스템 인수 전달

```python
# test.py

import sys
print(sys.argv[1])
```

위와 같이 `test.py`를 생성한 후, 터미널에서 아래 명령어를 입력하면 test.py 뒤 인자를 출력함.

```bash
$ python test.py apple
```

```bash
apple
```

os 라이브러리의 메소드로 다음과 같이 결과를 확인할 수도 있음.


```python
os.chdir('/Users/wooil/github/test/Blog/')
os.popen('python test.py apple').read()
```




    'apple\n'



## math

- 스칼라 연산 등 가벼운 계산 시 사용.
- 행렬, 배열 계산 등 큰 데이터 셋 계산 시 ```NumPy``` 사용

[math 세부 메소드 확인](https://docs.python.org/3/library/math.html)

**math와 numpy 비교**


```python
import math
import numpy as np
```


```python
a = 3
b = [3,6,9]
```


```python
math.cos(a)
```




    -0.9899924966004454




```python
np.cos(b)
```




    array([-0.9899925 ,  0.96017029, -0.91113026])



## random

- 난수 생성 기능


```python
import random
import matplotlib.pyplot as plt
import seaborn as sns
```

### random.random()
- Uniform Distribution - range : 0 ~ 1


```python
num = []
for i in range(10000):
    n = random.random()
    num.append(n)

sns.distplot(num)
plt.show()
```


![PNG](/assets/img/post_img/2019-12-16-python_std_library/output_45_0.png)


### random.uniform()
- Uniform Distribution - Range : Variables


```python
num = []
for i in range(10000):
    n = random.uniform(-1, 1) # -1 ~ 1 사이의 난수값 생성
    num.append(n)

sns.distplot(num)
plt.show()
```


![PNG](/assets/img/post_img/2019-12-16-python_std_library/output_47_0.png)


### randint()

- 정수 난수값 생성


```python
num = []
for i in range(100000):
    n = random.randint(-10, 10)
    num.append(n)

sns.distplot(num)
plt.show()
```


![PNG](/assets/img/post_img/2019-12-16-python_std_library/output_49_0.png)


### randrange()


```python
num = []
for i in range(100000):
    n = random.randrange(0, 20, 2)  # 0 ~ 20 step:2
    num.append(n)

sns.distplot(num)
plt.show()
```


![PNG](/assets/img/post_img/2019-12-16-python_std_library/output_51_0.png)


## datetime


```python
import datetime
```

### datetime.datetime.strptime()

- 문자열 날짜를 datetime으로 변환


```python
a = '20191220'
b = datetime.datetime.strptime(a, '%Y%m%d')

print(type(b))
print(b)
```

    <class 'datetime.datetime'>
    2019-12-20 00:00:00


### datetime.datetime.strftime()

- datetime 날짜를 문자열로 변환


```python
c = datetime.datetime.strftime(b, '%m/%d/%Y')
d = datetime.datetime.strftime(b, '%Y-%m-%d')

print(type(c))
print(c)

print(type(d))
print(d)
```

    <class 'str'>
    12/20/2019
    <class 'str'>
    2019-12-20
