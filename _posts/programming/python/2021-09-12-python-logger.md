---
title: "Python Custom Logger 만들기"
categories: python
tags: Logger
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## 기본 logging 모듈 이용하기

레벨  | 이름 | 비고
:-------|:------|:-------
1 | DEBUG | 
2 | INFO | 
3 | WARNING | 기본 설정
4 | ERROR | 
5 | CRITICAL | 

- 기본적으로 `INFO` 레벨 보다 중요도가 높은 레벨을 출력


```python
# 로깅 모듈 불러오기
import logging

# 로거 생성
logger = logging.getLogger("test")

# 레벨 설정 - 'INFO' 레벨부터 출력
logger.setLevel(logging.INFO)

# 출력 포매팅 설정 - 시간, 로거이름, 로깅레벨, 메세지
formatter = logging.Formatter("%(asctime)s - %(name)s - %(levelname)s - %(message)s")

# 스트림 핸들러 설정 - 콘솔에 출력
stream_handler = logging.StreamHandler()
stream_handler.setFormatter(formatter)
logger.addHandler(stream_handler)

# 파일 핸들러 설정 - 파일에 출력
file_handler = logging.FileHandler('test.log')
file_handler.setFormatter(formatter)
logger.addHandler(file_handler)
```


```python
logger.info("INFO 레벨 로깅 문구 출력 테스트")
```

    2021-09-13 17:44:05,283 - test - INFO - INFO 레벨 로깅 문구 출력 테스트
    


```python
logger.error("ERROR 레벨 로깅 문구 출력 테스트")
```

    2021-09-13 17:44:05,483 - test - ERROR - ERROR 레벨 로깅 문구 출력 테스트
    


```python
logger.debug("DEBUG 레벨 로깅 문구 출력 테스트")
```

<br>

## 커스텀 로거 만들기


```python
from logging import handlers
import logging
import colorlog
import os
import sys
```


```python
class Logger:
    """
    커스텀 로거 클래스
    """
    def __init__(self, filePath=None):
        
        # 로거 이름
        self.className = "Logger"
        
        # 로그 파일 생성 경로
        if filePath is None:
            self.filePath = "./log/"

        # 로그 파일 생성 경로 부재 시 생성
        if not os.path.exists(self.filePath):
            os.makedirs(self.filePath)

        
    def initLogger(self):
        """
        로거 인스턴스 반환
        """
        # 로거 인스턴스 생성
        __logger = logging.getLogger("Logger")

        #=====================================================
        # 포매터 설정
        #=====================================================
        # 스트림 핸들러 포매터 설정
        streamFormatter = colorlog.ColoredFormatter(
            "%(log_color)s[%(levelname)-8s]%(reset)s <%(name)s>: %(module)s:%(lineno)d:  %(bg_blue)s%(message)s"
        )
        # 파일 핸들러 포매터 설정
        fileFormatter = logging.Formatter(
            "%(asctime)s [%(levelname)-8s] <%(name)s>: %(module)s:%(lineno)d: %(message)s"
        )
        #=====================================================
        # 핸들러 설정
        #=====================================================
        # 스트림 핸들러 정의
        streamHandler = colorlog.StreamHandler(sys.stdout)
        
        # 파일 핸들러 정의
        fileHandler = handlers.TimedRotatingFileHandler(
            os.path.abspath(f"{self.filePath}logData.log"),
            when="midnight",
            interval=1,
            backupCount=14,
            encoding="utf-8",
        )
        #=====================================================
        # 핸들러에 포매터 지정
        #=====================================================
        streamHandler.setFormatter(streamFormatter)
        fileHandler.setFormatter(fileFormatter)

        # 로거 인스턴스에 핸들러 삽입
        __logger.addHandler(streamHandler)
        __logger.addHandler(fileHandler)

        # 로그 레벨 정의
        __logger.setLevel(logging.DEBUG)

        return __logger
```

<br>

## 커스텀 로거 적용하기


```python
# 커스텀 로거 정의
logger = Logger()

# 로거 초기화
testLogger = logger.initLogger()
```


```python
# DEBUG 레벨 로깅 테스트
testLogger.debug("Text")
```

    [37m[DEBUG   ][0m <Logger>: <ipython-input-9-8b929ff392bd>:1:  [44mText[0m
    


```python
# INFO 레벨 로깅 테스트
testLogger.info("Text")
```

    [32m[INFO    ][0m <Logger>: <ipython-input-10-00e367f818bf>:1:  [44mText[0m
    


```python
# WARNING 레벨 로깅 테스트
testLogger.warning("Text")
```

    [33m[WARNING ][0m <Logger>: <ipython-input-11-b21b75470bde>:1:  [44mText[0m
    


```python
# ERROR 레벨 로깅 테스트
testLogger.error("Text")
```

    [31m[ERROR   ][0m <Logger>: <ipython-input-12-35d8986e5471>:1:  [44mText[0m
    


```python
# CRITICAL 레벨 로깅 테스트
testLogger.critical("Text")
```

    [01;31m[CRITICAL][0m <Logger>: <ipython-input-13-4c135fee5a22>:1:  [44mText[0m
    
