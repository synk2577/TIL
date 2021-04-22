[TOC]

# 연산자
## String concatenation
```javascript
console.log('my' + ' cat'); // my cat
console.log('1' + 2); // 12
console.log(`string literals: 1 + 2 = ${1 + 2}`); // string literals: 1 + 2 = 3
```

strirng literals에서는 특수기호나 줄바꿈 사용이 가능

## Numeric operators
```javascript
console.log(1 + 1); // add
console.log(1 - 1); // substract
console.log(1 / 1); // divide
console.log(1 * 1); // multiply
console.log(5 % 2); // remainder
console.log(2 ** 3); // exponentiation
```

## Increment/Decrement operators
```javascript
let counter = 2;
const preIncrement = ++counter;
// counter = counter + 1;
// preIncrement = counter;
console.log(`preIncrement: ${preIncrement}, counter: ${counter}`); // 3 3
const postIncrement = counter++;
// postIncrement = counter;
// counter = counter + 1;
console.log(`postIncrement: ${postIncrement}, counter: ${counter}`); // 3 4
const preDecrement = --counter;
console.log(`preDecrement: ${preDecrement}, counter: ${counter}`); // 3 3
const postDecrement = counter--;
console.log(`postDecrement: ${postDecrement}, counter: ${counter}`); // 3 2
```

## Assignment operators
```javascript
let x = 3;
let y = 6;
x += y; // x = x + y;
x -= y;
x *= y;
x /= y;
```

## Comparison operators
```javascript
console.log(10 < 6); // less than
console.log(10 <= 6); // less than or equal
console.log(10 > 6); // greater than
console.log(10 >= 6); // greater than or equal
```

## Logical operators

### || (or)
조건 중 하나라도 만족시, `true` 반환   
첫번째 연산 결과가 true인 경우, 더 이상 연산을 진행하지 않고 true 반환    
간단한 연산을 앞쪽에, 복잡한 연산을 뒤쪽에 배치하는 것이 효율적   
### && (and)
모든 조건이 true일 경우, `true` 반환    
즉, 하나라도 false일 경우, `false` 반환  
null check시 많이 사용  
### ! (not)
true는 false로, false는 true로 값을 반대로 변경

```javascript
const value1 = true;
const value2 = 4 < 2; // false

// || (or), finds the first truthy value
console.log(`or: ${value1 || value2 || check()}`); // or: true

// && (and), finds the first falsy value
console.log(`and: ${value1 && value2 && check()}`); // and: false  

// often used to compress long if-statement
// nullableObject && nullableObject.something
// if (nullableObject != null) {
//   nullabelObject.something;
// }

function check() {
  for (let i = 0; i < 10; i++) {
    //wasting time
    console.log('😱');
  }
  return true;
}

// ! (not)
console.log(!value1); // false
```

## Equality
### == (loose equality)
타입을 변경해 검사 
### === (strict equality)
타입이 다르면 다른 값으로 판단   
`===` 사용을 권장

```javascript
const stringFive = '5';
const numberFive = 5;

// == loose equality, with type conversion
console.log(stringFive == numberFive); // true
console.log(stringFive != numberFive); // false

// === strict equality, no type conversion
console.log(stringFive === numberFive); // false
console.log(stringFive !== numberFive); // true 
```

### ⚠️ **Object** Equality 검사할 때 주의 ⚠️    
Object는 메모리에 탑재될 때 reference 형태로 저장   
```javascript
// object equality by reference
const ellie1 = { name: 'ellie' };
const ellie2 = { name: 'ellie' };
const ellie3 = ellie1;
console.log(ellie1 == ellie2); // false
console.log(ellie1 === ellie2); // false
console.log(ellie1 === ellie3); // true
```
> ellie1과 ellie2에는 똑같은 데이터가 들어있는 Object지만 메모리에는 각각 다른 reference가 들어있음    
> 각각의 reference는 서로 다른 Object를 가르키고 있음   
> ellie3은 ellie1의 reference가 할당되어 똑같은 reference를 가짐   

### Quiz
```javascript 
// equality - puzzler
console.log(0 == false);
console.log(0 === false);
console.log('' == false);
console.log('' === false);
console.log(null == undefined);
console.log(null === undefined);
```
> **풀이**   
> 1번) `0`, `null`, `undefined`, `NaN`, `''`는 false로 간주      
> 2번) `0`은 boolean 타입이 아니기 때문에 false    
> 3번) empty string(`''`)은 false  
> 4번) empty string(`''`)은 boolean 타입이 아니므로 false  
> 5번) `null`과 `undefined`는 동일한 것으로 간주되므로 true ⚠️  
> 6번) `null`과 `undefined`는 다른 타입이므로 false   

## Conditional operators
### if, else if, else 
```javascript
// if, else if, else
const name = 'df';
if (name === 'ellie') {
  console.log('Welcome, Ellie!');
} else if (name === 'coder') {
  console.log('You are amazing coder');
} else {
  console.log('unkwnon');
}
// Welcome, Ellie!
```

## Ternary operator 
### condition ? vlaue1 : value2
`console.log(name === 'ellie' ? 'yes' : 'no');`

## Switch statement
```javascript
// use for multiple if checks
// use for enum-like value check
// use for multiple type checks in TS
const browser = 'IE';
switch (browser) {
  case 'IE':
    console.log('go away!');
    break;
  case 'Chrome':
  case 'Firefox':
    console.log('love you!');
    break;
  default:
    console.log('same all!');
    break;
}
```

## Loops
### while loop
while에서 조건을 검사한 후, {} 내부에서 행동 실행   
=> 조건문을 만족할 때만 블럭을 실행  
```javascript
// while loop, while the condition is truthy,
// body code is executed.
let i = 3;
while (i > 0) {
  console.log(`while: ${i}`);
  i--;
}
```

### do-while loop
do 블럭에서 행동을 먼저 한 후, while에서 조건 검사  
```javascript
// do while loop, body code is executed first,
// then check the condition.
do {
  console.log(`do while: ${i}`);
  i--;
} while (i > 0);
```

### for loop
`for (begin; condition; step)`
```javascript
// for loop, for(begin; condition; step)
for (i = 3; i > 0; i--) {
  console.log(`for: ${i}`);
}

for (let i = 3; i > 0; i = i - 2) {
  // inline variable declaration
  console.log(`inline variable for: ${i}`);
}
```

### nested loop
시간복잡도가 O(n^2)이므로 cpu에 좋지 않으므로 되도록 피하는 것이 좋음  
```javascript
// nested loops
for (let i = 0; i < 10; i++) {
  for (let j = 0; j < 10; j++) {
    console.log(`i: ${i}, j:${j}`);
  }
}
```

### break, continue
#### break
loop를 완전히 끝냄
#### continue 
현재 단계를 skip하고, 다음 단계로 넘어감 
```javascript
// Q1. iterate from 0 to 10 and print only even numbers (use continue)
for (let i = 0; i <= 10; i++) {
  if (i % 2 !== 0) continue;
  console.log(`i: ${i}`);
}

// Q2. iterate from 0 to 10 and print numbers until reaching 8 (use break)
for (let i = 0; i <= 10; i++) {
  if (i === 8) break;
  console.log(`i: ${i}`);
}
```

### label
`break`나 `continue`구문과 함께 사용   
요즘은 사용하지 않는 추세  







