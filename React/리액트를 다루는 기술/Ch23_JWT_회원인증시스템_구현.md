# 23.1 JWT의 이해

JWT 기술을 이용해 서버에 회원 인증 시스템 구현해보자!    
- **`JSON Web Token`** 의 약자로 데이터가 JSON으로 이루어져 있는 토큰을 의미     
- 두 개체가 서로 안전하게 정보를 주고받을 수 있ㄷ록 웹표준으로 정의된 기술

### 세션기반 vs. 토큰기반 인증
사용자의 로그인 상태를 처리하는 두가지 인증 방식
#### 1. 세션 기반 인증 시스템
서버가 사용자가 로그인 중임을 기억하고 있음    

#### 단점
- 서버 확장이 번거로울 수 있음    
- 서버 인스턴스가 여러 개인 경우, 모든 서버가 같은 세션을 공유해야 하므로 전용 DB가 필요함

#### 2. 토큰 기반 인증 시스템 
##### **`토큰`**
- 로그인 이후 서버가 만들어주는 문자열 
- 문자열에는 `사용자의 로그인 정보`와 해당 정보가 서버에서 발급되었음을 `증명하는 서명`이 들어있음                           
    - 서명 데이터는 해싱 알고리즘을 통해 만들어짐 (`HMAC SHA256` or `RSA SHA256`) 
- 서명의 존재로 `무결성`이 보장됨


#### 장점
- 서버에서 사용자 로그인 정보를 기억할 필요가 없으므로 사용하는 리소스가 적음 → 서버의 확장성이 매우 높음     
- 서버의 인스턴스가 여러 개라도 서버끼리 사용자 로그인 상태를 공유할 필요 없음   
- 인증 시스템 구현이 간편
- 사용자 인증 상태 관리가 쉬움 
   


<br> 

# 23.2 User 스키마/모델 만들기

src/models/user.js
```javascript
import mongoose, { Schema } from 'mongoose';

const UserSchema = new Schema({
  username: String,
  hashedPassword: String, // 단방향 해시 함수로 암호화
});

const User = mongoose.model('User', UserSchema); // 스키마 이름, 스키마 객체
export default User;
```

### 모델 메서드 만들기
모델에서 사용할 수 있는 함수
- 인스턴스 메서드
  - 모델을 통해 만든 문서 인스턴스에서 사용 가능한 함수
    ```javascript
    const user = new Schema({ username: 'velopert' });
    user.setPassword('mypass123');
    ```
- 스태틱 메서드
  - 모델에서 바로 사용 가능한 함수 
    ```javascript
    const user = User.findByUsername('verlopert');
    ```
### 인스턴스 메서드 만들기
`function` 키워드로 구현 (화살표 함수 ❌)     
→ 함수 내부에서 `this`로 접근이 필요함!! `this`는 문서 인스턴스를 가르킴      
```javascript
import mongoose, { Schema } from 'mongoose';
import bcrypt from 'bcrypt';

const UserSchema = new Schema({
  username: String,
  hashedPassword: String, // 단방향 해시 함수로 암호화
});

// Instance Method
// setPassword: 비밀번호를 파라미터로 받아서 hashedPassword 값 설정
UserSchema.methods.setPassword = async function (password) {
  const hash = await bcrypt.hash(password, 10);
  this.hashedPassword = hash;
};

// checkPassword: 파라미터로 받은 비밀번호가 해당 계정의 비밀번호와 일치하는지 확인
UserSchema.methods.checkedPassword = async function (password) {
  const result = await bcrypt.compare(password, this.hashedPassword);
  return result; // true or false
};

const User = mongoose.model('User', UserSchema); // 스키마 이름, 스키마 객체
export default User;
```


### 스태틱 메서드 만들기
`this`는 모델(`User`)을 가르킴   
```javascript
import mongoose, { Schema } from 'mongoose';
import bcrypt from 'bcrypt';

const UserSchema = new Schema({
  username: String,
  hashedPassword: String, // 단방향 해시 함수로 암호화
});

// Instance Method
// setPassword: 비밀번호를 파라미터로 받아서 hashedPassword 값 설정
UserSchema.methods.setPassword = async function (password) {
  const hash = await bcrypt.hash(password, 10);
  this.hashedPassword = hash;
};

// checkPassword: 파라미터로 받은 비밀번호가 해당 계정의 비밀번호와 일치하는지 확인
UserSchema.methods.checkedPassword = async function (password) {
  const result = await bcrypt.compare(password, this.hashedPassword);
  return result; // true or false
};

// Static Method
// findByUsername: username으로 데이터를 찾을 수 있게 함 
UserSchema.statics.findByUsername = function (username) {
  return this.findOne({ username});
};

const User = mongoose.model('User', UserSchema); // 스키마 이름, 스키마 객체
export default User;
```


<br> 

# 23.3 회원 인증 API 만들기
새로운 라우트를 정의   

함수의 틀 잡기    

