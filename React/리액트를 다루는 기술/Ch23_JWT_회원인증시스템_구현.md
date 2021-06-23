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
**서버에서 토큰을 발급**해 클라이언트에서 사용자 로그인 정보를 가지고 있도록 함!!

`yarn add jsonwebtoken`: JWT 토큰을 만들기 위한 모듈

### 비밀키 설정하기

`openssl rand -hex 64`: 랜덤 문자열을 만들어줌
→ `.env` 파일에서 `JWT_SECRET`값으로 설정

비밀키는 JWT 토큰의 서명을 만드는 과정에서 사용되며, 외부 공개는 절대 안됨!

### 토큰 발급하기

### 1. 브라우저의 `localStorage` or `sessionStorage에` 담아서 사용

- 장점
    - 사용이 편리
    - 구현이 쉬움
- 단점
    - XXS(Cross Site Scripting) 공격에 취약 - 페이지에 악성 스크립트를 삽입해 토큰 탈취

### 2. 브라우저 `쿠키`에 담아서 사용 ✔

- 장점
    - `httpOnly` 속성 활성화시 XXS 공격으로부터 안전
- 단점
    - CSRF (Cross Site Request Forgery) 공격에 취약 - 토큰을 쿠키에 담으면 사용자가 서버로 요청할 때마다 무조컨 토큰이 함께 전달됨
    → 사용자가 모르게 원하지 않는 API 요청을 하게 만듦
    - CSRF 토큰 사용 및 Referer 검증 등의 방식으로 제대로 막을 수 있음

사용자 토큰을 쿠키에 담아서 사용!! - register와 login 함수 수정
src/api/auth/auth.ctrl.js

```javascript
// 회원가입
export const register = async (ctx) => {
  (...)

    // 응답할 데이터에서 hasedPassword 필드 제거
    ctx.body = user.serialize(); // src/models/user.js - serialize() 인스턴스 함수

    // 사용자 토큰을 쿠키에 담아서 사용
    const token = user.generateToken();
    ctx.cookies.set('access_token', token, {
      maxAge: 1000 * 60 * 60 * 24 * 7, // 7일
      httpOnly: true,
    });
  } catch (e) {
    ctx.throw(500, e);
  }
};

// 로그인
export const login = async (ctx) => {
  (..)
    ctx.body = user.serialize();

    // 사용자 토큰을 쿠키에 담아서 사용
    const token = user.generateToken();
    ctx.cookies.set('access_token', token, {
      maxAge: 1000 * 60 * 60 * 24 * 7, // 7일
      httpOnly: true,
    });
  } catch (e) {
    ctx.throw(500, e);
  }
};

```

Postman으로 로그인 요청 후, Headers의 Set-Cookit 헤더 확인
<img width="792" alt="스크린샷 2021-06-23 오전 1 57 29" src="[https://user-images.githubusercontent.com/55572222/122968318-5fd96480-d3c6-11eb-943d-8cd22893e249.png](https://user-images.githubusercontent.com/55572222/122968318-5fd96480-d3c6-11eb-943d-8cd22893e249.png)">

### 토큰 검증하기

사용자의 토큰 확인 후, 검증하는 작업 → `미들웨어`로 처리

src/lib/jwtMiddleware.js - lib 디렉터리에 **미들웨어 생성**
```javascript
import jwt, { decode } from 'jsonwebtoken';

const jwtMiddleware = (ctx, next) => {
  const token = ctx.cookies.get('access_token');
  if (!token) return next(); // 토큰 없음

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    console.log(decoded);
    return next();
  } catch (e) {
    // 토큰 검증 실패
    return next();
  }
};

export default jwtMiddleware;
```

src/main.js - main.js에서 **app에 미들웨어 적용**하기
```javascript
require('dotenv').config();
import Koa from 'koa';
import Router from 'koa-router';
import bodyParser from 'koa-bodyparser';
import mongoose from 'mongoose';

import api from './api';
import jwtMiddleware from './lib/jwtMiddleware';
import createFakeData from './createFakeData';

// 비구조화 할당을 통하여 process.env 내부 값에 대한 레퍼런스 만들기
const { PORT, MONGO_URI } = process.env;

mongoose
  .connect(MONGO_URI, {
    useNewUrlParser: true,
    useFindAndModify: false,
  })
  .then(() => {
    console.log('Connected to MongoDB');
    createFakeData();
  })
  .catch((e) => {
    console.error(e);
  });

const app = new Koa();
const router = new Router();

// 라우터 설정
router.use('/api', api.routes()); // api 라우트 적용

// 라우터 적용 전에 bodyParser 적용
app.use(bodyParser());
app.use(jwtMiddleware); // router 미들웨어보다 먼저 적용되어야 함

// app 인스턴스에 라우터 적용
app.use(router.routes()).use(router.allowedMethods());

// PORT 가 지정되어있지 않다면 4000 을 사용
const port = PORT || 4000;
app.listen(port, () => {
  console.log('Listening to port %d', port);
});
```
> app에 router 미들웨어 적용하기 전에 이루어져야함       


