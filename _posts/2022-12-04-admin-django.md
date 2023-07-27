---
title:  "[python] Django 사용하기 3편 [관리자]"

tag: 
  - python
  - django
---

- Django는 관리자를 만드는 것이 매우 편리하다
- 주의할 점은 admin이라는 이름으로 주소에 포함하는 것을 주의 하도록 한다.

### 장고 관리자

- 슈퍼유저 만들기

```
python manage.py creaetesuperuser

사용자 이름 (leave blank to use 'pahke'): admin
이메일 주소: admin@mysite.com
Password: 
Password (again):

```


- 모델 관리

1. admin.site.register로 Question 모델 등록
2. 이후 장고관리자 화면에서 Question을 사용할 수 있다.

```
--> pybo / admin.py

from django.contrib import admin
from .models import Question

admin.site.register(Question) 

```


- 모델 검색

```
--> pybo / admin.py

from django.contrib import admin
from .models import Question

class QuestionAdmin(admin.ModelAdmin):
	search_fields = ['subject']

admin.site.register(Question, QuestionAdmin)

```


