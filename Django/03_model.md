# Model of MTV Pattern

### Model 생성
member > models.py에 모델 생성
```python
from django.db import models

# Create your models here.
# ORM : SQL을 직접 작성하지 않아도 DB로 접근해 CRUD 가능하게 함

# Table Name : 명사형 단수
class User(models.Model):
    userid = models.CharField(max_length=64, verbose_name='아이디')
    username = models.CharField(max_length=64, verbose_name='사용자명')
    password = models.CharField(max_length=64, verbose_name='비밀번호')
    registered = models.DateTimeField(auto_now_add=True, verbose_name='등록')
    GENDERS = (('M','남성(Man)'), ('W', '여성(Woman)')) # tuple 
    gender = models.CharField(max_length=1, verbose_name='성별', choices=GENDERS)
    
    # models.데이터타입(속성1, 속성2, ...)

    # DataFields
    # CharField: 문자열필드
    # DateTimeField auto_now_add = True: add할 때의 시간이 들어감 
        # 0000-00-00 00:00:00 datetime
    # TextField
    # IntegerField

    # 속성 
    # max_length: 최대 길이
    # verbose_name: 별칭 (관리자 페이지에서 보임)
    # null: default값 False
    # default
```

<br>

### Migration?
model의 변경 사항을 DB Schema에 적용하는 방법


### Migration Commands
- `python3 manage.py makemigrations [app_name]` : 마이그레이션 생성       
  → 변경사항이 마이그레이션 파일로 저장됨        
  → DB 형상관리에 대한 **보고서 작성 -> 실제 DB에 반영되지 않은 상태**        
- `python3 manage.py migrate [app_name]` : 마이그레이션 적용        
  → **실제 DB에 반영**
- `python3 manage.py showmigrations` : 마이그레이션 내역 (히스토리) 확인    

⇒ **변경사항이 있을 때마다 마이그레이션 적용**해야 함!! 

<br>

### Admin Page 
member > admin.py 에서 User 모델 등록
```python
from django.contrib import admin
from .models import User
# Register your models here.

admin.site.register(User) # User Model을 관리자 사이트에 등록 
```

서버 실행 후, 브라우저에서 관리자 페이지 (`주소/admin`) 접속     
![image](https://user-images.githubusercontent.com/55572222/130225886-62a8bb35-86ef-43f6-8772-e68c8ac5cff3.png)       

`python3 manage.py createsuperuser`: superuser 생성해 관리자창 로그인     
![image](https://user-images.githubusercontent.com/55572222/130226186-df9a480e-363c-411d-89ec-6b9e478942b9.png)   

User 생성/수정/삭제     
![image](https://user-images.githubusercontent.com/55572222/130226047-9c5e29c3-305b-4bd8-8aa4-921ebafa5b57.png)
![image](https://user-images.githubusercontent.com/55572222/130226071-d4bf2315-08c7-40ba-9554-7820f8211c80.png)



