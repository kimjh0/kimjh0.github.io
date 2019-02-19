---
title: "Python exe 패키지 제작"
tags:
  - Python
---

## 개요
- Python 으로 개발한 프로그램을 배포하기 위함
- 임의의 윈도우 컴퓨터에서 exe 파일을 통해 Python 코드를 실행

## PyInstaller 설치
pip 를 이용하여 설치

```bash
$ python -m pip install pyinstaller
```

## exe 패키지 제작
### PyInstaller 파라미터

| 파라미터 | 설명 |
|--|--|
|-F|하나의 파일로 제작. 프로그램 실행시마다 자동으로 압축을 해제함 -> 성능이 떨어짐|
|-w|실행시 cmd 창을 표시하지 않음. *nix 에서는 무시|
|-i|아이콘 적용|
|--uac-admin|관리자 권한 실행|
추가 설명 [https://pyinstaller.readthedocs.io/en/stable/usage.html]

### 제작

```bash
$ pyinstaller script.py
```

성공적으로 제작되면 dist 폴더에 exe 패키지가 생성됨, 해당 디렉토리 또는 파일을 배포
또한 설치 환경(또는 Virtual Machine)에서 검증 해볼것

## 이슈사항
### Support statical linking of Python library

https://github.com/pyinstaller/pyinstaller/issues/420
Python 3.7 64bit, venv 환경에서 PyInstaller 사용시 에러 발생

#### 원인

- PyInstaller의 패키지 생성 과정에서 python dll 경로가 필요
- python dll 경로는 실행중인 python.exe 의 PE 정보에서 찾음
- Python 3.7 64bit, venv 의 python.exe 가 static 하게 빌드 되어 있음 (python dll 을 참조 하지 않음)
- python dll 경로를 찾지 못 해 에러 발생
- *nix 는 ldd 로 찾음

#### 해결
개발 환경 변경
- Python 3.7 32bit 문제 없음
- Python 3.6 64bit 문제 없음

### UAC not working in onefile again, migrate old UAC test
https://github.com/pyinstaller/pyinstaller/issues/1729
PyInstaller 의 onefile 옵션 사용시 UAC 동작 안함

원인 분석 및 해결 안 함