Postman에서 http://localhost:4000/api/auth/check 경로에 GET 요청시 터미널에 토큰 해석 결과 뜸      
![스크린샷 2021-06-23 오후 5 03 53](https://user-images.githubusercontent.com/55572222/123059700-ff3d3c80-d444-11eb-9c58-4d2efe6016cb.png)
        
ctx의 state에 넣어서 **토큰이 해석된 결과를 미들웨어에서 사용 가능하도록** 함!      
src/lib/jwtMiddleware.js    
```javascript
import jwt, { decode } from 'jsonwebtoken';

const jwtMiddleware = (ctx, next) => {
  const token = ctx.cookies.get('access_token');
  if (!token) return next(); // 토큰 없음

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    // ctx의 state에 토큰이 해석된 결과를 미들웨어 사용 가능하도록 함
    ctx.state.user = {
      _id: decode._id,
      username: decoded.username,
    };

    console.log(decoded);
    return next();
  } catch (e) {
    // 토큰 검증 실패
    return next();
  }
};

export default jwtMiddleware;
```

src/api/auth/auth.ctrl.js - check
```javascript
// GET /api/auth/check
// 로그인 상태 확인
export const check = async (ctx) => {
  const { user } = ctx.state;
  if (!user) {
    // 로그인 중 아님
    ctx.status = 401; // Unauthorized
    return;
  }
  ctx.body = user;
};
```

Postman으로 GET 요청 (http://localhost:4000/api/auth/check) - 서버 응답 확인          


### 토큰 재발급하기
jwtMiddleware로 토큰이 해석되고 터미널에 해석된 결과가 출력됨

![스크린샷 2021-06-23 오후 5 03 53](https://user-images.githubusercontent.com/55572222/123059700-ff3d3c80-d444-11eb-9c58-4d2efe6016cb.png)        
- `iat`: 토큰이 언제 만들어졌는지
- `exp`: 토큰이 언제 만료되는지 

#### `exp`로 표현된 날짜가 3.5일 미만인 경우, 토큰을 재발급하는 기능 구현하기

src/lib/jwtMiddleware.js
```javascript
import jwt, { decode } from 'jsonwebtoken';
import User from '../models/user';

const jwtMiddleware = async (ctx, next) => {
  const token = ctx.cookies.get('access_token');
  if (!token) return next(); // 토큰 없음

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    // ctx의 state에 토큰이 해석된 결과를 미들웨어 사용 가능하도록 함
    ctx.state.user = {
      _id: decode._id,
      username: decoded.username,
    };

    console.log(decoded);

    // 토큰의 남은 유효 기간이 3.5일 미만이면 재발급
    const now = Math.floor(Date.now() / 1000);
    if (decode.exp - now < 60 * 60 * 24 * 3.5) {
      const user = await User.findById(decode._id);
      const token = user.generateToken();
      ctx.cookies.set('access_token', token, {
        maxAge: 1000 * 60 * 60 * 24 * 7, //7dlf
        httpOnly: true,
      });
    }

    return next();
  } catch (e) {
    // 토큰 검증 실패
    return next();
  }
};

export default jwtMiddleware;
```
> 토큰 재발급 확인     
> user 모델 파일의 generateToken 함수에서 토큰 유효 기간을 3일로 설정       
> → login API 요청하고 check API 요청     
> → Headers에서 새 토큰이 Set-Cookie를 통해 설정됨      



### 로그아웃 기능 구현하기
쿠키를 지워줘야 함      

src/api/auth/auth.ctrl.js - logout
```javascript
// 로그아웃
// POST /api/auth/logout
export const logout = async (ctx) => {
  // 쿠키 지우기
  ctx.cookies.set('access_token');
  ctx.status = 204; // No Content
};
```

Postman에서 Post (http://localhost:4000/api/auth/logout) 호출       
<img width="1084" alt="스크린샷 2021-06-23 오후 6 00 49" src="https://user-images.githubusercontent.com/55572222/123068550-f3557880-d44c-11eb-825b-21475366cd3c.png">      
Cookie 헤더에서 `access_token`이 비워짐     



<br> 

# 23.5 posts API에 회원 인증 시스템 도입하기

기존 posts API에 회원 인증 시스템 추가하기        
- 새 포스트는 로그인한 유저만 작성 가능
- 포스트 삭제/수정은 작성자만 가능        

각 함수를 직접 수정해서 기능 구현도 가능하지만, 해당 프로젝트에서는 `미들웨어`를 만들어 관리!      

### 스키마 수정하기

**각 포스트를 어떤 사용자가 작성했는지** 알아야 하기 때문에 기존의 Post 스키마를 수정함 → 스키마에 **`사용자 정보`**를 넣어줌!!       

- 관계형 데이터베이스: 데이터의 id만 관계 있는 데이터에 넣어줌 
- MongoDB: 필요한 데이터를 통째로 집어넣음        

#### Post 스키마 안에 사용자의 id와 username을 넣어야 함

src/models/posts.js - user 정보 추가
```javascript
import mongoose from 'mongoose';

const { Schema } = mongoose;

const PostSchema = new Schema({
  title: String,
  body: String,
  tags: [String], // 문자열로 이루어진 배열
  publishedDate: {
    type: Date,
    default: Date.now, // 현재 날짜를 기본값
  },
  // 사용자 정보 추가
  user: {
    _id: mongoose.Types.ObjectId,
    usernmae: String,
  }
});

const Post = mongoose.model('Post', PostSchema); // 스키마 이름, 스키마 객체
export default Post;
```

### posts 컬렉션 비우기
post 데이터에는 사용자 정보가 필요하므로 이전에 생성한 데이터들이 유효하지 않으므로 삭제! → MongoDB Compass의 오른쪽 휴지통 아이콘 




### 로그인했을 때만 API를 사용할 수 있게 하기

**`checkLoggedIn 미들웨어`**        
로그인해야만 글쓰기/수정/삭제 가능하도록 수정           

src/lib/checkLoggedIn.js
```javascript
const checkedLoggedIn = (ctx, next) => {
  if (!ctx.state.user) {
    ctx.status = 401; // Unauthorized - 로그인 상태가 아닌경우
    return;
  }
  return next(); // 로그인 상태라면 그 다음 미들웨어 실행
};

export default checkedLoggedIn;
```
> **`로그인 상태를 확인하는 작업`** 은 다른 라우트에서도 사용될 가능성이 많기 때문에 재사용을 위해 lib 디렉터리에 저장!

이 미들웨어를 posts 라우터에서 사용
```javascript
import Router from 'koa-router';
import * as postsCtrl from './posts.ctrl';
import checkedLoggedIn from '../../lib/ckeckLoggedIn';

const posts = new Router();

// 여러 종류의 라우트 설정, printInfo 함수 호출
posts.get('/', postsCtrl.list);
posts.post('/', checkedLoggedIn, postsCtrl.write); // 포스트 작성 - 로그인 상태 확인 미들웨어
posts.post('/', postsCtrl.write);
posts.get('/:id', postsCtrl.checkObjectId, postsCtrl.read);
posts.delete('/:id', checkedLoggedIn, postsCtrl.remove); // 포스트 삭제 - 로그인 상태 확인 미들웨어
// posts.put('/:id', postsCtrl.replace);
posts.patch('/:id', checkedLoggedIn, postsCtrl.update); // 포스트 업데이트 - 로그인 상태 확인 미들웨어

// 리팩토링
// const post = new Router(); // /api/posts/:id 경로를 위한 라우터
// post.get('/', postsCtrl.read);
// post.delete('/', postsCtrl.remove);
// // posts.put('/:id', postsCtrl.replace);
// post.patch('/', postsCtrl.update);

// posts.use('/:id', postsCtrl.checkObjectId, post.routes());

export default posts;
```


### 포스트 작성 시 사용자 정보 넣기







### 포스트 수정 및 삭제 시 권한 확인하기






<br> 

# 23.6 username/tags로 포스트 필터링하기 





<br> 

# 23.7 정리 




