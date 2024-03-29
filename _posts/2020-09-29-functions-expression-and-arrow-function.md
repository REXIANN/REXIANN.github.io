---
layout: post
title: "5. 함수의 선언과 표현, Arrow function"
tags: [javascript, function, declaration, expressional, anonymous, arrow]
excerpt_separator: <!--more-->
---

절차지형 언어의 경우 함수나 변수가 선언되는 위치가 굉장히 중요하다. 자바스크립트 또한 절차지향언어의 하나이다. 따라서 sub-programing이 중요하다. <!--more-->

자바스크립트는 class가 추가되어서 OOP라고 할 수 있지만 순수한 OOP는 아니고 prototype를 활용한 가짜 OOP이다.





## 함수

* 프로그램을 구성하는 기본적인 building block
* sub-program이라고도 불리며 재사용이 가능함
* 한가지 작업이나 값을 계산하기 위해서 사용된다
* 하나의 함수는 반드시 <u>한개</u> 의 작업만 실행해야 한다!!! 
* 자바스크립트에서 함수는 오브젝트 이다(나중에 일급함수로 사용되는걸 같이 보자)

### 함수 선언식

`function name(param1, param2) { body ... return}`

함수이름은 현재형 동사를 사용하는 것이 권장된다 `(ex: doSomething, checkSomething)`

```javascript
function printHello() {
  console.log('Hello World!')
}
printHello() // 'Hello World!'

function printMessage(message) {
  console.log(message)
}
printMessage('Hi there!') // 'Hi there!'
// 타입스크립트의 경우 함수선언식의 파라미터뒤에 타입을 붙인다
function name(param: String): number {
  return value // must be number type
}

// 기본인자(ES6에 추가)
function showMessage(message, from='kim') {
  console.log(`${message}, by ${kim}`)
}
showMessage('hello') // 'hello, by kim'

// 나머지 인자(ES6에 추가)
function printAll(...args) {
  // 1.
  for (let i = 0; i < args.length; i++) {
    console.log(args[i])
  }
  // 2.
  for (const arg of args) {
    console.log(arg)
  }
  // 3.
  args.forEach((arg) => console.log(arg))
}
printAll('dream', 'comes', 'true') // dream comes true
```



### 스코프

Closure, lexical environment를 위해선 꼭 알아야 함.

알아야 할 하나의 원칙: <u>**밖에서는 안이 보이지 않고 안에서만 밖을 볼 수 있다**</u>

```javascript
let globalMessage = 'global' // global variable
function printMessage() {
  let message = 'hello' // local variable
  function printAnother() {
    const childMessage = 'child'
    console.log(message) // 'hello'
  }
	console.log(childMessage) // Error!
}
```



### Early return, early exit

```javascript
// good
function upgradeUser(user) {
  if (user.point <= 10) {
    return;
  }
  // upgrade logics...
}

// bad
function upgradeUser(user) {
  if (user.point > 10) {
    // upgrade logics...
  }
}
```



### 함수표현식

`const print = function () {}`

* 변수에 함수를 할당하는 형태로 쓴다.
* 함수의 이름이 없이 필요한 부분만 작성하므로 anonymous function이라고도 한다.
* 만일 이름을 붙인다면 named function이라고 부른다.

```javascript
const print = function () { console.log('print') }
print() // 'print'
const printAgain = print
printAgain() // 'print'
```

함수표현식의 경우 호이스팅의 적용을 받지 않는다. 따라서 선언되는 위치가 중요하다!



## 콜백함수

```javascript
function randomQuiz(answer, printYes, printNo) {
  if (answer === 'correct') {
    printYes()
  } else {
    printNo()
  }
}

const printYes = function() { console.log('Yes!') }
const printNo = function print() { console.log('No!') }
// named function은 디버깅 시 사용된 함수의 이름이 보인다는 장점이 있다
// 또한 재귀호출이 가능하게 하는 장점이 있다

randomQuiz('wrong', printYes, printNo) // 'Yes!'
randomQuiz('correct', printYes, printNo) // 'No!'
```



## 화살표함수

 `const arrowFunction = () => {}`

* 화살표 함수는 익명함수이다.
* ES6에 추가된 기능이다.
* 다른 함수와 다르게 화살표 함수는 `this` 를 가지지 않는다. (global한 this를 쓰지 않고 블록 스코프를 따른다)

기본구문

```javascript
const arrowFunction = (param1, param2) => { statements } // statement를 실행할 때
const arrowFunction = (param1, param2) => { return expression } // 값을 반환할 때
const arrowFunction = (param1, param2) => expression  // 바로 위의 함수와 같은 기능을 한다

// 매개변수가 하나뿐인 경우 괄호의 생략이 가능하다
const arrowFunction = (param1) => { statements }
const arrowFunction = param1 => { statements }

// 매개변수가 없더라고 괄호가 필요하다
const arrowFunction = () => { statements }

// 객체 리터럴을 반환하는 경우 함수 본문(body)을 괄호 속에 넣어야 한다.
const arrowFunction = () => ({ key: 'value' }) // O
// 괄호가 없으면 {} 안에 있는 내용은 해석할 '문장'으로 취급한다. 주의!
const arrowFunction = () => { key: 'value' } // X

// 기본매개변수와 나머지매개변수를 지원한다
const arrowFunction = (param1, ...arr) => { statement }
const arrowFunction = (param1, param2 = 'defaultValue') => { statement }

```



다음과 같이 활용할 수 있다.

```javascript
const simplePrint = function () { console.log('simple print') }
const simplePrint = () => console.log('simple print')
// 위의 두 표현식은 동일한 기능을 한다.

const add = (a, b) => a + b;
const multiply = (a, b) => {
  return a * b;
}
```



### IIFE: Immediately Invoked Function Expression

```javascript
(function hello() {
  console.log('hello IIFE')
})()
// 함수 자체를 괄호로 묶고 함수 뒤에 괄호를 넣어주면 함수가 바로 실행된다
// 요즘은 많이 쓰이지 않는 추세이나 함수를 작성하고 바로 실행결과를 보고 싶을 때 유용하다!
```

