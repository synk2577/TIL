# Django Server 실행

현재 프로젝트 구조!!

```
/ssac-django
  /first_homepage [프로젝트 폴더] 🔥
    /first_homepage [앱 폴더 - default]
      settings.py 
      urls.py -> 최상단 접속주소
    manage.py
    /member [앱 폴더]
      urls.py -> 앱 별 접속 경로 (new!!)
      views.py   
      /templates
        aa.html 
```

## Routing 설정 & 동작과정 

1. `/ssac-django/first_homepage/first_homepage/urls.py` 파일을 먼저 읽음 (**최상단 접속주소**이므로!!)

    ```python
    (...)

    urlpatterns = [
      path('admin/', admin.site.urls),
      path('dontgiveup/', include('member.urls'))
    ]
    ```

    ⇒ `~/dontgiveup/`에 이어지는 주소를 찾기 위해 **member.urls 파일**을 찾음

<br>

2. `/ssac-django/first_homepage/member/urls.py` 파일을 읽음

    ```python
    (...)

    urlpatterns = [
      path('ok', views.hello)
    ]
    ```

    ⇒ urlpatterns 변수를 읽어서 `~dontgiveup/ok`로 주소 연결! 이 주소로 이동시 **views.hello를 참조**함

    [views.py](http://views.py) 파일에서 hello 함수를 읽음 

<br>

3. `/ssac-django/first_homepage/member/views.py` 파일을 수정

    ```python
    (...) 

    def hello(req):
      c = random.randint(1, 20)
      
      return render(req, 'aa.html', {'param1': c})
    ```

    ⇒ render 함수에서 req변수로 요청을 받아 aa.html 파일을 보여주도록 함! + aa.html 파일에 param1변수값 전달

    
    ```html
    <html>
      <head>
        <title>hi<title>
      </head>
      <body>
        {{ param1 }}
      </body>
    </html>
    ```
    ⇒ ~/member/templates/aa.html에서 **동적으로 값이 호출**됨
    




















