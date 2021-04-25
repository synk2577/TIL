# Objects

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object


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
<br/> 

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
<br/>

























