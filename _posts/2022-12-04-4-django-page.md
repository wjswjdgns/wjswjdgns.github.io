---
title:  "[python] Django 사용하기 4편 [페이지 만들기]"

tag: 
  - python
  - django
---

### 템플릿 설정하기

- html 템플릿 만들기

```

--> / config / settings.py   

(... 생략 ...)
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',

        # c:\project\mysite 안에 templates 디렉토리를 확인 하라는 의미
        'DIRS': [BASE_DIR / 'templates'],


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

- templates 디렉토리 생성
- 해당 디렉토리 안에 페이지 구분 별로 디렉토리를 추가로 생성한다.
- 해당 디렉토리 안에서 html 파일을 생성한다. 


---

### django 페이지 만들기 

```
--> pybo / views.py

from django.shortcuts import render
from .models import Question


def index(request):
    question_list = Question.objects.order_by('-create_date')
    context = {'question_list': question_list}
    return render(request, 'pybo/question_list.html', context)

```



- views를 통해 받아온 변수를 html에서 추가적인 조건문으로 원하는 형식으로 노출

```
{% if question_list %}
    <ul>
    {% for question in question_list %}
        <li><a href="/pybo/{{ question.id }}/">{{ question.subject }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>질문이 없습니다.</p>
{% endif %}

```

```
{% if question_list %} # question_list 가 있다면
{% for question in question_list %} # questin_list 를 순차적으로 하나씩 question에 대입
{{ question.id }} # for 문에 대입 된 question 객체의 id 번호를 출력
{{ question.subject }} #for 문에 의해 대입된 question 객체의 제목을 출력

```



### 탬플릿 태그 유형

1. 분기 

```
{% if 조건문1 %}
    <p>조건문1에 해당되는 경우</p>
{% elif 조건문2 %}
    <p>조건문2에 해당되는 경우</p>
{% else %}
    <p>조건문1, 2에 모두 해당되지 않는 경우</p>
{% endif %}
```

2. 반복
- forloop.counter : 루프내의 순서로 1부터 표시
- forloop.counter() : 루프내의 순서로 0부터 표시
- forloop.first : 루프의 첫번째 순서인 경우 True
- forloop.last : 루프의 마지막 순서인 경우 True

```
{% for item in list %}
    <p>순서: {{ forloop.counter }} </p>
    <p>{{ item }}</p>
{% endfor %}

```

3. 객체 출력

```
{{ 객체 }} # 객체 출력
{{ 객체.속성 }} #객체 안에 속성 값 출력
```


### 페이지에 데이터를 가지고 페이지 추가하기

1. urls.py 추가

```
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index),

# <int:question_id> 를 추가하여 pybo/2 url에 id값이 추가가 된다.
# 이러한 방법으로 데이터를 전달하는 것은 get 형식으로 생각하면 된다. 

    path('<int:question_id>/', views.detail),
]
```

2. views.py 추가

데이터를 불러오는 과정으로 생각하면 된다.

```
(... 생략 ...)

def detail(request, question_id):
    question = Question.objects.get(id=question_id)
    context = {'question': question}
    return render(request, 'pybo/question_detail.html', context)

```

3. question_detail.html 만들기

화면을 노출 시킬 프론트 단 완성 시키기

```
<h1>{{ question.subject }}</h1>
<div>
    {{ question.content }}
</div>
```


### 오류 처리하기

- 설정하기 전에는 500 에러를 발생 시키지만 페이지가 없는 경우는 404 에러를 발생시키는 것이 맞다

```
views.py

#get_object_or_404 추가
from django.shortcuts import render, get_object_or_404

# get_object_or_404(Question, pk=question_id) 형식으로 변경
# 무조건 받아오는 것에서 404에 대한 분기 처리를 했다고 이해하면 편함

question = Question.objects.get(id=question_id)
question = get_object_or_404(Question, pk=question_id)

```
