---
layout: post
title: "11.비동기 처리의 시작, 콜백 이해하기."
tags: [javascript, Asynchronous, Synchronous, CallBack, CallBack Hell]
excerpt_separator: <!--more-->
sitemap :
  changefreq : daily
  priority : 1.0
---

## Synchronous VS Asynchronous

자바스크립트는 synchronous 하다. 그 말은 호이스팅이 된 이후부터 코드는 우리가 작성한 순서에 따라 동기적으로 하나 하나 실행된다는 의미이다. <!--more--> 정해진 순서에 맞게 코드가 실행되는 것이라고 이해해도 좋다. 

(호이스팅이란? 변수 var와 함수선언식이 자동적으로 제일 위로 끌어올려져서(hoisting) 선언되는 것!) 



```javascript
console.log(1);
console.log(2);
console.log(3);
// 순서대로 1, 2, 3이 실행된다.
```



Asynchronous란 언제 코드가 실행될 지 예측할 수 없는 것을 의미한다. 대표적인 예로는 브라우저에서 제공하는 API인 setTimeout이 있다. 
```javascript
console.log(1);
setTimeout(function() { console.log('2')}, 1000) // 1000은 밀리초이다.
console.log(3);
// 1, 3, 2 가 실행된다.
```

여기서 setTimeout은 1초의 카운트를 진행함과 동시에 시간이 만료되었을 때 실행할 함수는 큐에 넣어버리고 바로 다음줄로 내려간다. 브라우저는 1초가 지나면 그때 해당 함수를 실행한다. 시간이 지나면 실행할 함수를 다시 불러줘! 라는 의미로 콜백(Call Back)함수라고 한다! 콜백함수는 보통 화살표 함수로 작성하며 콜백함수는 다시 Synchronous callback과 Asynchronous callback 두개로 나뉜다.



## Callback function

### Synchronous callback function

```javascript
console.log(1);
setTimeout(function() { console.log('2')}, 1000) 
console.log(3);

function printImmediately(print) {
  print()
}
printImmediately(() => console.log('hello'))
// 1, 3, hello, 2의 순서로 실행된다.
```

### Asynchronous callback function

```javascript
console.log(1);
setTimeout(function() { console.log('2')}, 1000) 
console.log(3);

function printWithDelay(print, timeout) {
  setTimeout(print, timeout)
}
printWithDelay(() => console.log('asynch'), 2000);
// 1, 3, 2, asynch 의 순서로 실행된다.
```

자바스크립트는 함수를 인자의 형태로 다른 함수에 전달할 수 있고 변수에 할당할 수도 있다. 이를 통해 콜백함수 기능을 지원한다. 언어마다 콜백함수를 지원하는 방법은 각자 다르며 다른 예로는 서브루틴, 람다표현식, 함수 포인터 등이 있으니 시간이 나면 한번 찾아보자. 

## 콜백지옥

콜백함수들을 계속 묶어나가면서(nesting) 콜백함수 안에서 또 콜백함수를 부르고 또 그안에서 다른 콜백함수를 부르고.. 하는 형태를 이루면 콜백이 연속적으로 발생한다는 의미에서 콜백지옥이라고 한다. 예제로 실습과 함께 진행하자

```javascript
class UserStorage {
  loginUser(id, password, onSuccess, onError) {
    // 아이디, 비번, 로그인성공 & 실패에 따른 메시지 출력하는 콜백함수
    setTimeout(() => {
      if (
        (id === 'kim' && password === 'jin') ||
        (id === 'lee' && password === 'yong')
      ) {
        onSuccess(id)
      } else {
        onError(new Error('user not found'))
      }
    }, 2000)
  }

  getRoles(user, onSuccess, onError) {
    // 사용자의 역할을 서버에 요청하여 받아오는 함수
    // 원래는 사용자의 역할을 한번에 받아오는 것이 정석 
     setTimeout( () => {
       if (user === 'kim') {
         onSuccess({ name: 'kim', role: 'admin' })
       } else {
         onError(new Error('no access'))
       }
     }, 1000)
  }
}
```



해당 코드를 작성한 뒤

1. 아이디와 비밀번호를 입력하여 로그인
2. 로그인 성공 시 사용자의 아이디를 받아옴
3. 받아온 사용자의 아이디를 가지고 사용자의 역할을 받아옴
4. 받아온 역할을 확인하여 사용자의 이름과 역할이 담긴 오브젝트 확인하기

```javascript
const userStorage = new UserStorage()
const id = prompt('enter your id')
const password = prompt('enter your password')
userStorage.loginUser(id, password, (user) => {
  userStorage.getRoles(
    user, userWithRole => {
      alert(`hello ${userWithRole.name}, role is ${userWithRole.role}`)
    }
  )
}, (error) => {
  console.log(error)
})
```

### 문제점

콜백함수 안에서 다른 함수를 호출하고 그 안에서 또 다른 콜백을 호출하고... 이렇게 전달->전달->전달의 연속의 문제점은 1. 가독성이 너무 떨어지며 2.로직을 이해하기 어려우며 3.디버깅이나 에러발생시 어떤 부분에서 발생했는지 분석이 힘들고 4.유지 보수가 까다로워진다. 

이를 promise와 asynch, await을 통해 어떻게 고치는지는 다음편에서 알아보자

