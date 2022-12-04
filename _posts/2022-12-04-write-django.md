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
# question_list 가 있다면
{% if question_list %}

# question_list를 순회하며 순차적으로 하나씩 question에 대입
{% for question in question_list %} 

# for 문에 의해 대입된 question 객체의 id 번호를 출력
{{ question.id }} 

# for문에 의해 대입된 question 객체의 제목을 출력
{{ question.subject }}	 
```


### 탬플릿 태그
- 장고에서 사용하는 탬플릿 태그는 3가지 유형만 알고 있으면 된다.

1. 분기

<pre>

{% if 조건문1 %}
    <p>조건문1에 해당되는 경우</p>
{% elif 조건문2 %}
    <p>조건문2에 해당되는 경우</p>
{% else %}
    <p>조건문1, 2에 모두 해당되지 않는 경우</p>
{% endif %}

</pre>


2. 반복

<pre>
  
{% for item in list %}
    <p>순서: {{ forloop.counter }} </p>
    <p>{{ item }}</p>
{% endfor %}
  
</pre>

```
# 루프내의 순서로 1부터 표시
forloop.counter	

# 루프내의 순서로 0부터 표시
forloop.counter0	

# 루프의 첫번째 순서인 경우 True
forloop.first	

# 루프의 마지막 순서인 경우 True
forloop.last	

```


3. 객체 출력

<pre>

{{ 객체 }} #객체 출력
{{ 객체.속성 }} # 객체 속성 값 출력

</pre>


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

```
<h1>{{ question.subject }}</h1>
<div>
    {{ question.content }}
</div>

```


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



