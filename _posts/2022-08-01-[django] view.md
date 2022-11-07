---
categories: django
---

`뷰 추가하기`

- HttpResponse 객체로 반환
- 혹은 Http404 예외로 반환
- 뷰 안에는 python의 어떤 라이브러리도 사용가능 (예: zip파일, xml출력, pdf 생성 등)

`template index.html 사용`

- settings 안에 APP_DIRS 옵션 True
- DjangoTeplates → INSTALLED_APPS 아래 templates 탐색
- polls/templates/polls/index.html

```python
# views.py
from django.http import HttpResponse
from django.template. import loader
from .models import Question

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {
        'latest_question_list': latest_question_list,
    }
		# 앞에 latest_question_list는 index.html에서 쓰일 변수
    return HttpResponse(template.render(context, request))
```

```python
# views.py (render)
# context를 HttpResponse로 넘기는 것은 자주 사용
# from django.shortcuts import render 사용

from django.shortcuts import render
from .models import Question

def detail(request):
	latest_question_list = Question.objects.order_by('-pub_date')[:5]
	context = {'latest_question_list' : latest_question_list}
	return render(request, 'polls/index.html', context)

```

```python
# vies.py (get_object_or_404)
# Http404 예외시키는 방법 간단하게 하기

from django.shortcuts import render, get_object_or_404
from .models import Question

def detail(request, question_id):
	question = get_object_or_404(Question, pk=question_id)
	return render(request, 'polls/detail.html', {'question' : question})

```

`같은 뷰네임을 구분하기`

```python
# urls.py

from django.urls import path
from . import views

# app_name을 추가
app_name = 'polls'
urlpatterns = [
		path('', views.index, name='index'),
    path('<int:question_id>/', views.detail, name='detail'),
    path('<int:question_id>/results/', views.results, name='results'),
    path('<int:question_id>/vote/', views.vote, name='vote'),
]

# index.html
<li><a href="{% raw %}{% url 'polls:detail' question.id %}">{{ question.question_text }} {% endraw %}</a></li>
# polls: 를 추가

```

`HttpResponseRedirect`

- POST 데이터를 성공적으로 처리 한 후에는 항상
- HttpResponseRedirect를 반환
