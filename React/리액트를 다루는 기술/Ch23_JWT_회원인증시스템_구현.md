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

### 로그인 구현하기 






<br> 

# 23.4 토큰 발급 및 검증하기





<br> 

# 23.5 posts API에 회원 인증 시스템 도입하기





<br> 

# 23.6 username/tags로 포스트 필터링하기 





<br> 

# 23.7 정리 




