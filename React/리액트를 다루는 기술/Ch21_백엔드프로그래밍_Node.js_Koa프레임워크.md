# 21.1 소개하기

### 백엔드 
서버는 데이터를 여러 사람과 공유하는 곳    

#### 백엔드 프로그래밍 (back-end)
- 데이터 등록, 조회 등 로직을 만드는 것    
- 언어에 구애받지 않고 여러 환경으로 진행 가능함 (PHP, Python, Golang, Java, JavaScript ...)


### `Node.js`
Google 크롬 웹 브라우저의 **V8 자바스크립트 엔진**을 기반으로 웹 브라우저 뿐만 아니라 **서버에서도 자바스크립트를 사용**할 수 있는 **런타임 개발**


### `Koa`
Node.js 환경에서 웹 서버 구축시 **Express, Hapi, Koa 등의 웹 프레임워크** 사용함     
기존의 Express 개발팀이 고칠 점을 개선해 개발한 아예 새로운 프레임워크       

Express | Koa
--------|-------
미들웨어, 라우팅, 템플릿, 파일 호스팅 기능 자체 내장 | **미들웨어 기능만** 내장 <br> 다른 기능은 라이브러리를 적용 → Express보다 가벼움 <br> **`async/await` 문법 지원으로 비동기 작업** 관리



<br>

# 21.2 작업환경 준비

노드 설치 확인    
`node --version`    

프로젝트 생성   
`mkdir blog`    
`cd blog`   
`mkdir blog-backend`    
`cd blog-backend`   
`yarn init -y // 패키지 정보 생성`   

package.json 파일 확인    
`cat package.json`    

Koa 웹 프레임워크 설치    
`yarn add koa`    

ESLint와 Prettier 설정   



<br>

# 21.3 Koa 기본 사용법

### 서버 띄우기

index.js
```javascript
const Koa = require('koa');

const app = new Koa();

app.use((ctx) => {
  ctx.body = 'hello world';
});

app.listen(4000, () => {
  console.log('Listening to port 4000');
});
```
> 서버 포트를 4000번으로 열고, 서버에 접속하면 'hello world' 텍스트 반환      
> 터미널에서 `node src` 실행후 http://localhost:4000/ 접속시 화면에 hello world 출력 확인 가능    

※ index.js 파일은 해당 디렉터리를 대표하는 파일이기에 예외적으로 디렉터리(src)까지만 입력해도 실행 가능함   


### 미들웨어 
Koa 애플리케이션은 미들웨어 배열로 구성됨    

**`app.use`함수: 미들웨어 함수를 애플리케이션에 등록하며, 등록되는 순서대로 처리됨**

① 미들웨어 함수 구조
```javascrip t
(ctx, next) => {
  // ctx: 웹 요청과 응답에 관한 정보
  // next: 현재 처리중인 미들웨어의 다음 미들웨어를 호출하는 함수 
}
```

② next 함수를 사용하지 않는 경우;    
미들웨어 등록후 next 함수 미호출시 그 다음 미들웨어를 처리하지 않음 
```javascript
ctx => {}
// ex) 라우터 미들웨어: 주로 다음 미들웨어를 처리할 필요가 없기에 next를 생략한 구조로 미들웨어를 작성함
```

index.js
```javascript
const Koa = require('koa');

const app = new Koa();

app.use((ctx, next) => {
  console.log(ctx.url);
  console.log(1);
  next(); // 현재 처리중인 미들웨어의 다음 미들웨어를 호출하는 함수
});
app.use((ctx, next) => {
  console.log(2);
  next();
});

app.use((ctx) => {
  ctx.body = 'hello world';
});

app.listen(4000, () => {
  console.log('Listening to port 4000');
});
```
> Listening to port 4000    
> /   
> 1   
> 2   



### 조건부 미들웨어 처리

