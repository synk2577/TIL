# 배열

## Declaration

```jsx
const arr1 = new Array();
const arr2 = [1, 2];
```

## Index position

```jsx
const fruits = ['apple', 'banana'];
console.log(fruits); // ['apple', 'banana']
console.log(fruits.length); // 2
console.log(fruits[0]); // apple
console.log(fruits[1]); // banana
console.log(fruits[2]); // undefined
console.log(fruits[fruits.length - 1]); // last index
```

## Looping over an array

```jsx
// a. for
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}

// b. for of
for (let fruit of fruits) {
  console.log(fruit);
}

// c. forEach
fruits.forEach((fruit, index, array) => {
  console.log(fruit, index, array);
});
// apple 0 [ 'apple', 'banana' ]
// banana 1 [ 'apple', 'banana' ]

fruits.forEach((fruit, index) => {
  console.log(fruit, index);
});
// apple 0
// banana 1

fruits.forEach((fruit) => {
  console.log(fruit);
});
// apple
// banana
```

## Additon, deletion, copy

### push & pop

```jsx
// push: add an item to the end
fruits.push('strawberry', 'peach');
console.log(fruits); // [ 'apple', 'banana', 'strawberry', 'peach' ]

// pop: remove an item fromthe end
fruits.pop();
console.log(fruits); //[ 'apple', 'banana', 'strawberry' ]
```

### unshift & shift

```jsx
// unshift: add an item to beginning
fruits.unshift('strawberry', 'lemon');
console.log(fruits); // [ 'strawberry', 'lemon', 'apple', 'banana', 'strawberry' ]

// shift: remove an item from the beginning
fruits.shift();
fruits.shift();
console.log(fruits); // [ 'apple', 'banana', 'strawberry' ]
```

> unshift, shift 는 push, pop보다 속도가 느리다!!    
왜냐하면, 기존 데이터를 한 칸씩 뒤로 미루거나 당기는 작업이 필요하기 때문!!    
이는 배열의 길이가 길수로 더 느려짐
> 

### splice

```jsx
// splice: remove an item by index position
fruits.push('strawberry', 'peach', 'lemon');
console.log(fruits); // [ 'apple', 'banana', 'strawberry', 'strawberry', 'peach', 'lemon' ]
fruits.splice(1, 1); // index 1인 요소부터 1개만 지우기 = banana
console.log(fruits); // [ 'apple', 'strawberry', 'strawberry', 'peach', 'lemon' ]
fruits.splice(1, 1, 'apple', 'watermelon'); // index 1인 요소 1개를 지우고 그 자리에 나머지 item 추가
fruits.splice(1, 0, 'apple', 'watermelon'); // index 1위치에 (데이터를 지우지 않고) 나머지 item 추가
console.log(fruits); // [ 'apple', 'apple', 'watermelon', 'strawberry', 'peach', 'lemon' ]

```

### combine

```jsx
// combine two arrays
const fruits2 = ['pear', 'coconut'];
const newFruits = fruits.concat(fruits2);
console.log(newFruits); // [ 'apple', 'apple', 'watermelon', 'strawberry', 'peach', 'lemon', 'pear', 'coconut' ]
```

## Searching

```jsx
// indexOf: find the index
console.clear();
console.log(fruits); // [ 'apple', 'apple', 'watermelon', 'strawberry', 'peach', 'lemon' ]
console.group(fruits.indexOf('apple')); // 0
console.group(fruits.indexOf('watermelon')); // 2
console.group(fruits.indexOf('coconut')); // -1 : 해당하는 값이 없는 경우

// includes
console.group(fruits.includes('watermelon')); // true
console.group(fruits.includes('coconut')); // false

// lastIndexOf
console.clear();
fruits.push('apple');
console.log(fruits); // [ 'apple', 'apple', 'watermelon', 'strawberry', 'peach', 'lemon', 'apple' ]
console.log(fruits.indexOf('apple')); // 0
console.log(fruits.lastIndexOf('apple')); // 6
```

