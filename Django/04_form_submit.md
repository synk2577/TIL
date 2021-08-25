## ORM (Object Relational Mapping)

**객체**와 **관계형 데이터베이스의 데이터**를 매핑   
```
데이터베이스 종류: mysql, mssql, oracle ...       
CRUD 

sql문 == query문 = 질의문: 데이터베이스에 CRUD 같은 작업들을 명령하기 위한 명령어 
```

Django는 DB가 builtin으로 들어가있고, DB를 관리하기위한 **Django는 ORM기능을 포함** ⇒ SQL문을 작성할 필요가 없음!!          

(SQL 쿼리문을 사용) 데이터 생성/삽입/삭제 등의 작업을 코드로 모두 작성		   
(ORM을 사용할 경우) 쿼리문 없이 간단한 코드 작성 가능

settings.py 파일에서 `DATABASES = {}` 구문에서 DB 설정이 가능함     		
DB 종류를 설정하면 ORM이 번역을 통해 적절한 쿼리문으로 변경해 줌!! → DB 종류가 변경되더라도 코드를 변경할 필요가 없어지는 장점! 


<br> 

## Form Tag
`<form>`는 **client가 server로 정보를 전송**하기 위한 방법 중 하나.

### form 요소의 종류

```html
<form action method>
	인풋 타입 히든 : <input type="hidden">
	인풋 타입 텍스트 : <input type="text">
	인풋 타입 패스워드 : <input type="password">
	인풋 타입 라디오 : <input type="radio"> 
	인풋 타입 체크박스 : <input type="checkbox">

	<br>
	셀렉트 :
	<select name="gender">
		<option value="m">남성</option>
		<option value="w">여성</option>
	</select>

	<textarea name="letter" rows="10" style="width: 100%;"></textarea>

	<button type="submit">타입 서브밋 버튼</button>
	<button type="button">타입 버튼 버튼</button>
</form>
```

`<input>`

- attribute: `type`   
    text, password, radio, checkbox ...
- **닫지 않아도 되는 태그**

`<input type="radio">`
- 복수 선택 불가능
    - `name` attribute로 **동일한 이름을 설정**해야 단일 선택이 가능해짐
- 선택 취소 불가능   
    - 없음 요소를 추가해 선택 실수를 방지

`<input type="checkbox">`
- 복수 선택 가능
- 선택 취소 가능

`<input type="hidden" name="sec" value="1">`
- hidden type의 역할   
    약속되어 있는 정보가 넘어오지 않으면 비정상적인 루트로 인식해 보안상 차단하는 역할

`<select>`
- radio 태그와 유사
- 클릭해야 요소를 볼 수 있음
- `select` 태그내 `option`태그로 선택사항을 작성
    - option태그에 value를 작성하지 않으면 txt가 value로 넘어감

`<button type="submit">`
- 버튼이 속해있는 **Form이 전송**됨!! (후손)
- default: submit

`<form action="" method="">`
- `action`
    - 정보를 보낼 곳의 주소
    - action 속성을 부여하지 않으면 자기 주소를 가르킴 (자기자신에게 보냄)
- `method`
    - 정보를 보낼 방법
    - `get` or `post`
        - default: get
    - **get 방식** : 정보를 url에 붙혀서 전송하는 방식!      
        submit btn 클릭 후 url: `정보보낼곳의주소?폼요소1name=폼요소1vlaue&폼요소2name=폼요소2vlaue`
    - **post 방식** : 정보를 노출되지 않도록 몰래 전송하는 방식! ⇒ 보안상 뛰어남      
        submit btn 클릭 후 url: `정보보낼곳의주소`

```
form 태그의 모든 요소는         
반드시 `name` attribute를 가져야 한다!! (nor, server로 전송이 되지 않음)       
대부분 `value` attribute를 가져야 한다!! (value를 안쓰면 공란으로 server에 넘어감)       
⇒ server에서 어떤 name으로 value를 갖는지 확인하기 위함
```

<br>

## 회원가입 폼 작성 후 정보 전송 (member app)

### 현재 프로젝트 구조
```python
/ssac-django
	/first_homepage [프로젝트 폴더] 
		manage.py
		/first_homepage [앱 폴더]
			settings.py 
			urls.py (최상단 url관리 파일)
		/member [앱 폴더]
			/templates
				signup.html
				signup_success.html		
			urls.py
			views.py 
```