```javascript
const Koa = require('koa');

const app = new Koa();

app.use((ctx, next) => {
  console.log(ctx.url);
  console.log(1);

  // 요청 경로에 authoried=1 쿼리 포함시 미들웨어 처리
  if (ctx.query.authorized !== '1') {
    ctx.status = 401; // Unahuthorized
    return;
  }

  next();
});

app.use((ctx, next) => {
  console.log(2);
  next();
});

app.use((ctx) => {
  ctx.body = 'hello world';
});

app.listen(4000, () => {
  console.log('Listening to port 4000');
});
```
> http://localhost:4000/?authorized=1 쿼리 포함시, 브라우저에 **hello world 출력**      
> http://localhost:4000/ 경로는 브라우저에 **Unauthorized 출력**


### next 함수의 `Promise` 반환
**next 함수 호출시 `Promise`를 반환**하는데, 이는 Express와의 차이점!!     

Promise   
다음에 처리해야할 미들웨어가 끝나야 완료됨! 
```javascript
const Koa = require('koa');

const app = new Koa();

app.use((ctx, next) => {
  console.log(ctx.url);
  console.log(1);

  // 요청 경로에 authoried=1 쿼리 포함시 미들웨어 처리
  if (ctx.query.authorized !== '1') {
    ctx.status = 401; // Unahuthorized
    return;
  }

  // Promise 가 끝난 다음 콘솔에 END 기록 
  next().then(() => {
    console.log('END');
  });
});

app.use((ctx, next) => {
  console.log(2);
  next();
});

app.use((ctx) => {
  ctx.body = 'hello world';
});

app.listen(4000, () => {
  console.log('Listening to port 4000');
});
```
> `.then`을 사용해 `Promise`가 끝난 다음에 콘솔에 END를 기록함



### `async/await` 사용
Koa는 `async/await`를 정식 지원함

```javascript
const Koa = require('koa');

const app = new Koa();

app.use(async (ctx, next) => {
  console.log(ctx.url);
  console.log(1);

  // 요청 경로에 authoried=1 쿼리 포함시 미들웨어 처리
  if (ctx.query.authorized !== '1') {
    ctx.status = 401; // Unahuthorized
    return;
  }

  // Promise 가 끝난 다음 콘솔에 END 기록
  await next();
  console.log('END');
});

app.use((ctx, next) => {
  console.log(2);
  next();
});

app.use((ctx) => {
  ctx.body = 'hello world';
});

app.listen(4000, () => {
  console.log('Listening to port 4000');
});
```
> 이전과 동일하게 동작함      
> Listening to port 4000    
> /?authorized=1    
> 1   
> 2   
> END   


<br>

# 21.4 nodemon 사용하기
코드 변경할 때마다 **서버를 자동으로 재시작**하게 해주는 도구

`yarn add --dev nodemon` : 개발용 의존 모듈로 설치 후 package.json에 명령어 등록

package.json 일부
```javascript
(...)
"scripts": {
    "start": "node src",
    "start:dev": "nodemon --watch src/ src/index.js"
  }
```
> `start` 스크립트: 서버 시작   
> `start:dev` 스크립트: **nodemon**을 통해 서버를 실행해주는 명령어 → src 디렉터리 주시하다가 **디렉터리 내부의 파일 병경시 감지후 src/index.js 파일 재시작**함



<br>

# 21.5 koa-router 사용하기
**다른 주소로 요청**이 들어왔을 때 **다른 작업을 처리할 수 있도록** 라우터 사용해야 함

`yarn add koa-router` : koa-router 모듈 설치

### 기본 사용법
라우터를 불러오고 적용하는 방법

index.js
```javascript
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

// router 설정
router.get('/', (ctx) => {
  ctx.body = 'HOME';
});
router.get('/about', (ctx) => {
  ctx.body = 'ABOUT';
});

// app 인스턴스에 라우터 적용
app.use(router.routes()).use(router.allowedMethods);

app.listen(4000, () => {
  console.log('Listening to port 4000');
});
```
> `router.get` 메서드의 **파라미터**        
> ① **라우터의 경로**     
> ② 해당 라우트에 적용할 **미들웨어 함수**       
> HTTP 메서드: get, post, put, delete ...      

