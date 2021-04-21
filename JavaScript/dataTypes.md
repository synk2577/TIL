# 데이터 타입
- global scope
  - 어디에서든 접근 가능
  - 애플리케이션이 실행되고 종료되는 순간까지 메모리에 탑재 
  - 최소한의 사용을 권장  
- local scope 
  - {} 내부에서만 접근 가능
<br>  

## Variable 
**r/w (read & write)**
### let
**변수 선언 키워드**  
ES6에 추가  
mutable data type: 값 변경 가능

### var
ES6 이전에 사용되었던 **변수 선언 키워드**  
#### 사용을 지양하는 추세    
⚠️ 변수를 선언하기 전에 값을 할당하거나 출력하는 행동이 가능함   
⚠️ block scope 없음 
#### var hosting
📍 선언 위치와 별개로 항상 제일 위로 선언을 끌어 올리는 것  
```javascript
// let (added in ES6)
let globalName = 'global name';
{
  let name = 'ellie';
  console.log(name);
  name = 'hello';
  console.log(name);
  console.log(globalName);
}
console.log(name);
console.log(globalName);

// var (don't ever use this!)
// var hoisting (move declaration from bottom to top)
// has no block scope
{
  age = 4;
  var age;
}
console.log(age);
```
<br>  

### ES6의 호환성
⭕️ Edge, Firefox, Chrome, Safari, Opera 모두 지원    
❌ IE는 지원 불가 -> BABEL을 이용해 배포시 ES5/4로 변환  
<br>  

## Constants 
**r (read only)**   
변수의 값을 할당하는 경우 포인터를 이용해서 값의 변경이 가능하지만, Constants의 경우 포인터가 잠겨 있어 **값을 선언함과 동시에 변경이 불가능**함   
immutable data type  

#### 장점
- security 
- thread safety: 여러 개의 스레드가 동시에 변수 값을 변경하는 행위를 막음
- reduce human mistakes

#### 변수 선언 키워드
Mutable | Immutable
--------|----------
let | const
<br>  

## Variable Types
### Primitive type  
- 더 이상 쪼개질 수 없는 single item  
- **값 자체**가 메모리에 저장 
- ex) number, string, boolean, null, undefined, symbol  
<br>  

### Object type
- 아이템을 묶어서 box container로 관리 
- ref를 통해서 실제로 object가 담겨 있는 메모리를 가르키므로 **레퍼런스**가 메모리에 저장됨
- ex) function, first-class function

  #### first-class function 이란?
  function이   
  1) 다른 데이터 타입처럼 변수 할당이 가능하며    
  2) 함수의 파라미터로 전달이 되고   
  3) return이 가능하다  
<br>  

### JS's data types for number
숫자 데이터 타입은 `number` 하나!  
숫자 값(integer, decimal number)에 상관 없이 데이터 타입은 모두 number  

  #### special numeric values
  - Infinity
  - negative Intinity
  - NaN (not a number)
  - bigint : 숫자 뒤에 n을 붙임
```javascript
// number
const count = 17; // integer
const size = 17.1; // decimal number
console.log(`value: ${count}, type: ${typeof count}`);
console.log(`value: ${size}, type: ${typeof size}`);

// number - speicla numeric values: infinity, -infinity, NaN
const infinity = 1 / 0;
const negativeInfinity = -1 / 0;
const nAn = 'not a number' / 2;
console.log(infinity);
console.log(negativeInfinity);
console.log(nAn);

// bigInt (fairly new, don't use it yet)
const bigInt = 1234567890123456789012345678901234567890n; // over (-2**53) ~ 2*53)
console.log(`value: ${bigInt}, type: ${typeof bigInt}`);
```
<br>  

### string
문자열  
문자열과 문자열의 더하기 가능  

  #### template literals (string)
  문자열과 값을 백틱기호를 사용하여 쉽게 작성
  ```javascript
  const brendan = 'brendan';
  const greeting = 'hello ' + brendan;
  console.log(`value: ${greeting}, type: ${typeof greeting}`);
  const helloBob = `hi ${brendan}!`; //template literals (string)
  ```
<br>

### boolean
참/거짓
- **false**: 0, null, undefined, NaN, ''
- **true**: any other value
#### null
empty라는 의미로 null로 값이 할당되어 있음
#### undefined
선언만 되어있고, 값이 지정되지 않은 것
```javascript 
// null
let nothing = null;
console.log(`value: ${nothing}, type: ${typeof nothing}`);

// undefined
let x; // let x = undefined;
console.log(`value: ${x}, type: ${typeof x}`);
```
<br> 

### symbol
map이나 다른 자료구조에서 고유한 식별자가 필요하거나 동시 다발적으로 일어나는 코드에서 우선순위를 부여할 때 **고유한 식별자**로 사용   
동일한 string으로 작성해도 다른 symbol로 인식!!   
동일한 symbol을 만들고 싶은 경우, `Symbol.for()` 사용  
console 출력시 `변수명.descripton` 으로 출력

```javascript 
// 주어지는 string에 상관없이 고유한 식별자로 사용
// symbol, create unique identifiers for objects
const symbol1 = Symbol('id');
const symbol2 = Symbol('id');
console.log(symbol1 === symbol2); // false
const gSymbol1 = Symbol.for('id');
const gSymbol2 = Symbol.for('id');
console.log(gSymbol1 === gSymbol2); // true
console.log(`value: ${symbol1.description}, type: ${typeof symbol1}`);
```
<br> 

## Dynamic Typing
JavaScript는 dynamically typed language로 선언시 어떤 타입인지 선언하지 않고 프로그램 동작시 할당된 값에 따라서 타입 변경이 가능   
프로토타입을 유연하게 사용할수 있다는 장점이 있지만, 다수의 개발자가 함께 작업하는 경우 문제가 발생하기도 함   
runtime에서 data type이 결정되기 때문에 error가 runtime으로 발생하는 경우가 많음!

### TS(TypeScript)의 등장
JavaScript 위에 type이 더해진 언어  
브라우저가 이해할 수 있는 JavaScript로 트랜스컴파일러를 이용해야 함 (BABEL)

