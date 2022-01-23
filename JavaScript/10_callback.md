# 콜백

## 동기와 비동기
- JavaScript is synchronous
- Execute the code block in order after hositing
  - hositing: `var`, `function` declaration (변수 선언의 끌어올림)

```javascript
'use strict';

console.log('1');
setTimeout(() => console.log('2'), 1000);
console.log('3');

// Synchronous callback
function printImmediatley(print) {
  print();
}
printImmediatley(() => {
  console.log('hello');
});

// Asynchronous callback
function printWidhDelay(print, timeout) {
  setTimeout(print, timeout);
}
printWidhDelay(() => console.log('aysnc callback'), 2000);

```

### hositing 결과 
```javascript
// Synchronous callback
function printImmediatley(print) {
  print();
}

// Asynchronous callback
function printWidhDelay(print, timeout) {
  setTimeout(print, timeout);
}

console.log('1'); // sync
setTimeout(() => console.log('2'), 1000); // async
console.log('3'); // sync
printImmediatley(() => {
  console.log('hello');
}); // sync
printWidhDelay(() => console.log('aysnc callback'), 2000); // async
```


## 콜백 지옥
```javascript
class UserStorage {
  loginUser(id, password, onSuccess, onError) {
    setTimeout(() => {
      if (
        (id === 'ellie' && password === 'dream') ||
        (id === 'coder' && password === 'academy')
      ) {
        onSuccess(id);
      } else {
        onError(new Error('not found'));
      }
    }, 2000);
  }

  getRoles(user, onSuccess, onError) {
    setTimeout(() => {
      if (user === 'ellie') {
        onSuccess({ name: 'ellie', role: 'admin' });
      } else {
        onError(new Error('no access'));
      }
    }, 1000);
  }
}

const userStorage = new UserStorage();
const id = prompt('enter your id');
const password = prompt('enter your password');
userStorage.loginUser(
  id,
  password,
  // 로그인 성공시
  (user) => {
    userStorage.getRoles(
      user,
      // 역할 출력
      (userWithRole) => {
        alert(
          `Hello ${userWithRole.name}, you haver a ${userWithRole.role} role`
        );
      },
      // 에러 발생
      (error) => {
        console.log(err);
      }
    );
  },
  // 로그인 실패시
  (error) => {
    console.log(error);
  }
);
```
### 콜백 지옥의 단점
- 가독성 최악
- 에러/디버깅시 유지보수의 어려움       
=> `async`/`await` 로 개선 가능