http://localhost:4000/ & http://localhost:4000/about 접속시 결과 확인 가능


### 라우트 파라미터와 쿼리
파라미터와 쿼리를 읽는 방법
- **파라미터**
    - `/about/:name` 형식: 콜론(:)을 사용해 라우트 경로 설정 
    - `/about/:name?` 형식: 파라미터가 있을 수도 있고 없을 수도 있는 경우, 물음표 사용
    - `ctx.params` 객체에서 조회 
- **쿼리**
    - `/posts/?id=10` 형식
    - `ctx.query` 객체에서 조회
    - 쿼리 문자열을 자동으로 **객체 형태로 파싱** 


```javascript
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

// router 설정
router.get('/', (ctx) => {
  ctx.body = '홈';
});

router.get('/about/:name?', (ctx) => {
  const { name } = ctx.params;
  // name 존재 유무에 따른 결과 출력
  ctx.body = name ? `${name}의 소개` : '소개';
});

router.get('/posts', (ctx) => {
  const { id } = ctx.query;
  // id 존재 유무에 따른 결과 출력
  ctx.body = id ? `포스트 #${id}` : '포스트 아이디가 없습니다.';
});

// app 인스턴스에 라우터 적용
app.use(router.routes()).use(router.allowedMethods);

app.listen(4000, () => {
  console.log('Listening to port 4000');
});
```
> http://localhost:4000/about/seoyeon/ & http://localhost:4000/posts & http://localhost:4000/posts?id=531 접속 후 결과 확인
> 
> 파라미터와 쿼리 모두 **주소를 통해 특정 값을 받아 올 때** 사용    
> `파라미터`: **처리할 작업의 카테고리**를 받아오거나, 고유 ID 혹은 이름으로 **특정 데이터 조회**    
> `쿼리`: **옵션에 관련된 정보**를 받아옴   




### REST API
❌ 웹 브라우저에서 **DB에 직접 접속해 데이터 변경**하는 것은 보안상 문제!!    
👌🏻 **`REST API`** 만들어서 사용
  
DB |← <br> → 처리 | 서버 REST API | ← 데이터 조회, 생성, 삭제, 업데이트 요청 <br> → 응답 | 클라이언트
--|--|--|--|--|
> 클라이언트가 서버에 자신이 데이터 조회, 생성, 삭제, 업데이트 요청시   
> 서버는 필요한 로직에 따라 DB에 접근해 작업 처리

#### HTTP 메서드 종류
REST API 요청 종류에 따라 다른 HTTP 메서드 사용   
- `GET`: 데이터 **조회**
- `POST`: 데이터 **등록**, 인증 작업
- `DELETE`: 데이터 **삭제**
- `PUT`: 데이터를 새 정보로 통째로 **교체**
- `PATCH`: 데이터의 특정 필드 **수정**

REST API 설계시에는 API 주소와 메서드에 따라 어떤 역할을 하는지 쉽게 파악할 수 있도록 작성해야 함



### 라우트 모듈화
프로젝트에서 여러 종류의 라우터를 만드는데, index.js 한 파일에 작성시 유지보수의 어려움   
→ 라우터를 여러 파일에 분리시켜 작성하고, 이를 불러와 적용하는 방법


src/api/index.js
```javascript
const Router = require('koa-router');
const api = new Router();

api.get('/test', (ctx) => {
  ctx.body = 'test success!';
});

// 라우터 내보내기
module.exports = api;
```

src/index.js
```javascript
const Koa = require('koa');
const Router = require('koa-router');

const api = require('./api');

const app = new Koa();
const router = new Router();

// router 설정
router.use('/api', api.routes()); // api 라우트 적용 - 서버의 메인 라우터인 /api 경로로 설정  

// app 인스턴스에 라우터 적용
app.use(router.routes()).use(router.allowedMethods);

