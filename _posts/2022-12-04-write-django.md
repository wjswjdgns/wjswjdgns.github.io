---
title:  "[python] Django 사용하기 4편 [페이지]"

tag: 
  - python
  - django
---

### 페이지 만들기

- views.py 는 데이터를 정제하는 곳이라고 생각을 하면 편함

```
--> views.py 에서 

from django.shortcuts import render
from .models import Question


def index(request):
    question_list = Question.objects.order_by('-create_date') # 있으면 역방향 없으면 순방향
    context = {'question_list': question_list}
    return render(request, 'pybo/question_list.html', context)

```

### 템플릿 만들기

- 별도로 관리를 하기 위해 templates 를 만든다.

```

(... 생략 ...)
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'], # c:\projects\mysite 안에 templates를 바라보라는 의미
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
(... 생략 ...)

```

1. templates 생성
2. pybo 생성
3. templates > pybo에 html 파일 생성


### 화면 만들기

<img width="688" alt="image" src="https://user-images.githubusercontent.com/55444587/205487868-6fbc0d96-8379-4a96-a47d-544e6a45e6ee.png">

<img width="724" alt="image" src="https://user-images.githubusercontent.com/55444587/205487881-315b031c-2be6-47b7-a1a9-93d830a8cad9.png">


### 탬플릿 태그
- 장고에서 사용하는 탬플릿 태그는 3가지 유형만 알고 있으면 된다.

1. 분기

<img width="688" alt="image" src="https://user-images.githubusercontent.com/55444587/205487750-58576604-4afe-495d-be45-211b5b99bf7d.png">


2. 반복

<img width="690" alt="image" src="https://user-images.githubusercontent.com/55444587/205487765-827a4502-4b90-4713-a811-58455b8cb577.png">

<img width="383" alt="image" src="https://user-images.githubusercontent.com/55444587/205487896-58452f8f-2311-4334-a356-b32e047b7096.png">



3. 객체 출력

<img width="687" alt="image" src="https://user-images.githubusercontent.com/55444587/205487918-8d503975-40d2-4882-a3cc-6b1ed67b2d71.png">

객체에 속성이 있는 경우는 파이썬과 동일한 방법으로 도트 (.) 문자를 이용하여 표시하면 된다.

<img width="688" alt="image" src="https://user-images.githubusercontent.com/55444587/205487929-da3790f8-bbe0-4cb8-9d90-edb39f81459a.png">


### 상세화면 만들기

1. url에 정보 보내기 (get)

```
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index),

# <int : question_id> 를 추가하게 되면 url에 추가 된다.
# 이러한 과정을 get 방식으로 보낸다고 한다. 

    path('<int:question_id>/', views.detail), 
]

```

2. views 에서 데이터 정제하기 

```

(... 생략 ...)

def detail(request, question_id):
    question = Question.objects.get(id=question_id)
    context = {'question': question}
    return render(request, 'pybo/question_detail.html', context)

```

3. 데이터를 정제하여 보낸 페이지 question_detail.html 를 제작한다.

<img width="687" alt="image" src="https://user-images.githubusercontent.com/55444587/205488052-d18e6c20-48a7-449e-a4d8-ece4d014ef69.png">



### 오류 페이지 처리

기본적으로 django 에서는 페이지가 없으면 오류가 발생하여 서버 오류 (500) 를 보내주게 되는데 기본적으로 페이지가 없는 경우에는 페이지가 없다는 404에러를 보내줘야 한다.


```
from django.shortcuts import render, get_object_or_404
from .models import Question

(... 생략 ...)

def detail(request, question_id):

# question = get_object_or_404(Question, pk=question_id)는 있으면 데이터를 보내주고 없으면 404 에러를 발생 시킨다는 의미이다. 

    question = get_object_or_404(Question, pk=question_id)
    context = {'question': question}
    return render(request, 'pybo/question_detail.html', context)

```

## P.S
- git page로 작업하는데 html 내부의 django 태그를 사용할 경우 업로드에 에러가 나는데 정말... 답답
- elif 에러 (업로드 안됨)
- for 문 에러 (업로드 안됨)
- <h1> </h1> 에러  (내부 텍스트가 안보임)
- <div> </div> 에러 (내부 텍스트가 안보임)
