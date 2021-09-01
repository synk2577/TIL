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
    

<br />

## Routing - url에 따라 내용이 달라지는 페이지

현재 프로젝트 구조!!

```
/ssac-django
	/first_homepage [프로젝트 폴더] 
		manage.py
		/first_homepage [앱 폴더]
			settings.py 
			urls.py (최상단 url관리 파일) -> dontgiveup
		/member [앱 폴더]
			/templates
				c.html		
			urls.py -> novel/숫자/문자/문자
			views.py 
```

1. `member/urls.py` 에서 path()함수로 주소 추가  
    앞선 상황과 달리 url에 매번 다른 값을 입력 받음 
    
    ```python 
    from django.urls import path
    from . import views # .(현재위치- member 앱폴더)에서 views.py 열기 

    urlpatterns = [
        path('ok', views.hello),
        path('recieve', views.rec),
        path('send', views.send),
        path('novel/<int:chapter>/<str:player1>/<str:player2>', views.novel), # 주소로 무언가를 입력받을 수 있음 
    ]
    ```
2. `member/views.py`에 새로운 뷰 추가
    ```python
    from django.shortcuts import render
    import random 

    (...)

    def novel(req, chapter, player1, player2): # 인수를 받음!! 
        return render(req, 'c.html', {'c': chapter, 'p1': player1, 'p2': player2})
    ```

3. 템플릿에 새로 추가한 뷰의 파라미터를 이용해 코드 작성
    ```html
    이것은 소설 {{c}}화입니다. <br />
    이것은 주인공인 {{p1}}, {{p2}}의 이야기
    ```
    ![image](https://user-images.githubusercontent.com/55572222/131691159-1e0f630e-a3c9-4f9c-a698-7a584633de75.png)

















