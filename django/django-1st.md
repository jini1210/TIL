# django(장고)에서 패턴(교재 142 p.)



Model   			Controller        	View = MVC   => 웹

models.py		views.py		templates = MTV   => 장고



user ---------------------------> chrome

​             Controller

​          1. views.py에 요청      -----------------------------------> 2. models.py에 값을 넘겨주고

​                                                                                            

​           4. views.py가 다시 받음 <--------------------------------------- 3. View,Templates가 받아서





### <u>**★★★위의 내용은 반드시 숙지하고 있어야 함(셋트로 묶어서 기억할 것)★★★**</u>



http://127.0.0.1:8000/admin   ----> django 관리자 모드





#application 생성

$ python manage.py startapp my_to_do_app

(앱 생성후 아래내용을 추가한다.)

### ★d/mybiz/bigdata/ToDoList-with-Django/myproject/settings.py에서 코드변경★

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'my_to_do_app'   # app을 추가했기 때문에 앱의 이름을 가장 아랫줄에 넣어준다.
```

### myproject/urls.py의 내용을 수정한다

```python
from django.contrib import admin
from django.urls import path,include    #include 써준다

urlpatterns = [
    path('',include('my_to_do_app.urls')),  #추가해준다
    path('admin/', admin.site.urls),
```

### my_to_do_app/urls.py 생성 후 내용을 적는다

```python
from django.urls import path
from . import views

urlpatterns=[
    path('',views.index)
]
```

### my_to_do_app/views.py에 내용을 적는다

```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
# index함수를 실행해서 함수결과를 client에 넘겨준다.
def index(request):
    return HttpResponse('my_to_do_app first page')
```

#http://127.0.0.1:8000에 새로 작성한 내용을 적용시켜준다.(서버 작동)

$python manage.py runserver 



### Html파일을 클라이언트에 응답해줄 때 방법

#my_to_do_app에서 ``templates``폴더 생성 후 하위폴더로 ``my_to_do_app``폴더 생성

#앞단계에서 생성한 ``my_to_do_app``폴더에 ``index.html``을 생성해 구현한다.

``views.py``에서 ``render``함수를 이용해 ``index.html``을 불러올수 있도록 한다.(아래내용)

```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def index(request):
    #return HttpResponse('my_to_do_app first page')
    return render(request,'my_to_do_app/index.html')    #요청이 오면 이 경로를 응답하겠다.
```

### django에서는 table을 model로 표현하고 model을 class으로 정의한다.

테이블 표현 소스

```python
from django.db import models

# Create your models here.
class Todo(models.Model):
    content=models.CharField(max_length=255)    #datatype,문자열의 최대길이를 나타내줌
```

#서버 활성화

$ conda activate ToDoList

#테이블 생성하기

 $ python manage.py makemigrations

#db에 migrations파일 적용하기

$ python manage.py migrate

- DB 진입방법(기본적으로 SQLite가 제공된다 : 웹에 사용되는 db임,가벼워서)

```bash
sqlite> python manage.py dbshell
SQLite version 3.35.4 2021-04-02 15:20:15
Enter ".help" for usage hints.
sqlite> .tables
auth_group                  django_admin_log
auth_group_permissions      django_content_type
auth_permission             django_migrations
auth_user                   django_session
auth_user_groups            my_to_do_app_todo
auth_user_user_permissions
```
- my_to_do_app_todo 테이블 정보확인하기

```bash
sqlite> pragma table_info(my_to_do_app_todo);
0|id|integer|1||1
1|content|varchar(255)|1||0
```

뒷부분 |1| |1

​             |1| |0

| Null 허용하지 않으면 1 | primary key 1        |
| ---------------------- | -------------------- |
| Null 허용하면 0        | primary key 아니면 0 |

순서 | 이름 | 형태 | not null여부 | pk여부

- table의 데이터 검색

```bash
sqlite> SELECT * FROM my_to_do_app_todo;
```

- sqlite종료하기

```bash
sqlite> .quit
(ToDoList) 
jini8@DESKTOP-RUKK1GT MINGW64 /d/mybiz/bigdata/ToDoList-with-Django/myproject
$
```

