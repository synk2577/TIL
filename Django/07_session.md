# 세션

서버가 가지고 있는 인증 정보    
ex) 네이버에 한 번 로그인하면 어떤 작업을 해도 로그인 정보가 유지되어 다시 로그인하지 않아도 정상적으로 사이트 이용 가능 

→ 최초 로그인 시점에 **서버가 세션을 생성**하기 때문!! 
세션을 생성하지 않는다면?    
메일 읽기/쓰기 등의 작업에서 매번 사용자의 권한을 확인해야 함 → 비효율적인 코드로 실용성이 없음         

로그인 했을 때 그 정보가 유지되도록 해보자!     
※ **세션은 request에 담겨 있어** 따로 불러오지 않아도 사용 가능! 

<br>


⇒ 이전에 사용한 login 함수 (views.py)

    ```python
    def login(request):
    	if request.method == "POST" :
    		userid = request.POST.get('userid')
    		password = request.POST.get('password')
    		data = {}
    		if not ( userid and password ):
    			data['error'] = '모든 값을 입력해주세요.'
    			return render(request, 'user/login.html', data)
    		else:
    			user = User.objects.get(userid=userid)
    			if user.password == password:
    				return redirect('../main')
    			else:
    				data['error'] = '아이디 또는 비밀번호가 틀립니다.'
    				return render(request, 'user/login.html', data)
    	else:
    		return render(request, 'user/login.html')
    ```

⇒ 로그인 성공시 user라는 key로 세션 등록! 

```python
def login(request):
 if request.method == "POST" :
  userid = request.POST.get('userid')
  password = request.POST.get('password')
  data = {}
  if not ( userid and password ):
   data['error'] = '모든 값을 입력해주세요.'
   return render(request, 'user/login.html', data)
  else:
   user = User.objects.get(userid=userid)
   if user.password == password:
    request.session['user'] = userid
    return redirect('../main')
   else:
    data['error'] = '아이디 또는 비밀번호가 틀립니다.'
    return render(request, 'user/login.html', data)
 else:
  return render(request, 'user/login.html')
```

<br>


- `request.session.get('키')` : **세션 검사**(존재여부) - 세션을 가져오고 싶은 경우
    세션이 존재하면 true, 존재하지 않는다면 false 반환 
- `request.session.pop('키')` : **세션 삭제**
    ```python
    user = request.session.get('키') # user 세션 생성 
    request.session.pop('user') # 세션 삭제
    ```
- `req.session.get('키') = 값` : **세션 로드**
- `req.session['키'] = 값` : **세션 저장**
    - `req.session['키']` 가 일종의 `변수처럼` 사용됨!! 해당 변수에 저장된 세션을 사용하는 것이지!!


Q. 세션을 삭제하는 이유?   
A. 로그아웃 시, 세션이 유지되면 안되기 때문!
