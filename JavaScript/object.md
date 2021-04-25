# Objects

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object

<br> 


## Literals and properties
object는 key와 value의 집합체
```javascript
const obj1 = {}; // 'object literal' syntax
const obj2 = new Object(); // 'object constructor' syntax

function print(person) {
  console.log(person.name); // ellie
  console.log(person.age); // 4
}

const ellie = { name: "ellie", age: 4 };
print(ellie);

// with JavaScript magic (dynamically typed language)
// can add properties later
ellie.hasJob = true;
console.log(ellie.hasJob); // true

// can delete properties later
delete ellie.hasJob;
console.log(ellie.hasJob); // undefined
```
javascript는 **Dynamically typed language**로 Runtime시 동적으로 타입이 결정되는 언어   
뒤늦게 object의 property 추가/삭제가 가능함   
<br> 


## Computed properties 
object는 property를 .을 이용해 접근 가능   
computed properties 배열에서 데이터를 받아오는 것처럼 접근 가능    
**key 항상 string 타입**으로 받아와야 함  
- `.` : key의 value를 받아올 때 사용 
- `['']` (computed properties): 정확히 어떤 key가 필요할지 모를 때 (런타임에서 결정될 때 사용)  
- 실시간으로 원하는 key의 value를 받아오고 싶다면 computed property를 사용하면 됨!!  
```javascript
// key should be always string
console.log(ellie.name); // ellie
console.log(ellie["name"]); // ellie
ellie["hasJob"] = true;
console.log(ellie.hasJob); // true  

function printValue(obj, key) {
//   console.log(obj.key); // undefined
  console.log(obj[key]); 
}
printValue(ellie, "name"); // ellie
printValue(ellie, "age"); // 4
```
<br>


## Property value shorthand / Constructor Function 
```javascript
// Property value shorthand
const person1 = { name: "bob", age: 2 };
const person2 = { name: "steve", age: 3 };
const person3 = { name: "dave", age: 4 };
const person4 = new Person("elile", 30);
console.log(person4); // Person {name: "elile", age: 30}

// Constructor Function
function Person(name, age) {
  // this = {};
  this.name = name;
  this.age = age;
  // return this;
} 
```
> Object 생성 함수명은 대게 대문자로 시작     
> **Property value shorthand**    
> - key와 value의 이름이 동일하다면 한번만 작성해도 됨.    
> - `return {name: name, age: age};` <=> `return {name, age};`  
<br> 

## in operator: property existence check (key in obj)
```javascript
console.log("name" in ellie); // true
console.log("age" in ellie); // true
console.log("random" in ellie); // false
console.log(ellie.random); // undefined
```
> `undefined`: 정의하지 않은 key를 확인할 때  
> 

## `for ..in` vs `for ..of`
많이 사용됨!
```javascript
console.clear();
for (let key in ellie) {
  console.log(key); 
  // name
  // age
  // hasJob
}

// for (value of iterable)
const array = [1, 2, 4, 5];
for (let value of array) {
  console.log(value);
  // 1
  // 2
  // 3
  // 4
  // 5
}
```
<br>


## Cloning

```javascript
// Object.assign(dest, [obj1, obj2, obj3...])
const user = { name: "ellie", age: "20" };
const user2 = user;
user2.name = "coder";  
console.log(user); // {name: "coder", age: "20"}

// 1) old way (수동적으로 할당)
const user3 = {};
for (let key in user) {
  user3[key] = user[key];
}
console.clear();
console.log(user3); // {name: "coder", age: "20"}

// 2) new!
const user4 = Object.assign({}, user);
console.log(user4); // {name: "coder", age: "20"}
```


> user와 user2의 ref가 같은 obj를 참조함   
> 
> **object clone 방법**
> 1. `for .. in` 을 이용한 수동적 할당  
> 2. Object의 `assign()` 사용 


### another example
```javascript
const fruit1 = { color: "red" };
const fruit2 = { color: "blue", size: "big" };
const mixed = Object.assign({}, fruit1, fruit2);
console.log(mixed.color); // blue
console.log(mixed.size); // big
```
> `assign()`에서 파라미터에 동일한 property가 있는 경우 나중 파라미터의 property로 값을 덮어씌움










