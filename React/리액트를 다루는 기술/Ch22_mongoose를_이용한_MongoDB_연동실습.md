# 22.1 소개하기

서버 개발시 DB를 사용하면 웹 서비스에서 사용되는 **데이터를 저장**하고, **효율적으로 조회하거나 수정이 가능**함   

**RDBMS** (관계형 데이터 베이스)의 한계
- 데이터 스키마가 고정적
    - 새로 등록해야하는 데이터 형식이 있느 경우, 기존 데이터를 모두 수정해야 함 
- 확장성
    - 저장하고 처리할 데이터 양이 틀어나면 해당 DB 서버 성능을 업그레이드하는 방식을 사용 

문서지향적 **NoSQL DB**
- 유동적인 스키마
- 쉬운 확장
    - 서버 데이터 양이 늘어나도 여러 컴퓨터로 분산하여 처리


데이터 구조가 자주 바뀌는 환경은 **MongoDB**가 유리    
까다로운 조건으로 데이터를 필터링해야 하거나, ACID 특성을 지켜야 한다면 **RDMBS**가 더 유리    

※ ACID 특성: 원자성, 일관성, 고립성, 지소성으로 DB 트랜잭션이 안전하게 처리되는 것을 보장


### 문서 (documents)
RDBMS의 레코드(record)와 비슷한 개념    
- 문서의 데이터 구조는 한 개 이상의 키-값 쌍으로 구성
- 새로운 문서를 만들면 `_id` 고유값 자동 생성
- 다른 스키마를 가지는 문서들이 한 컬렉션(여러 문서가 들어있는 곳)에서 공존 가능

### MongoDB 구조
서버 > 데이터베이스 > 컬렉션 > 문서


### 스키마 디자인
#### RDBMS 
각 포스트, 댓글마다 테이블을 만들어 필요에 따라 조인    
<img src="./Images/ch22/01.png" width="300px" >

#### NoSQL
모든 것을 문서 하나에 넣음

```sql
{
  _id: ObjectId,
  title: String,
  body: String,
  username: String,
  createDate: Date,
  comments: [
    {
      _id: ObjectId,
      text: String,
      createDate: Date,
    },
  ],
};
```

- 서브다큐먼트 (subdocument)
    - 문서 내부에 또 다른 문서가 위치함
    - 일반 문서를 다루는 것처럼 쿼리 가능
- 문서 하나에 최대 16MB 데이터 용량 (100자 댓글 데이터 기 68,000개)  


<br>

# 22.2 MongoDB 서버 준비
설치과정 생략



<br>

# 22.3 mongoose의 설치 및 적용

### `mongoose`
Node.js 환경에서 사용하는 **MongoDB 기반 ODM(Object Data Modelling) 라이브러리**     
DB 문서들을 자바스크립트 객체처럼 사용 가능   

### dotenv
**환경변수들을 파일에 넣고 사용**할 수 있게 하는 개발 도구   
mongoose를 사용해 MongoDB 서버 접속시, 서버에 주소나 계정 및 비밀번호가 필요한 경우 민감하거나 환경별로 달라질 수 있는 값을 코드 안에 직접 작성하지 않고 **환경변수로 설정**하는 것이 좋음    
프로젝트를 github에 올릴 때, `.gitignore`를 작성해 환경변수 파일을 제외해야 함

`yarn add mongoose dotenv`: 프로젝트 디렉터리에 mongoose와 dotenv 설치     

### 환경변수 파일
.env (황경변수 파일) - 프로젝트 루트 경로
```
PORT=4000
MONGP_URI="mongodb://localhost:27017/blog"
```
> `blog`: 사용할 DB 이름

### mongoose로 서버와 DB 연결
src/index.js
```javascript
require('dotenv').config(); // .env 파일의 정보를 불러옴7
const Koa = require('koa');
const Router = require('koa-router');
const bodyParser = require('koa-bodyparser');
const mongoose = require('mongoose'); 

const api = require('./api');

// 비구조화 할당을 통하여 process.env 내부 값에 대한 레퍼런스 만들기
const { PORT, MONGO_URI } = process.env;

// connect() 함수를 이용해 서버와 DB 연결
mongoose
  .connect(MONGO_URI, {
    useNewUrlParser: true,
    useFindAndModify: false,
  })
  .then(() => {
    console.log('Connected to MongoDB');
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

// app 인스턴스에 라우터 적용
app.use(router.routes()).use(router.allowedMethods());

// PORT 가 지정되어있지 않다면 4000 을 사용
const port = PORT || 4000;
app.listen(port, () => {
  console.log('Listening to port %d', port);
});
```

<br>

# 22.4 esm으로 ES 모듈 import/export 문법 사용 

Node.js에서 import/export 정식 지원 x     
- `.mjs` 확장자를 사용하고 node 실행시 `--experimental-modules 옵션`을 넣어주면 사용이 가능함
- `esm 라이브러리`를 이용해 VSCode에서 자동완성을 통해 모듈을 쉽게 부를수도 있음
    - `yarn add esm`



