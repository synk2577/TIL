# HTML Validation Check
유효성 검사  🔗 [html-form-validation](https://jeonghwan-kim.github.io/dev/2020/06/08/html5-form-validation.html)

```html
<form>
  ...
  <div>
    ID <input type="text" name="id" required />
  </div>
  <div>
    PW <input type="password" name="password" required />
  </div>
  <div>
    Email
    <input
      type="email"
      name="password"
      pattern="[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*.[a-zA-Z]$"
    />
  </div>
</form>
```

- `required` : 필수속성
- `button type=submit`이 클릭될때 button이 속한 form 안에 form 요소들이 미리 정의한 required 속성의 규칙을 지켰는지 검사하여 submit을 이어감. 만약 규칙이 지켜지지 않았다면, 넘어가지 않고 경고 메세지를 띄움
- `min`, `max`: 최소/최대 정수 지정
- `minLength`, `maxLength`: 최소/최대 글자수 지정
- `type`: 타입 (이메일 등)
- `pattern` : 정규표현식 사용 가능

ex) e-mail 유효성 검사     
`알파벳, 숫자, 제한된 특수문자 1개이상@알파벳, 숫자, 제한된 특수문자 1개이상.알파벳, 숫자, 제한된 특수문자 1개이상`      
→ 코드로 어떻게 표현하지? **정규표현식**!!

![image](https://user-images.githubusercontent.com/55572222/130968999-dcedb151-0a05-48cb-861f-5e9bd4078489.png)


### 정규표현식 - regex (regular expression)
🔗 [Javascript Regex](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_Expressions)

- `/`: 정규식을 사용한다는 의미 (정규표현식을 `//`으로 감싼다)
- `^`: 시작
- `$`: 끝
- `[]`: 범위
    - ex) `[a-z]`: a부터 z까지 중 문자 하나
    - ex) `[가-힣]`: 한글의 모든 음절 범위를 표현
- `{}`: 개수
    - ex) `{2, 4}`: 2개 부터 4개까지
- `()`: 그룹 검색 및 분류
- `.`: 임의의 글자 하나
- `+`: 1개 이상의 글자
- `*`: 0개 이상의 모든 문자
- `?`: 0~1번 반복되는 문자열

<br>

### 이스케이프 시퀀스

🔗 [escape sequence](https://atomic0x90.github.io/c-language/2019/05/28/C-Language-escape-sequence.html)      
프로그래밍 언어 특성상 표현할 수 없는 기능, 문자를 표현해준다. 컴퓨터를 제어하는 목적으로 사용되는 특수한 문자이다.

**Email Validation Regex**    
`/[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*.[a-zA-Z]$/`     
⇒ [https://regex101.com/](https://regex101.com/)에서 정규표현식 test 가능 

Q. 저 질문있어요 언어마다 정규식이 조금씩 다르다고 하셨는데 저희는 장고프레임워크 하고있구 이거는 파이썬 기반인데 정규식은 자바스크립트로 연습하라고 하신이유가있나요??        
유효성 검사를 frontend, backend에서 한번씩 총 2번 해주어야 함!!
