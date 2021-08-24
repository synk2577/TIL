### ORM (Object Relational Mapping)

**객체**와 **관계형 데이터베이스의 데이터**를 매핑   
code와 실DB를 매핑

  데이터베이스 종류: mysql, mssql, oracle ...       
  CRUD 

  **sql문 == query문 = 질의문** : 데이터베이스에 CRUD 같은 작업들을 명령하기 위한 명령어 
  
Django는 DB가 builtin으로 들어가있고, DB를 관리하기위한       
**Django는 ORM기능을 포함** ⇒ SQL문을 작성할 필요가 없음!!          
SQL 쿼리문을 사용할 경우, 데이터 생성/삽입/삭제 등의 작업을 코드로 모두 작성해주어야 하지만,         
ORM을 사용할 경우, 쿼리문 없이 간단히 작성이 가능

settings.py 파일에서 `DATABASES = {}` 구문에서 DB 설정이 가능함     
  ex) DB 종류를 설정하면 ORM이 번역을 통해 적절한 쿼리문으로 변경해 줌!! → DB 종류가 변경되더라도 코드를 변경할 필요가 없어지는 장점! 

--- 
### Form Tag

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

form 태그의 모든 요소는         
**반드시 `name` attribute**를 가져야 한다!! (nor, server로 전송이 되지 않음)       
**대부분 `value` attribute**를 가져야 한다!! (value를 안쓰면 공란으로 server에 넘어감)       
⇒ server에서 **어떤 name으로 value를 갖는지 확인**하기 위함

### token
일종의 통행증!

---

**form data 받기** 

```html
<form action="[http://118.67.133.61:8000/dontgiveup/receive](http://118.67.133.61:8000/dontgiveup/receive)" method="post">
	<input type="text" name="my_info">
	<button>전송</button>
</form>
```

**403 Error** - 보안관련 에러
로컬에서 폼 작성하고 submit버튼 클릭 후 → 서버로 전송하는 과정; 페이지를 열람할 권한이 없어서 뜨는 에러   
`{% csrf_token %}` : 사이트에 접속할 수 있도록 함     
form 태그 안에 반드시 작성해주어야 함     

**Django Template 언어**    
- `{{ }}` : 변수
- `{% %}` : 코딩
