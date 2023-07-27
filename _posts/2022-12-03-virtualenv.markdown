---
title:  "[python] 가상환경 -  virtualenv 사용"
tag: 
  - python
  - 가상환경 
  - virtualenv
---

### 설치 및 사용
- 가상환경 모듈 virtualenv를 설치

```
# pip 에 virtualenv 설치
pip install virtualenv 
```

- 모듈 설치 후 사용하기

```
# venv 이름으로 가상환경 생성
virtualenv venv

# venv 이름으로 python 2 설치
virtualenv venv --python=python2.7

# venv 이름으로 python 3 설치
virtualenv venv --python=python3.6

# 특정 경로에 venv 가상환경 설치하기
virtualenv /home/user/venv --python=python3.6
```

- 가상환경 활성 및 비활성화 하기

```
# 가상환경이 만들어진 위치에서 bin/activate를 입력해주어야합니다.
cd
source venv/bin/activate

# 개인적으로 경로를 지정했다면 해당경로로 이동한 후 bin/activate를 입력해줍니다.
cd [생성경로]
source bin/activate
```
