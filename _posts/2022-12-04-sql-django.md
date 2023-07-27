---
title:  "[python] Django 사용하기 2편 [데이터베이스]"
tag: 
  - python
  - django
  - sql
---

### 데이터 베이스 만들기

- 데이터 베이스 만들기

```
# 기본 요소
python manage.py migrate # 기본 데이터 베이스 만들기
python manage.py makemigrations  # 모델을 가지고 새로운 데이터 입력하기
```

```
# 모델 만들기
# models.py

from django.db import models


class Question(models.Model):
    subject = models.CharField(max_length=200) #subject라는 최대 200 길이의 텍스트 컬럼 
    content = models.TextField() # content 라는 텍스트 컬럼
    create_date = models.DateTimeField() # create_date 라는 날짜 컬럼


class Answer(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE) # question 이라는 외래키? 
    content = models.TextField() # content 라는 텍스트 컬럼
    create_date = models.DateTimeField() # create_date 라는 날짜 컬럼


```

```
# 작성한 모델을 가지고 테이블 생성하기

#이제 작성한 모델을 이용하여 테이블을 생성해 보자. 테이블 생성을 위해 가장 먼저 해야 할 일은 pybo 앱을 config/settings.py 파일의 INSTALLED_APPS 항목에 추가하는 일이다.

(... 생략 ...)
INSTALLED_APPS = [
    'pybo.apps.PyboConfig', # 추가
    'django.contrib.admin',
    'django.contrib.auth',
    (... 생략 ...)
]
(... 생략 ...)


# 새롭게 만들었을 경우 makemigrations를 해줘야 한다.
1. (mysite) c:\projects\mysite> python manage.py makemigrations 
2. (mysite) c:\projects\mysite> python manage.py migrate


[비고]
# sqlmigrate 진행 시 실제로 어떤 쿼리가 실행 되는지 알 수 있다
(mysite) c:\projects\mysite> python manage.py sqlmigrate pybo 0001
```


### Django shell 및 jupyter notebook 활용하기

- 장고 프로젝트 설정이 로딩된 파이썬 쉘
- 일반 파이썬 쉘을 통해서는 장고 프로젝트 환경에 접근 불가
- 프로젝트 내의 각종 모듈 패키지를 활용하기 위해서는 장고 shell를 통해서 접근해야한다.

**장고 쉘 실행방법**

```
$ python3 manage.py shell

$ python3 manage.py shell_plus # django extentions 설치 후 사용 가능
```


**juptyter notebook 활용**

```
1. $ pip install django-extensions # django 확장팩 설치

2. settings.py의 INSTALLED_APPS에 아래 내용을 추가

INSTALLED_APPS = [
'django_extensions',
] 


3. pip3 install "ipython[notebook]" #ipython 및 jupyternotebook 설치

4. python manage.py shell_plus --notebook # 실행 시키기
```

### 오류

```
Django 3.0은 asgi / async 지원 기능을 추가 하고 비동기식 컨텍스트에서 동기식 요청을하는 것을 막고 있습니다. 동시에 IPython은 최상위 이벤트 async / await support를 추가 하여 기본 이벤트 루프 내에서 전체 인터프리터 세션을 실행하는 것으로 보입니다.

불행히도이 두 가지 추가 기능의 조합은 jupyter 노트북에서 장고 ORM 작업이 SynchronousOnlyOperation예외를 발생 시킨다는 것을 의미합

이를 해결한 것이

import os
os.environ["DJANGO_ALLOW_ASYNC_UNSAFE"] = "true"

```


### 데이터 베이스 조작하기

**메인데이터 추가하기**

```

# [변수] = [테이블명] ( [컬럼1] = [내용], [컬럼2] = [내용], [컬럼3] = [내용] )

q = Question(subject = 'Django란 무엇인가?' , content= '장고에 대해서 알고 싶다', create_date=timezone.now())

q.save() # 데이터 베이스에 저장

```



**메인데이터 조회하기**

```

Question.objects.all() 


(... 생략 ...)

class Question(models.Model):
    subject = models.CharField(max_length=200)
    content = models.TextField()
    create_date = models.DateTimeField()

	# 이 부분을 추가 함으로써 id 값 대신 제목을 표시 할 수 있다.
    def __str__(self):
        return self.subject 

(... 생략 ...)


# 조건 검색
Question.objects.filter(id=1) #filter를 통해서 조건 모두 리턴 (QuerySet 리턴)
Question.objects.get(id=1) #get을 통해서 유일 값 리턴 (한건만 리턴)


```

**메인데이터 수정하기**

- 데이터를 불러서 변수에 넣은 다음 "변수.컬럼명 = 변경할 내용" 형식으로 저장 후 데이터를 Save 한다. 

```
q = Question.objects.get(id=2)
q.subject = "Django Model Question"
q.save()
```

**메인데이터 삭제하기**

```
q = Question.objects.get(id =1)
q.delete()
```


**서브데이터 작성하기 (외래키)**
- 메인 데이터를 불러오고 해당 데이터를 연결해서 답변을 작성해서 등록 한다.
- [변수] = [테이블명] ([외래키 지정 key] = [불러온 메인 데이터] , [컬럼 2] = [ 내용] , [컬럼3] = [내용] 

```
q.Question.objects.get(id =2)
from django.utils import timezone
a = Answer(question = q, content = "네 자동으로 생성하빈다", create_date=timezone.now())
a. save
```

**서브데이터 조회하기**

- 답변을 조회하는 방법은 질문과 마찬가지로 Answer의 id 값을 사용하면 된다.

```
a = Answer.object.get(id =1) 

a.question # 외래키 컬럼을 불러오는 것으로 연결 되어 있는 메인 데이터도 확인이 가능하다


# 질문을 통해서 답변들 가져오기
q.answer_set.all() #q.[연결모델명]_set 형식으로 생각하면 된다. 
```