<br>

# 22.5 데이터베이스의 스키마 모델

### 스키마 (schema)
컬렉션이 들어가는 문서 내부의 **각 필드가 어떤 형식으로 되어 있는지 정의**하는 객체

### 모델 (model)
**스키마를 사용해 만드는 인스턴스**로 실제 작업을 처리하는 함수를 지닌 객체       

`스키마 → 모델 → 데이터베이스`


### 스키마 생성
models 디렉터리: 유지보수를 위해 스키마와 모델 관련 코드를 모아서 작성

src/models/post.js
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
});
```
> mongoose 모듈의 Schema를 사용하여 정의      

#### Schema에서 지원하는 타입
타입 | 설명
----|----
String | 문자열
Number | 숫자
Date | 날짜
Buffer | 파일을 담을 수 있는 버퍼
Boolean | true 또는 false 값
Mixed (Schema.Types.Mixed) | 어떤 데이터도 담을 수 있는 형식
ObjectId (Schema.Types.ObjectsId) | 객체 아이디. 주로 다른 객체 참조시 사용
Array | 배열 형태의 값으로 []로 감싸서 사용


### 모델 생성
src/models/post.js
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
});

const Post = mongoose.model('Post', PostSchema); // 스키마 이름, 스키마 객체
export default Post;
```
> **`mongoose.model 함수`** 로 모델 생성        
> 첫번째 파라미터: `스키마 이름`      
> 두번째 파라미터: `스키마 객체`      
> DB는 스키마 이름을 정하면, **그 이름의 복수형태로 DB 컬레션에 이름을** 만듦


<br>

# 22.6 MongoDB Compass의 설치 및 사용


<br>

# 22.7 데이터 생성과 조회 

MongoDB에 데이터를 등록해 데이터 보존하기          

src/api/posts/posts.ctrl.js
```javascript
import Post from '../../models/post'; 

export const write =  (ctx) => {}; // POST /api/posts - 포스트 작성

export const list =  (ctx) => {}; // GET /api/posts - 데이터 조회

export const read =  (ctx) => {}; // GET /api/posts/:id - 특정 포스트 조회

export const remove = (ctx) => {}; // DELETE /api/posts/:id - 데이터 삭제

export const update = (ctx) => {}; // PATCH /api/posts/:id - 데이터 수정
```
> 기존 PUT 메서드에 연결했던 `replace`는 구현하지 않음!

### 데이터 생성

src/api/posts/posts.ctrl.js - write
```javascript
export const write = async (ctx) => {
  const { title, body, tags } = ctx.request.body;
  const post = new Post({ // new 키워드
    title,
    body,
    tags,
  });

  try {
    await post.save(); // save 함수 
    ctx.body = post;
  } catch (e) {
    ctx.throw(500, e);
  }
};
```
> `new` 키워드: 포스트 인스턴스 생성      
> 생성자 함수의 파라미터: 정보를 지닌 객체   
>     
> `save()` 함수: 인스턴스가 DB에 저장          
> `save()` 반환값이 `Promise`이므로 `async/await` 문법 사용 → DB 저장 요청을 완료할 때까지 await를 사용해 대기함        


### 데이터 조회

src/api/posts/posts.ctrl.js - list
```javascript
export const list = async (ctx) => {
  try {
    const posts = await Post.find().exec(); // find().exec(): 서버에 쿼리 요청
    ctx.body = posts;
  } catch (e) {
    ctx.throw(500, e);
  }
};
```
> 모델 인스턴스의 `find()` 함수: 데이터 조회시 사용 
> `find().exec()`: 서버에 쿼리 요청



### 특정 포스트 조회

src/api/posts/posts.ctrl.js - read
```javascript
export const read = async (ctx) => {
  const { id } = ctx.params;
  try {
    const post = await Post.findById(id).exec(); // findById(): 특정 id를 가진 데이터 조회
    if (!post) {
      ctx.status = 404;
      return;
    }
    ctx.body = post;
  } catch (e) {
    ctx.throw(500, e);
  }
};
```    
> `findById()`: 특정 id를 가진 데이터 조회        
> 404 error; id가 존재하지 않는 경우     
> 500 error; 전달받은 id가 ObjectId 형태가 아니여서 발생하는 오류     




<br>

# 22.8 데이터 삭제와 수정

### 데이터 삭제
데이터 삭제 관련 함수
- `remove()`: 특정 조건을 만족하는 데이터 모두 지움
- `findByIdAndRemove()`: id를 찾아서 지움
- `findOneAndRemove()`: 특정 조건을 만족하는 데이터 하나를 찾아서 제거

```javascript
export const remove = async (ctx) => {
  const { id } = ctx.params;
  try {
    await Post.findByIdAndRemove(id).exec();
    ctx.status = 204; // No Content (성공했지만 응답 데이터 없음)
  } catch (e) {
    ctx.throw(500, e);
  }
};
```
> 특정 id를 DELETE 한 후, 동일한 주소로 GET 요청시 404 오류 발생 (Not Found)      


