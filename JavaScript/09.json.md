# JSON

## HTTP

HyperText Transfer Protocol      

클라이언트가 어떻게 서버와 통신할지 정리한 것 = 통신 프로토콜 규약     

- Clinet → `request` → Server
- Client ← `response` ← Server
- HyperText: Link뿐만 아니라 문서, 이미지 파일을 포함한 것

http를 이용해 서버에게 데이터를 요청해서 받아오는 방법 = **`AJAX`**

## AJAX

Asynchronous JavaScript And XML   

웹 페이지에서 동적으로 서버에게 데이터를 주고 받을 수 있는 기술     

ex) XHR (XMLHttpRequest)

: 브라우저 api에서 제공하는 obj 중 하나로 간단하게 서버에게 데이터를 요청하고 받아올 수 있음 

`fetch()` API: 간편하게 데이터를 주고받을 수 있음 (최근 추가: IE 미지원)

## XML

Markup Language 중 하나    

태그를 이용해 데이터를 표현      

서버와 클라이언트 사이에서 데이터 전송할 때, XML 뿐만 아니라 다양한 타입의 파일을 주고 받음      

`XMLHttpRequest` obj: 데이터를 주고받을 수 있으나, 파일 사이즈가 커지고 가독성이 좋지 않아 사용하지 않는 추세   

## JSON

JavaScript Object Notation      

ECMAScript 3rd 1999에서 영감을 받아 만들어진 데이터 포맷   

Object와 같이 `{ key : value }` 로 이루어짐     

브라우저 뿐만 아니라 모바일에서 서버와 데이터를 주고 받을 때, 서버와 통신하지 않아도 object를 파일 시스템에 저장할 때도 많이 이용하고 있음 

- simplest data interchange format
- lightweight text-based structure
- easy to read
- key-value pairs
- used for serialization and transmission of data between the network the network connection
- independent programming language and platform


## How to Study JSON?

1. object를 어떻게 serialize(직렬화)하여 JSON으로 변환할지
2. 직렬화된 JSON을 deserialize하여 object로 변환할지

## Object to JSON

```jsx
// stringify(obj)
let json = JSON.stringify(true);
console.log(json); // true

json = JSON.stringify(['apple', 'banana']);
console.log(json); // ["apple","banana"]

const rabbit = {
  name: 'tori',
  color: 'white',
  size: null,
  birthDate: new Date(),
  jump: () => {
    console.log(`${name} can jump!`);
  },
};
// a) stringify(value)
json = JSON.stringify(rabbit);
console.log(json); // {"name":"tori","color":"white","size":null,"birthDate":"2022-01-09T06:55:31.712Z"}
// jump(): object의 data가 아니기 때문에 제외
// Symbol(): javascript에만 있는 특별한 데이터도 제외

// b) stringify(value, array)
json = JSON.stringify(rabbit, ['name']); // 두번째 인자: 배열 -> 원하는 프로퍼티만 stringify 가능
console.log(json); // {"name":"tori"}

// c) stringify(value, callback)
console.clear();
json = JSON.stringify(rabbit, (key, value) => {
  console.log(`key: ${key}, value: ${value}`);
  return value;
  // return key === 'name' ? 'ellie' : value; // 세밀하게 조정 가능
});
console.log(json);
// key: , value: [object Object] // 최상위 obj
// key: name, value: tori
// key: color, value: white
// key: size, value: null
// key: birthDate, value: 2022-01-09T07:03:48.616Z
// key: jump, value: () => {
//     console.log(`${name} can jump!`);
//   }
// {"name":"tori","color":"white","size":null,"birthDate":"2022-01-09T07:03:48.616Z"}
```

## JSON to Object

```jsx
console.clear();
json = JSON.stringify(rabbit);
console.log(json);
const obj = JSON.parse(json, (key, value) => {
  console.log(`key: ${key}, value: ${value}`);
  // return value;
  return key === 'birthDate' ? new Date(value) : value; // ***
});
console.log(obj);
// {
//   name: 'tori',
//   color: 'white',
//   size: null,
//   birthDate: '2022-01-09T07:07:16.729Z'
// }

rabbit.jump();
// obj.jump(); // TypeError
// obj to json; stringify only data (not function)
// -> json to obj: function을 가지지 않음

console.log(rabbit.birthDate.getDate());
console.log(rabbit.birthDate.getDate()); // ***
```

## Site about JSON

JSON Diff checker: [http://www.jsondiff.com/](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa0pFeFVHc1FtdWJkbGxteE9rbV9BT19hYkdoQXxBQ3Jtc0ttY29xR3hweTkyMWlYbDR0N3hXUk1PdmNVNFlJWDh5Ym1veWw4NFNpajhMeGpLQzY2blJNdVg5MXdxLUdWa1J1LUpyVUJVdU1Za0NaNHBBNFota3JGR3Q2UUlIVTJVeGF0eHN3eUtCdldHTGZ2Q1BESQ&q=http%3A%2F%2Fwww.jsondiff.com%2F&v=FN_D4Ihs3LE)

  JSON 파일들이 다른 점을 찾아줌

JSON Beautifier/editor: [https://jsonbeautifier.org/](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbU9JNnlTUkRMcGlHTVBFRVpCU2N6aXZMSTFOd3xBQ3Jtc0trNDk5YzI0Q2l2R3J1WXZYZ0NobUVMX0h1WDVjd3lkVFlscWZ1UklUSlF1VE1HVEtCcGVISmZPbDdJczFXQWJ3SHp0d1BZRWNaa1ZFd1R4QURPMXNpV1RVVzRtUGRZNXpsaFVrWXJtX0h3eG1pLTROQQ&q=https%3A%2F%2Fjsonbeautifier.org%2F&v=FN_D4Ihs3LE)

  JSON format을 보기 쉽게 정리해줌 

JSON Parser: [https://jsonparser.org/](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbjJOakV1UFk5T05UWHNBc0FobzBocEFzZFZMUXxBQ3Jtc0tuMVF6LXA2TGI1akNFYmtpNnkwSmZXdExzOFVzM2czaTQ0eUVfTnB4MWRERHZ3ai1jdkpjdVh3RmZvUU5BSmtKTzc4OEJvTks3NmFYbk9SeE4yaUFxa0ZKZld1NzRfRC1RYnZsaXpMQ2tmRHNPeGFvdw&q=https%3A%2F%2Fjsonparser.org%2F&v=FN_D4Ihs3LE)

  JSON type을 obj 형태로 볼 수 있음 

JSON Validator: [https://tools.learningcontainer.com/j...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa0tkMWhFcXoyWEpfVXNabFNsVEZIeUd4MUdwd3xBQ3Jtc0ttaThtektOVTZvOUw4TEZocUJhMzkzTlJBNkRCN1psQ2hBNzB5bWF6VklCUmpwUi15STBTUW05ZXIzSmlSTWJ3OUdBLUIzUHV1QUc3NWJ1b1ZjekNORF9fdFlxZjVoc3JPMFgxdHRLVV9vRTJVRHp5SQ&q=https%3A%2F%2Ftools.learningcontainer.com%2Fjson-validator%2F&v=FN_D4Ihs3LE)

  JSON의 유효성 검사
