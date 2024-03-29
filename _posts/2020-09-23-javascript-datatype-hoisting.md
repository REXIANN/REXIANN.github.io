---
layout: post
title: "JavaScript - 데이터타입, const? let? var? , 호이스팅"
tags: [javascript, script, data type, const, let, var, hoisting, dynamic typing]
excerpt_separator: <!--more-->




---



## Variable(변수)

JS에서 변수를 만들기 위해서는 `let` 이라는 예약어를 사용한다. <!--more-->

```javascript
let message = 'hello world!'
console.log(message)   // 'hello world!'
```





 ### block scope

{}을 사용하여 코드를 작성하면 블록 밖에서는 안에 있는 변수를 볼 수 없다

```javascript
{
  let message = 'hello world!'
	console.log(message)  // 'hello world!'
}
console.log(message) // Error
```

그냥 밖에다가 쓰면 global scope라고 하며 global한 변수들은 어플리케이션이 시작될때부터 끝날 때 까지 존재하며, 블록의 안이나 글로벌에서 가져다가 쓸 수 있다. 

`let` 이라는 예약어는 ES6에서 추가되었다. 그 이전에는 `var` 라는 예약어를 사용했었으며 지금 이 예약어를 사용하는 것은 권장되지 않는다. 왜냐하면 var를 사용할경우 블록스코프를 무시하며(블록 스코프 바깥에서 블록스코프 안에 있는 var 예약어로 선언된 변수에 접근이 가능해진다) 또한 변수를 선언하기도 전에 할당하는 것이 가능하기 때문이다. 이는 호이스팅이라고 한다.

### Hoisting

호이스팅이란 영어로는 '끌어올리다' 라는 뜻이다. 자바스크립트에서 호이스팅이 일어나면 실행순서에 상관없이 맨 위로 끌어올려서(hosting) 선언을 먼저 해주고 실행한다. 호이스팅되는 변수는 대표적으로 var와 선언형으로 정의된 함수가 있다.  



## Constant

변수와 반대되는 개념으로 상수 라고도 한다. 한번 선언하게 되면 해당 값을 가리키는 포인터가 잠기기 때문에 값을 변경할 수 없다. Immutable Data type이라고도 한다. 그리고 가급적이면 immutable하게 데이터를 선언하는 것을 권장한다. 왜냐하면 한번 선언이 되면 값이 변경되지 않기 때문에 해커들이 접근하여 값을 변경하는 것이 불가능 하고(safety), 값을 읽어오고 사용하는데 집중할 수 있기 때문에 실행이 빠르기(thread safety) 때문이다. 



## 데이터 타입

### Primitive vs Object

#### primitive(원시 타입)

 `Number`, `string`, `boolean`, `null`, `undefined`, `symbol` 등이 있다.

```javascript
let a = 1
let b = 'abc'
let c = true //or false
let d = null
let e = undefined
let f = Symbol()
```

#### Object(오브젝트 타입)

위의 primitive 타입들을 묶어서 하나의 오브젝트로 만들어서 관리할 수 있는 타입. 자바스크립트에서는 function도 오브젝트 타입이다.

```javascript
let a = {
  aa: 1,
  bb: 'world',
  cc: false
}
```



### Number

자바스크립트에서 숫자를 위해 쓰이는 타입은 단 한개! Number만으로 양수, 음수, 정수 등의 표현이 가능하다

```javascript
const a = 1
const b = -1
const c = 1.56
```

그 이외에도 양의 무한대인  `Infinity`, `-Infinity`, `NaN` 이라는 값들이 있다.

```javascript
const infinity = 1 / 0 // Infinity
const negativeInfinity = -1 / 0 //-Infinity
const notANumber = 'asdf' / 2 //NaN (Not a Number)
```

숫자타입은 Number 한개로 `-2**53 ~ 2**53` 범위에 있는 숫자만 표현이 가능하지만 숫자 끝에 n을 붙이면 bigInt타입으로 사용할 수 있다. bigInt타입은 Chrome과 Firefox에서만 사용하능하며 다른 브라우저(ex: Safati, IE)에서는 사용이 불가능하다

```javascript
const bigInt = 999999999999999999999999999999999999999999999n
```



### String

자바스크립트는 한 개의 글자이든, 여러개의 글자이든 `' '` 또는 `" "`로 표현이 가능하다. 문자열끼리는 더하기 기호를 사용해서 더하는 것이 가능하다

```javascript
const a = 'hello'
const b = 'world'
const hi = a + b
console.log(hi) // hello world
```

또한 template literals라는 문법을 사용해서 문자열을 ` (백틱)을 사용해서 선언하고 문자열 안에 ${}기호를 삽입하여 변수를 할당하는 것 또한 가능하다.

```javascript
const a = 'hello'
const b = 'world'
console.log(`${a}! my name is ${b}`) // hello! my name is world
const c = 'letter'
console.log(`value: ${c}, type: ${ typeof c}`) // value: letter, type: string
```



### 기타

#### null

아무것도 없음 을 의미한다.

```javascript
let a = null
console.log(`value: ${a}, type: ${typeof a}`) // value: null, type: object
```

#### undefined

null과는 다르게 값이 아직 정해지지 않았음을 의미한다.

```javascript
let a
console.log(`value: ${a}, type: ${typeof a}`) // value: undefined, type: undefined
```



#### symbol

심볼은 고유한 식별자를 만들기 위해 사용한다. 

```javascript
const s1 = Symbol('asdf')
const s2 = Symbol('asdf')
console.log(s1 === s2) // false
```

두 가지의 심볼은 같아 보이지만 다르다. 각 스트링은 고유한 심볼로 적용된다는 뜻이다. 

만일 값이 같으면 같은 심볼로 나타나게 하고 싶다면 `.for`를 사용할 수 있다.
```javascript
const s1 = Symbol.for('asdf')
const s2 = Symbol.for('asdf')
console.log(s1 === s2) // true
```



## Dynamic typing(동적 타입)

자바스크립트는 동적타이핑을 지원한다. 다시 말해 변수가 선언될 때 타입을 지정하지 않고 나중에 필요에 맞게 타입을 지정해 줄 수 있다는 의미이다.  반대되는 개념으로는 static typing이 있으며 변수가 선언될 때 타입을 지정해주어야 함을 의미한다.

다만 큰 프로젝트에서는 Dynamic typing 때문에 종종 문제가 되기도 한다.

```javascript
let text = 'hello'
console.log(`value: ${text}, type: ${typeof text}`) // value: hello, type: string
text = 1
console.log(`value: ${text}, type: ${typeof text}`) // value: 1, type: number
text = '5' + 2
console.log(`value: ${text}, type: ${typeof text}`) // value: 52, type: string
text = '4' / '2'
console.log(`value: ${text}, type: ${typeof text}`) // value: 2, type: number
```

이러한 문제를 보완하고자 나온 것이 바로 TypeScript이다. typeScript는 JS의 superset으로 JS에 타입이 추가된 형태이다. 그리고 타입스크립트는 브라우저가 이해할 수 있는 JS로 컴파일을 해야 한다(Babel). 



## first class function(일급 함수)

일급함수란 다음 세가지를 만족하는 것을 의미한다

1. 변수에 함수를 할당할 수 있다
2. 함수를 인자로 전달할 수 있다
3. 함수를 반환값으로 사용할 수 있다