### 데이터 수정
`findByIdAndUpdate()` 함수 이용 
- 첫번째 파라미터: `id`        
- 두번째 파라미터: `업데이트 내용`       
- 세번째 파라미터: `업데이트 옵션`       

```javascript
export const update = async (ctx) => {
  const { id } = ctx.params;
  try {
    const post = await Post.findByIdAndUpdate(id, ctx.request.body, {
      new: true, // true: 업데이트된 데이터 반환
      // false: 업데이트되기 전의 데이터 반환
    });
    if (!post) {
      ctx.status = 404;
      return;
    }
  } catch (e) {
    ctx.throw(500, e);
  }
};
```
> PATCH 메서드는 데이터의 일부만 업데이트도 가능함!



<br>

# 22.9 요청 검증 

### ObjectId 검증
id가 올바른 ObjectId 형식이 아닌 경우; **`500 오류`** 발생 (보통 서버에서 처리하지 않아 내부적 문제가 생긴 경우 발생하는 오류임)        
잘못된 id의 전달은 클라이언트가 잘못된 요청을 보낸 것; **`400 Bad Request 오류`** 를 띄워야 함       

#### id값이 올바른 ObjectId 형식인지 확인하는 방법        
```javascript
import mongoose frmo 'mongoose';

const { ObjectId } = mongoose.Types;
ObjectId.isValid(id);
```
> `ObjectId`를 검증이 필요한 API는 `read`, `remove`, `update` 3가지!!      
> **검증 코드의 중복을 피하기 위해** posts.ctrl.js에 **`미들웨어`**로 작성       

src/api/posts/posts.ctrl.js
```javascript
import Post from '../../models/post';
import mongoose from 'mongoose';
import { ObjectID } from 'bson';

// 미들웨어 - ObjectId 검증
const { ObjectId } = mongoose.Types;

export const checkObjectId = (ctx, next) => {
  const { id } = ctx.params;
  if (!ObjectId.isValid(id)) {
    ctx.status = 400; // Bad request
    return;
  }
  return next();
};

(...)
```

src/api/posts/index.js
```javascript
import Router from 'koa-router';
import * as postsCtrl from './posts.ctrl';

const posts = new Router();

// 여러 종류의 라우트 설정, printInfo 함수 호출
posts.get('/', postsCtrl.list);
posts.post('/', postsCtrl.write);
posts.get('/:id', postsCtrl.checkObjectId, postsCtrl.read);
posts.delete('/:id', postsCtrl.checkObjectId, postsCtrl.remove);
// posts.put('/:id', postsCtrl.replace);
posts.patch('/:id', postsCtrl.checkObjectId, postsCtrl.update);

// 리팩토링 
// const post = new Router(); // /api/posts/:id 경로를 위한 라우터
// post.get('/', postsCtrl.read);
// post.delete('/', postsCtrl.remove);
// // posts.put('/:id', postsCtrl.replace);
// post.patch('/', postsCtrl.update);

// posts.use('/:id', postsCtrl.checkObjectId, post.routes());

export default posts;
```

→ **일반 ObjectId의 문자열 길이가 다른 잘못된 id를 넣는 경우**, 500이 아닌 `400 Bad Request` 발생!! (ex. aaaa)


### Request Body 검증
write, update API에서 전달받은 요청 내용을 검증해야 함!         
- 포스트 작성시 서버는 title, body, tags 값을 모두 전달 받아야 함
- 클라이언트가 값을 빼먹으면 400 오류가 발생     

[`Joi 라이브러리`](https://github.com/sideway/joi)를 이용해 이를 수월하게 처리하기         
`yarn add @hapi/joi`


src/api/posts/posts.ctrl.js
```javascript
import Post from '../../models/post';
import mongoose from 'mongoose';
import Joi from '@hapi/joi';

(...)

export const write = async (ctx) => {
  const schema = Joi.object().keys({
    // 객체가 다음 필드를 가지고 있음을 검증
    title: Joi.string().required(), // required(): 필수 항목
    body: Joi.string().required(),
    tags: Joi.array().items(Joi.string()).required(), // 문자열로 이루어진 배열
  });

  // 검증하고나서 검증 실패인 경우 에러 처리
  const result = schema.validate(ctx.request.body);
  if (result.error) {
    ctx.status = 400; // Bad request
    ctx.body = result.error;
    return;
  }

  const { title, body, tags } = ctx.request.body;
  const post = new Post({
    title,
    body,
    tags,
  });

  try {
    await post.save();
    ctx.body = post;
  } catch (e) {
    ctx.throw(500, e);
  }
};

// GET /api/posts - 데이터 조회
export const list = async (ctx) => {
  try {
    const posts = await Post.find().exec(); // find().exec(): 서버에 쿼리 요청
    ctx.body = posts;
  } catch (e) {
    ctx.throw(500, e);
  }
};
```        
> 문자열을 전달해야 하는 title 값에 숫자 전달시 에러 발생 



<br>

# 22.10 페이지네이션 구현










<br>

# 22.11 정리