src/api/auth/auth.ctrl.js
```javascript
export const register = async ctx => {
    // 회원가입
};
export const login = async ctx => {
    // 로그인
};
export const check = async ctx => {
    // 로그인 상태 확인
};
export const logout = async ctx => {
    // 로그아웃
};
```      

4개의 API를 만듦       

src/api/auth/index.js
```javascript
import Router from 'koa-router';
import * as authCtrl from './auth.ctrl';

const auth = new Router();

auth.post('/register', authCtrl.register);
auth.post('/login', authCtrl.login);
auth.get('/check', authCtrl.check);
auth.post('/logout', authCtrl.logout);

export default auth;
```

auth 라우터를 api 라우터에 적용하기

src/api/index.js
```javascript
import Router from 'koa-router';
import posts from './posts';
import auth from './auth';

const api = new Router();

api.use('/posts', posts.routes());
api.use('/auth', auth.routes());

// 라우터 내보내기
export default api;
```

### 회원가입 구현하기 

src/api/auth/auth.ctrl.js 
```javascript
import Joi from '@hapi/joi';
import User from '../../models/user';

// POST /api/auth/register
// {
//     username: 'velopert',
//     password: 'mypass123'
// }
export const register = async (ctx) => {
  // Request body 검증하기
  const schema = Joi.object().keys({
    username: Joi.string().alphanum().min(3).max(20).required(),
    password: Joi.string().required(),
  });
  const result = schema.validate(ctx.request.body);
  if (result.error) {
    ctx.status = 400;
    ctx.body = result.error;
    return;
  }

  const { username, password } = ctx.request.body;
  try {
    // username이 이미 존재하는지 확인 - 중복 계정 확인
    const exists = await User.findByUsername(username);
    if (exists) {
      ctx.status = 409; // Conflict
      return;
    }

    const user = new User({
      username,
    });

    await user.setPassword(password); // 비밀번호 설정
    await user.save(); // 데이터베이스에 저장

    // 응답할 데이터에서 hasedPassword 필드 제거
    ctx.body = user.serialize(); // src/models/user.js - serialize() 인스턴스 함수
  } catch (e) {
    ctx.throw(500, e);
  }
};

export const login = async (ctx) => {
  // 로그인
};

export const check = async (ctx) => {
  // 로그인 상태 확인
};

export const logout = async (ctx) => {
  // 로그아웃
};
```
> `findByUsername()` 스태틱 메서드: 중복 계정이 생성되지 않도록 기존에 해당 username 존재 여부 확인      
> `setPassword()` 인스턴스 함수: 비밀번호 설정      
> `serialize()` 인스턴스 함수: user.js 모델 파일에 작성된 함수로 hasedPassword 필드가 응답되지 않도록 JSON으로 변환한 후 delete를 통해 해당필드를 지우는 과정이 자주 필요하여 작성됨      

POST http://localhost:4000/api/auth/register 요청시                
```json
{
    "username": "welopert",
    "password": "mypass123"
}
```

`_id`, `username`, `__v` 응답이 나타남     

중복된 username으로 요청 보낼시 Conflict 에러 발생 확인!


### 로그인 구현하기 

src/api/auth/auth.ctrl.js - login
```javascript
// 로그인
// POST /api/auth/login
// {
//   usernmae: 'verlopert',
//   password: 'mypass123'
// }
export const login = async (ctx) => {
  const { username, password } = ctx.request.body;

  // username, password가 없으면 에러 처리
  if (!username || !password) {
    ctx.status = 401; // Unauthorized
    return;
  }

  try {
    const user = await User.findByUsername(username); // 사용자 데이터를 찾음
    // 계정이 존재하지 않으면 에러 처리
    if (!user) {
      ctx.status = 401;
      return;
    }

    const valid = await user.checkPassword(password); // 비밀번호 검사 
    // 잘못된 비밀번호
    if (!valid) {
      ctx.status = 401;
      return;
    }
    ctx.body = user.serialize(); // 성공시 계정 정보를 응답 
  } catch (e) {
    ctx.throw(500, e);
  }
};
```
> `username`, `password` 값이 전달되지 않으면, 에러 처리     
> `findByUsername()`: 사용자 데이터를 찾고, 사용자 데이터가 없다면 에러 처리
> `checkPassword()`: 계정이 유효한 경우 비밀번호 검사하고 성공시 계정 정보를 응답

회원가입에서 생성했던 계정 정보로 로그인 API 요청
<img width="875" alt="스크린샷 2021-06-22 오후 5 56 47" src="https://user-images.githubusercontent.com/55572222/122895803-698da880-d383-11eb-9828-0aa93a08db92.png">

틀린 비밀번호로 로그인 API 요청 → `401 Unauthorized` 에러 발생!
<img width="883" alt="스크린샷 2021-06-22 오후 5 57 12" src="https://user-images.githubusercontent.com/55572222/122895729-5a0e5f80-d383-11eb-97da-c70163d38339.png">



<br> 

# 23.4 토큰 발급 및 검증하기





<br> 

# 23.5 posts API에 회원 인증 시스템 도입하기





<br> 

# 23.6 username/tags로 포스트 필터링하기 





<br> 

# 23.7 정리 




