---
categories: etc
---

# 환경 설정

## conda 설치

[Anaconda | The World's Most Popular Data Science Platform](https://www.anaconda.com/)

## conda 환경 변수 설정

시스템 속성 > 고급 시스템 설정 > 환경 변수 > 시스템 변수 > path

`C:\Users\user\anaconda3`

`C:\Users\user\anaconda3\Library`

`C:\Users\user\anaconda3\Scripts`

추가

## conda 가상환경 만들기

```powershell
$conda create -n "가상환경명" python=3.7
```

## conda 그외 명령어

```powershell
# 생성된 가상환경들 확인
$conda env list

# SSL 인증 무시
$conda config --set ssl_verify no
```

## vscode에서 conda 가상환경 실행

`ctrl + shift + p`

python 인터프리터 선택

생성한 가상환경 선택

- powershell 이 아닌, cmd 창에서 작성하기

```powershell
$conda activate "가상환경명"
```

## 패키지 설치

```powershell
$pip install selenium webdriver_manager pymysql
$pip freeze > requirements.txt
```