app.listen(4000, () => {
  console.log('Listening to port 4000');
});
```

> http://localhost:4000/api/test/ 에 접속 후 페이지에서 'test success!' 확인 가능



### posts 라우트 생성

src/api/posts/index.js - **posts 라우트 설정**
```javascript
const Router = require('koa-router');
const posts = new Router();

const printInfo = (ctx) => {
  ctx.body = {
    method: ctx.method, // 현재 요청의 메서드
    path: ctx.path, // 경로
    params: ctx.params, // 파라미터
  }; // JSON 객체 반환
};

// 여러 종류의 라우트 설정, printInfo 함수 호출
posts.get('/', printInfo);
posts.post('/', printInfo);
posts.get('/:id', printInfo);
posts.delete('/:id', printInfo);
posts.put('/:id', printInfo);
posts.patch('/:id', printInfo);

module.exports = posts;
```

src/api/index.js - **api 라우트에 posts 라우트 연결**
```javascript
const Router = require('koa-router');
const posts = require('./posts');

const api = new Router();

api.use('/posts', posts.routes());

// 라우터 내보내기
module.exports = api;
```
> <img width="319" alt="스크린샷 2021-06-16 오후 5 07 24" src="https://user-images.githubusercontent.com/55572222/122182549-567f6200-cec5-11eb-906c-cf1bebae0731.png">
> 
> GET 메서드 사용 API: 웹 브라우저에서 주소 입력해 테스팅   
> POST, DELETE, PUT, PATCH 메서드 사용 API: 자바스크립트로 호출 → **Postman 프로그램으로 쉽게 REST API 요청 테스팅** 가능    


#### Postman 프로그램 설치 및 사용

#### 컨트롤러 파일 작성
**컨트롤러**   
라우트 처리 함수들을 다른 파일로 따로 분리해서 관리할 수 있는데, 이 **라우트 처리 함수들을 모아놓은 파일**

자바스크립트의 배열 기능만 사용해 임시로 기능 구현!!

**`koa-bodyparser` 미들웨어**   
POST/PUT/PATCH 메서드의 Request Body에 JSON 형식으로 데이터를 넣어주면, 이를 파싱해 서버에서 사용 가능하게 함    
`yarn add koa-bodyparser`   

src/index.js
```javascript
const Koa = require('koa');
const Router = require('koa-router');
const bodyParser = require('koa-bodyparser'); // !주의: 코드 윗부분에 적용

const api = require('./api');

const app = new Koa();
const router = new Router();

// router 설정
router.use('/api', api.routes()); // api 라우트 적용

// router 적용 전에 bodyParser 적용
app.use(bodyParser());

// app 인스턴스에 라우터 적용
app.use(router.routes()).use(router.allowedMethods);

app.listen(4000, () => {
  console.log('Listening to port 4000');
});
```

posts/posts.ctrl.js
```javascript
let postId = 1; // id의 초깃값입니다.

// posts 배열 초기 데이터
const posts = [
  {
    id: 1,
    title: '제목',
    body: '내용',
  },
];

/* 포스트 작성
POST /api/posts
{ title, body }
*/
exports.write = (ctx) => {
  // REST API의 request body는 ctx.request.body에서 조회할 수 있습니다.
  const { title, body } = ctx.request.body;
  postId += 1; // 기존 postId 값에 1을 더합니다.
  const post = { id: postId, title, body };
  posts.push(post);
  ctx.body = post;
};

/* 포스트 목록 조회
GET /api/posts
*/
exports.list = (ctx) => {
  ctx.body = posts;
};

/* 특정 포스트 조회
GET /api/posts/:id
*/
exports.read = (ctx) => {
  const { id } = ctx.params;
  // 주어진 id 값으로 포스트를 찾습니다.
  // 파라미터로 받아 온 값은 문자열 형식이니 파라미터를 숫자로 변환하거나,
  // 비교할 p.id 값을 문자열로 변경해야 합니다.
  const post = posts.find((p) => p.id.toString() === id);
  // 포스트가 없으면 오류를 반환합니다.
  if (!post) {
    ctx.status = 404;
    ctx.body = {
      message: '포스트가 존재하지 않습니다.',
    };
    return;
  }
  ctx.body = post;
};

