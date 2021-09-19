# Class vs Object

## class
- fildes + methods  
- template
- declare once
- no data in
<br>
 
 
## object
- instance of a class
- created many times
- data in 
<br>


## JavaScript classes
### ~ES5
- class를 정의하지 않고 object 생성이 가능했음 
- function을 이용해 template을 만들 수 있었음
### ES6
- introduced in ES6
- syntactical sugar over prototype-based inheritance (기존 프로토타타입 기반)
<br>


## Class declarations
- **생성자**: object 생성시 필요한 데이터 전달  
- **필드**: 전달된 데이터 할당  

```javascript
class Person {
  // constructor
  constructor(name, age) {
    // fields
    this.name = name;
    this.age = age;
  }

  // methods
  speak() {
    console.log(`${this.name}: hello!`);
  }
}

const ellie = new Person("ellie", 20);
console.log(ellie.name); // ellie
console.log(ellie.age); // 20
ellie.speak(); // ellie: hello!
```
<br>

## Getter and Setter
- **get**: 값 리턴
- **set**: 값 설정
```javascript
class User {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }

  get age() {
    return this._age;
  }

  set age(value) {
    // if (value < 0) {
    //   throw Error('age can not be negative');
    // }
    this._age = value < 0 ? 0 : value;
  }
}

const user1 = new User("Steve", "Job", -1);
console.log(user1.age); // 0
```
> `this.age`는 `get age()`를 호출하고, `= age;`는 `set age(value)`를 호출  
> setter 내부에서 value를 this.age에 할당할 때 메모리 값을 업데이트하는 것이 아닌 **setter를 호출**하게 됨  
> 즉, setter가 setter를 재호출하는 무한반복 상황으로 call stack이 차버림(max)  
**> 따라서 getter/setter에서는 `this._age` 형식으로 변수를 지정**

<br>


## Fields (public, private)
아주 최근에 추가되어 지원안되는 브라우저 존재
```javascript
class Experiment {
  publicField = 2; // 외부에서 접근 가능
  #privateField = 0; // 클래스 내부에서만 접근 
}
const experiment = new Experiment();
console.log(experiment.publicField); // 2
console.log(experiment.privateField); // undefined
```
<br> 


## Static properties and methods
object와 상관없이 **공통적으로 class에서 사용**되는 것을 **static/static method를 이용**하는 것이 메모리 사용을 줄임
```javascript
class Article {
  static publisher = "Dream Coding";
  constructor(articleNumber) {
    this.articleNumber = articleNumber;
  }

  static printPublisher() {
    console.log(Article.publisher);
  }
}

const article1 = new Article(1);
const article2 = new Article(2);
console.log(article1.publisher); // undefined
console.log(Article.publisher); // Dream Coding
Article.printPublisher(); // Dream Coding 
```
<br> 


## Inheritance
공통 부분 재사용을 통한 유지보수가 쉬움
- 다양성
  - 필요한 함수의 재정의 
  - 재정의한 경우, 재정의된 함수로 호출 
  - super 키워드 사용시, 부모의 함수도 호출 가능 (Triagle class's getArea())  
```javascript
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }

  draw() {
    console.log(`drawing ${this.color} color!`);
  }

  getArea() {
    return this.width * this.height;
  }
}

class Rectangle extends Shape {}
class Triangle extends Shape {
  draw() {
    super.draw();
    console.log("🔺");
  }
  getArea() {
    return (this.width * this.height) / 2;
  }

  toString() {
    return `Triangle: color: ${this.color}`;
  }
}

const rectangle = new Rectangle(20, 20, "blue");
rectangle.draw(); // drawing blue color!
console.log(rectangle.getArea()); // 400
const triangle = new Triangle(20, 20, "red"); 
triangle.draw(); // drawing red color! // 🔺
console.log(triangle.getArea()); // 200
```

<br> 


## Class checking: instanceOf
`[instance] of [className]`   
class를 이용해서 만들어진 새로운 인스턴스로 클래스의 인스탠스 여부를 확인 가능    
`true` or `false` return    

```javascript
console.log(rectangle instanceof Rectangle); // t
console.log(triangle instanceof Rectangle); // f
console.log(triangle instanceof Triangle); // t
console.log(triangle instanceof Shape); // t
console.log(triangle instanceof Object); // t
console.log(triangle.toString()); // Triagle: color: red

let obj = { value: 5 };
function change(value) {
  value.value = 7;
}
change(obj);
console.log(obj); // {value: 7}
```
> JS의 모든 object는 JS의 Object를 상속한 것!!!  
> 즉, 모든 object에서 Object에 존재하는 method 사용이 가능함 (ex. toString())  
> 
 

### vs. `hasOwnProperty()`   
객체에 인수로 지정한 **프로퍼티가 존재**하는지 여부 확인 

```javascript
obj.hasOwnProperty(prop)
```

