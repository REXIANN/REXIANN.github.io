---
layout: post
title: "코딩의 기본 연산자, if for loop + 코드리뷰 팁"
tags: [javascript, operator, concatenation, comparison]
excerpt_separator: <!--more-->





---

## 지난시간 복습

원시타입의 경우 값 자체가 메모리에 저장된다. 

반면 오브젝트 타입의 경우 메모리에 한번에 올릴 수가 없고 레퍼런스가 있고 레퍼런스가 실제 값이 있는 주소의 값을 가지고 있다(가리키고 있다). <!--more-->

원시타입의 경우 `object.freeze()` 를 통해서 포인터를 잠글 수 있다(Immutable하게 만들 수 있다).

자바스크립트에서 기본적으로 모든 오브젝트 타입은 mutable이다. 



### operator

1. String concatenation

```javascript
console.log('my' + 'cat')
console.log('1' + 2)
console.log(`string literals: 1 + 2 = ${1 + 2}`)
```

2. Numeric Operators

```javascript
console.log(1 + 1) // add
console.log(1 - 1) //substract
console.log(1 * 1) // multiply
console.log(1 / 1) // divide
console.log(5 % 2)  // remainder
console.log(2 ** 3) //exponentiation
```

3. Increment and decrement operators

```javascript
let counter = 2
const preIncrement = ++counter // 3
conosle.log(`${preIncrement} ${counter}`) // 3, 3

let counter = 2
const postIncrement = counter++
console.log(`${postIncrement} ${counter}`) // 3, 2
```

4. Assignment operators

```javascript
let x= 3
let y = 6
x += y // 9
x -= y // -3
```

5. Comparison operators

```javascript
console.log(10 < 6) // less than, lt
console.log(10 <= 6) // less than or equal, lte
console.log(10 > 6) // greater than, gt
console.log(10 >= 6) // greater than or equal, gte
```

6. Logical operators

```javascript
const value1 = true;
const value2 = 4 < 2; // false
// ||(or), &&(and), !(not)
console.log(value1 || value2) // true
console.log(value1 && value2) // false
console.log(!value1) // true

// && 연산자의 경우 null-check가 가능하다! null은 false로 형변환이 되기 때문에 실행조건을 맞출 떄 쓰기도 한다.
// && 연산자의 경우 모든 조건을 따지게 되므로 순서를 고려하여 작성하자!
```

7. Equality

```javascript
const stringFive = '5'
const numberFive = 5

// == loose equality, with type conversion
// 묵시적 형변환을 했을 때 같다면 같은 것으로 취급
console.log(stringFive == numberFive) // true

// === strict equality, no type conversion
// 묵시적 형변환을 허용하지 않음. 정확한 형변환을 위해 이걸 쓰는것을 권장
console.log(stringFive === numberFive) // false

//equality - puzzler
console.log(0 == false) // true
console.log(0 === false) // false
console.log('' == false) // true
console.log('' === false) // false
console.log(null == undefined) // true
console.log(null === undefined) // false

// 묵시적 형변환을 사용하면 3단논법이 먹히지 않는 이상한 자바스크립트의 유연성(?)을 볼 수 있다
const a = '0'
const b = 0
const c = []
 
conosole.log(a == b) // true
console.log(b == c) // true
console.log(a == c) // false
```

8. Conditional operators: if

```javascript
const name = 'kim'
if (name === 'kim') {
  console.log(`Welcome, ${name}`)
} else if (name === 'Sam') {
  console.log(`Welcome, ${name}`)
} else {
  console.log(`Who is this?`)
}
```

9. Ternary operator: ?

```javascript
// condition ? true-value : false-value;
const condition = 'Hello'
console.log(condition === 'Hello'? 'World': 'Everybody')
```

10. Switch statement

```javascript
const browser = 'IE'
switch( browser) {
  case 'IE':
    console.log('Unsupported browser')
    break;
  case 'Chrome':
    console.log('Welcome!')
    break;
  case 'Firefox':
    console.log('Welcome!')
    break;
  default:
    console.log('Hi there')
    break;
}
// 추후 타입스크립트에서 타입검사를 위해 스위치를 쓰는것이 좋다
```

11. Loops

```javascript
let i = 3
while (i > 0) {
  console.log(`while: ${i}`)
  i--;
}

let i = 3
do {
  console.log(`do while ${i}`)
  i--;
} while (i > 0);
```

12. For loop

```javascript
// for (시작조건, 종료조건, 반복조건)
for (let i = 0; i < 5; i++) {
  console.log(`for: ${i}`)
}

for (let i = 5; i > 0; i -= 2) {
  console.log(`inline variable for: ${i}`)
}

// nested loops
for (let i = 0; i < 10; i++) {
  for (let j = 0; j < 10; j++) {
    console.log(`${i}, ${j}`)
  }
}
```