/* 특정 포스트 제거
DELETE /api/posts/:id
*/
exports.remove = (ctx) => {
  const { id } = ctx.params;
  // 해당 id를 가진 post가 몇 번째인지 확인합니다.
  const index = posts.findIndex((p) => p.id.toString() === id);
  // 포스트가 없으면 오류를 반환합니다.
  if (index === -1) {
    ctx.status = 404;
    ctx.body = {
      message: '포스트가 존재하지 않습니다.',
    };
    return;
  }
  // index번째 아이템을 제거합니다.
  posts.splice(index, 1);
  ctx.status = 204; // No Content
};

/* 포스트 수정(교체)
PUT /api/posts/:id
{ title, body }
*/
exports.replace = (ctx) => {
  // PUT 메서드는 전체 포스트 정보를 입력하여 데이터를 통째로 교체할 때 사용합니다.
  const { id } = ctx.params;
  // 해당 id를 가진 post가 몇 번째인지 확인합니다.
  const index = posts.findIndex((p) => p.id.toString() === id);
  // 포스트가 없으면 오류를 반환합니다.
  if (index === -1) {
    ctx.status = 404;
    ctx.body = {
      message: '포스트가 존재하지 않습니다.',
    };
    return;
  }
  // 전체 객체를 덮어씌웁니다.
  // 따라서 id를 제외한 기존 정보를 날리고, 객체를 새로 만듭니다.
  posts[index] = {
    id,
    ...ctx.request.body,
  };
  ctx.body = posts[index];
};

/* 포스트 수정(특정 필드 변경)
PATCH /api/posts/:id
{ title, body }
*/
exports.update = (ctx) => {
  // PATCH 메서드는 주어진 필드만 교체합니다.
  const { id } = ctx.params;
  // 해당 id를 가진 post가 몇 번째인지 확인합니다.
  const index = posts.findIndex((p) => p.id.toString() === id);
  // 포스트가 없으면 오류를 반환합니다.
  if (index === -1) {
    ctx.status = 404;
    ctx.body = {
      message: '포스트가 존재하지 않습니다.',
    };
    return;
  }
  // 기존 값에 정보를 덮어씌웁니다.
  posts[index] = {
    ...posts[index],
    ...ctx.request.body,
  };
  ctx.body = posts[index];
};
```
> `exports.이름 = ...` 형식으로 함수를 내보냄       
> `const 모듈이름 = require('파일이름');`     
> `모듈이름.이름();` 형식으로 불러옴     


컨트롤러 함수를 각 라우트에 연결하기      

src/api/posts/index.js
```javascript
const Router = require('koa-router');
const postsCtrl = require('./posts.ctrl');

const posts = new Router();

// 여러 종류의 라우트 설정, printInfo 함수 호출
posts.get('/', postsCtrl.list);
posts.post('/', postsCtrl.write);
posts.get('/:id', postsCtrl.read);
posts.delete('/:id', postsCtrl.remove);
posts.put('/:id', postsCtrl.replace);
posts.patch('/:id', postsCtrl.update);

module.exports = posts;
```
> write, replace, update API는 요청시 Request Body가 필요함!! → Postman에서 실습 가능   




<br>

# 21.6 정리

Koa를 사용해 백엔드 서버를 만들어봄!    

`REST API`를 살펴본 후, `자바스크립트 배열`을 사용해 어떻게 작동하는지를 구현했으나, 서버 재시작시 데이터가 소멸되는 단점!   
→ `MySQL`, `MongoDB` 등 데이터베이스에 정보를 저장해 관리하면, 많은 양의 데이터를 효율적으로 읽고 쓰기 가능함 (22장)