#### 1) 2개의 html 파일에 각각 회원가입 폼 & 데이터 전달받을 페이지 생성 
- member/templates/**signup.html**		
    form 태그의 `action` attribute: 받을 곳의 주소		
    form 태그의 `method` attribute: 보내는 방식 (회원가입이므로 post 방식) 

    ```html
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <title>Sign Up</title>
      </head>
      <body>
        <h1>Sign Up</h1>
        <form
        action="http://49.50.173.206:8000/dontgiveup/recieve"
        method="post"
        >
            {% csrf_token %}
            <div>
                ID <input type="text" name="id">
            </div>

            <div>
                PW <input type="password" name="password">
            </div>
            
            <div>
                성별
                <select name="gender">
                    <option value="M" >남성</option>
                    <option value="W" >여성</option>
                </select>
            </div>
            <div>
                학력
                <label>
                    <input type="radio" name="education[]" value="nope">선택 안함
                </label>
                <label>
                    <input type="radio" name="education[]" value="elementry school">초졸
                </label>
                <label>
                    <input type="radio" name="education[]" value="middle school">중졸
                </label>
                <label>
                    <input type="radio" name="education[]" value="high school">고졸
                </label>
                <label>
                    <input type="radio" name="education[]" value="university">대졸
                </label>
            </div>
            <div>
                생년월일(8자리)
                <input type="number" name="birth">
            </div>
            <button type="submit">회원가입</button>
        </form>
    </html>
    ```

- member/templates/**signup_success.html**
    `{{ param.. }}` : view에서 template으로 전달한 변수값 (Django Template 언어)

    ```html
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <title>SignUp Success!</title>
      </head>
      <body>
        <h3>{{param1}}님, 회원가입을 축하드립니다!</h3>

        <div>
          <h5>가입 정보를 확인해주세요!</h5>
          <table>
            <tr>
              <td>아이디</td>
              <td>{{param1}}</td>
            </tr>
            <tr>
              <td>패스워드</td>
              <td>{{param2}}</td>
            </tr>
            <tr>
              <td>성별</td>
              <td>{{param3}}</td>
            </tr>
            <tr>
              <td>생년월일</td>
              <td>{{param4}}</td>
            </tr>
            <tr>
              <td>학력</td>
              <td>{{param5}}</td>
            </tr>
          </table>
        </div>
      </body>
    </html>
    ```

#### 2) 라우팅 설정 

2-1) member앱의 urls.py파일에 url 추가		
```python
from django.urls import path
from . import views # .(현재위치- member 앱폴더)에서 views.py 열기 

urlpatterns = [
    path('ok', views.hello),
    path('recieve', views.rec), # dontgiveup/recieve
    path('send', views.send), # dontgiveup/semd 
]
```

2-2) member앱의 views.py파일에 함수 작성		
**파라미터 동적 호출**: render의 세번째 인자로 파라미터를 동적 호출할 수 있음					
`{'[파라미터명]': [함수의 인자].[form 태그의 method속성값(GET|POST)].get('[전달하려는 태그의 name속성값]')}`

```python
from django.shortcuts import render
import random 

def hello(req): # req: 요청한 사람의 웹브라우저 
    c=random.randint(1, 20)
    
    return render(req, 'aa.html', {'parameter1':c, 'parameter2':'seoyeon'} )

def send(req):
    return render(req, 'signup.html') # 회원가입 페이지 

def rec(req):
    return render(req, 'signup_success.html', {'param1': req.POST.get('id'), 'param2': req.POST.get('password'), 'param3': req.POST.get('gender'), 'param4': req.POST.get('birth'), 'param5': req.POST.get('education[]') }) # 회원가입 성공 페이지
```

<br>


### **403 Error** - 보안관련 에러
로컬에서 폼 작성하고 submit버튼 클릭 후 → 서버로 전송하는 과정; 페이지를 열람할 권한이 없어서 뜨는 에러		
Django는 기본적으로 csrf token으로 CSRF 공격을 방어한다


### CSRF (Cross Site Request Forgery) & CSRF Token
CSRF는 웹사이트 취약점 공격 중 하나로 사용자의 의지와 무관하게 **공격자가 의도한 행위를 특정 웹사이트에 요청**하는 공격      	
→ CSRF 공격에 대한 방어는 `POST`, `PATCH`, `DELETE` Method에 적용함  (GET Method는 방어하진 않음! → 조회성)     

사용자의 세션에 임의의 난수값을 생성하고 사용자의 요청마다 난수값을 포함시켜 전송함     		   
→ backend에서 요청을 받을때 마다 세션에 저장된 토큰 값과 요청 파라미터에 전달되는 토큰값이 일치하는지 검증!      

Django에서는 template의 form 태그 내부에 **`{% csrf_token %}` 을 작성**해 CSRF 공격을 방어할 수 있음     			
⇒ 적합하지 않은 경로에서 http 요청을 보내는 경우, 올바르지 않은 요청이라 판단해 403에러로 요청 거부할 수 있음    

![image](https://user-images.githubusercontent.com/55572222/130762023-67e1147e-163d-4a0b-bf00-69dae370c067.png)

=> `input` 태그의 value속성 값이 **임의의 난수값**임!  


### **Django Template 언어**
[🔗 공식문서(한글)](https://django-doc-test-kor.readthedocs.io/en/old_master/topics/templates.html#template-inheritance)

- 템플릿 변수 `{{ 변수 }}`		
    : view에서 template으로 객체 전달 가능 
- 템플릿 태그 `{% tag %}`		
    : 텍스트를 작성하거나 루프/로직을 수행



