# CRUD
Create, Read, Update, Delete



## Insert
`.save()`

```python
# models.py
from django.db imoprt models

class Person(models.Model):
  SHIRT_SIZES = (
    ('S', 'Small'),
    ('M', 'Medium'),
    ('L', 'Large'),
  )
  nane = models.CharField(max_length=60)
  shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```

```python
# views.py
p = Person(name="Fred Flintstone", shirt_sze="L)
p.save()
```

🔗[model 관련 공식 문서](https://docs.djangoproject.com/ko/3.2/topics/db/models/)     
🔗[crud 명령어 관련 공식 문서](https://docs.djangoproject.com/ko/3.2/topics/db/queries/)

```html
<!-- Django Template Language -->

{% for member in total_member %}
	아이디 : {{ member.userid }}<br>
	이름 : {{ member.username }}<br>
{% endfor %} <!-- 반복문의 마지막 위치를 알려줌: 작성해주어야 for문 완성 --> 
```

<br>

## Update
변경할 것을 변경한 후에 `.save()`

```python
member = Member.objects.filter(userid="haha")

print(member.userid) # haha
print(member.username) # 홍길동

# id: 하하
# name: 홍길동
# username = "성춘향" 

member.save()
```

- PK (Primary Key, 주 키)
    - 하나의 레코드를 **다른 레코드와 식별하게 해주기 위한 "고유값"**  
    - **연속적인 숫자**로 하는 것이 일반적임 

    DB에서 CRUD를 처리할 때 내부적으로 pk를 이용해 많은 것을 함! 
    → pk를 이용해 조회, 삭제 등의 작업을 진행

    Django는 DB를 포함하기에 pk를 따로 지정할 필요가 없음! 

### **[filter() vs. get() 차이점](https://stackoverflow.com/questions/3221938/difference-between-djangos-filter-and-get-methods)**
- `filter()`
    - QuerySet을 반환
- `get()`
    - Object 반환
    - 가지고올 데이터가 하나만 있음을 보장할 때 사용
⇒ Basically use `get()` when you want to get **a single unique object**, and `filter()` when you want to get **all objects** that match your lookup parameters.

### 비밀번호 변경 예제
```python
(...)

# 로그인
def login(req):
    # get() 이용해 정보를 변경하고자 하는 "특정 user"를 가져옴 
    user = User.objects.get(userid="cherry1)
    # 비밀번호 변경
    user.password="2345 # password: User Model의 password 속성을 의미 
    user.save # DB에 저장
    
    return render(req, 'login.html')
```

`delete()`, `save()` 문은 한 객체에 대해서만 실행이 가능함 
  - ⭕️ user가 **object**이어야 정상적으로 실행
  - ❌ **queryset**이면 안됨     
⇒ **함수의 실행 대상이 하나만 존재**해야 한다는 의미      

🔗 [ref](https://stackoverflow.com/questions/9143262/delete-multiple-objects-in-django) (stackoverflow) - 복수 개의 데이터를 delete하는 방법! 
```python
Post.objects.all().delete()
Post.objects.filter(pup_date__gt=datetime.now()).delete()
```

<br>

## DELETE
`.delete()`

```python 
def signout(req):
    # get() 이용해 정보를 변경하고자 하는 "특정 user"를 가져옴 
    user = User.objects.get(userid="cherry1)

    # 회원 탈퇴
    user.delete()
    
    return render(req, 'signout.html)
```


<br>


# ORM, QuerySet

## QuerySet
전달받은 모델의 객체 목록 → DB로부터 데이터를 읽고, 필터를 걸거나 정렬 작업 가능			

Django Shell을 이용한 실습		
`python3 manage.py shell`: Django Shell 실행 명령어


(User 모델을 생성했다는 가정)
- `>>> from user.models import User`
    - User 객체를 사용하기 위해 모델을 불러와야 함 

- `>>> User.objects.all()`
    - 모든 객체 조회 

- `>>> User.objects.create(userid='4',username='테스트4',password='d',gender='M')`
    - 새로운 객체 생성 → 객체를 새로 만들 때, 모든 속성값들을 설정해야 함 

- `>>> User.objects.filter(gender='W')`
    - 필터링 (성별이 여자인 객체 필터링)

- `>>> User.objects.order_by('registered')` - 오름차순
- `>>> User.objects.order_by('-registered')` - 내림차순 (필드명 앞에 `-`)
    - 정렬 (등록 시간을 기준으로 정렬)

- `exit()`
    - 장고 shell 종료



