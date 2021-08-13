# Function
- fundamental building block in the program
- subprogram can be used multiple times
- performs a task or calculates a value
<br> 

## Function declartion
- 구조: `function name (parm1, parm2) { body ... reurn; }`
- 오직 한가지 일만 담당
- 함수명은 동사형으로
- JS에서 function은 **Object**
```javascript 
function printHello() {
  console.log('Hello');
}
printHello();

function log(message) { 
  console.log(message);
}
log('Hello@');
log(1234);
```

### JS의 단점 vs. TS의 장점
```javascript
// JavaScript
"use strict";
function log(msg) {
    console.log(msg);
    return 0;
}
```
> JS에는 type이 없으므로 함수 자체의 **인터페이스만으로는 파라미터의 타입을 파악하기 어려움**   

```typescript
// TypeScript
function log(msg: string): number {
    console.log(msg);
    return 0;
}
```
> TS에서는 **params나 return의 타입을 명시**해야 함!!  
> 함수의 인터페이스에서 `함수명`, `파라미터`, `파라미터의 타입`, `리턴 타입`을 확인 가능함   
> 규모가 큰 프로젝트에서는 타입을 명시하는 TS를 사용하는 경우가 많음    
<br>

## Parameters
- **primitive parameters**: 메모리에 value가 그대로 저장되어 value가 전달됨
- **object parameters**: 메모리에 ref가 저장되므로 ref가 전달됨 
```javascript
function changeName(obj) {
  obj.name = 'coder';
}
const ellie = { name: 'ellie' };
changeName(ellie);
console.log(ellie); // {name: "code"}
```

### Parameters(매개변수) vs. Arguments(인자)
- **`Parameters`**: 함수 정의시 받는 변수 값
  ```javascript
  function funcName(param1, param2 ...) {
    ...
  }
  ```
- **`Arguments`**: 함수 호출시 전달되는 값
  ```javascript
  funcName(argu1, argu2, ...);
  ```


<br> 

## Default parameters (ES6)
파라미터의 기본값 설정 가능
```javascript
function showMessage(message, from = 'unknown') {
  console.log(`${message} by ${from}`);
}
showMessage('Hi!'); // Hi! by unknown
```
<br>

## Rest parameters (ES6)
`...`로 표현하며, 정해지지 않은 수의 인수를 배열 형태로 전달  

```javascript
function printAll(...args) {
  for (let i = 0; i < args.length; i++) {
    console.log(args[i]);
  }

  for (const arg of args) {
    console.log(arg);
  }

  args.forEach((arg) => console.log(arg));
}
printAll('dream', 'coding', 'ellie');
```
배열 원소 출력 방법
1. for loop 
2. for of 구문
3. forEach() 함수 사용
<br>

## Local scope
> scope? 
> "밖에서는 안이 보이지 않고 안에서만 밖을 볼 수 있다!"  

```javascript
let globalMessage = "global"; // global variable
function printMessage() {
  let message = "hello";
  console.log(message); // local variable
  console.log(globalMessage);
  function printAnother() {
    console.log(message);
    let childMessage = "hello";
  }
  // console.log(childMessage); //error
}
printMessage(); 
// hello
// global
```
<br>

## Return a value
모든 함수는 `undefined` or `value`를 return    
값을 return 하지 않는 함수는 `return undefined;`를 의미  
```javascript
function sum(a, b) {
  return a + b;
}
const result = sum(1, 2); // 3
console.log(`sum: ${sum(1, 2)}`);
```
<br>

## Early return, early exit
- Bad case) 
  - if문 내부에 특정 조건을 만족하는 로직을 작성
  - 가독성 떨어짐   
- Good case) 
  - if문 내부에 조건이 맞지 않는 경우를 작성해 early return하여 함수를 종료
  - if문 외부에서 조건이 맞을 때만 필요한 로직을 작성

```javascript
// bad
function upgradeUser(user) {
  if (user.point > 10) {
    // long upgrade logic...
  }
}

// good
function upgradeUser(user) {
  if (user.point <= 10) {
    return;
  }
  // long upgrade logic...
}
```
<br> 

# First-class function
- functions are treated like any other **variable**
- can be **assigned** as a value to variable
- can be **passed** as an argument to other functions.
- can be **returned** by another function
<br> 

## Function expression 
함수 선언과 동시에 할당   

Function expression | Function declaration
------------------- | -------------------
할당된 다음부터 호출 가능 | hosted 가능 (함수 선언 이전에 호출이 가능)

### anonymous function
```javascript 
const print = function () {
  // anonymous function
  console.log("print");
};
print();
const printAgain = print;
printAgain();
const sumAgain = sum;
console.log(sumAgain(1, 3));
```
<br> 

##  Callback function using function expression
Callback function : 전달된 함수를 부르는 것

```javascript
function randomQuiz(answer, printYes, printNo) {
  if (answer === "love you") {
    printYes();
  } else {
    printNo();
  }
}
// anonymous function
const printYes = function () {
  console.log("yes!");
};

// named function
// better debugging in debugger's stack traces
// recursions
const printNo = function print() {
  console.log("no!");
};
randomQuiz("wrong", printYes, printNo);
randomQuiz("love you", printYes, printNo);
```
> printYes(): anonymous function    
> printNo(): named function
> 
> **named function**
> 1. debugger's stack traces에서 함수의 이름을 알고자 할 때 사용   
> 2. 함수 내부에서 함수 자신을 또 호출할 때 사용 (재귀 호출)
> 
<br> 

## Arrow function
‼️ **always anonymous function**


```javascript
// const simplePrint = function () {
//   console.log('simplePrint!');
// };

const simplePrint = () => console.log("simplePrint!");
const add = (a, b) => a + b;
const simpleMultiply = (a, b) => {
  // do something more
  return a * b;
};
```
<br>

## IIFE: Immediately Invoked Function Expression
함수 선언시 나중에 따로 호출해줘야 함    
선언함과 동시에 호출하려면 `()`로 함수를 묶고 `();`를 작성!    
자주 쓰이지는 않음!  
```javascript
(function hello() {
  console.log("IIFE");
})();
```


