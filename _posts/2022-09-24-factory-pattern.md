---
layout: post
title: "자바스크립트 팩토리 패턴(Factory Pattern in JS)"
tags: [factory, design pattern, js]
excerpt_separator: <!--more-->
sitemap:
changefreq: daily
priority: 1.0
---
팩토리 패턴은 크게 두 가지로 설명할 수 있다.

1. 객체를 사용하는 코드에서 객체의 생성을 떼어내서 별도의 객체에 추상화 한다.
2. 상속관계의 경우, 상위 객체가 중요한 뼈대를 담당하고, 하위 객체에서 객체 생성에 관한 구체적인 내용을 결정한다.

<!--more-->

상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며, 상위 클래스에서는 인스턴스의 구현에 대해 신경쓰지 않기 때문에
더 많은 유연성을 가지게 된다.

또한 객체 생성 로직이 분리되어 있기 때문에 코드를 리팩터링 하더라도 한 곳만 고치게 된다. 따라서 유지보수성이 증가한다.

## 자바스크립트에서 사용되는 팩토리 패턴
```ts
const num = new Object(42);
const str = new Object("str");

num.constructor.name // Number
str.constructor.name // String
```

전달 받은 값에 따라 다른 객체를 생성하는 것을 볼 수 있다.
즉, 전달받은 값에 따라 다른 객체를 생성하며, 인스턴스의 타입을 정한다.

커피팩토리를 기반으로 라떼와 에스프레소를 만드는 팩토리를 구현해보면 다음과 같다.

```js
class Latte {
  constructor() {
    return this.name = "Latte";
  }
}

class Espresso {
  contructor() {
    return this.name = "Espresso";
  }
}

class LatteFactory {
  static makeCoffee() {
    return new Latte();
  }
}

class EspressoFactory {
  static makeCoffee() {
    return new Espresso();
  }
}

const factoryList = { LatteFactory, EspressoFactory };

class CoffeeFactory {
  static makeCoffee(type) {
    const factory = factoryList[type];
    
    return factory.makeCoffee();
  }
}

const main = () => {
  const coffee = CoffeeFactory.makeCoffee("LatteFactory");
  
  console.log(coffee.name); // Latte
}

main();
```

CoffeeFactory 라는 상위 클래스가 중요한 뼈대(여기서는 Coffee를 만들기 위한 기능)를 결정한다.
그리고 하위 클래스인 LatteFactory와 EspressoFactory 는 해당 메서드를 구체화 하는 역할을 하고 있다.

이는 의존성 주입으로도 볼 수 있다. 왜냐하면 CoffeeFactory 에서 LatteFactory의 인스턴스를 생성하지 않고
LatteFactory 에서 생성한 인스턴스를 CoffeeFactory 에 주입하고 있기 때문이다.

실질적으로 라떼를 만드는 과정을 수정해야 할 경우 CoffeeFactory 는 전혀 신경 쓸 필요 없고 Latte 클래스에서 구현부를 수정하면 된다.
왜냐하면 실질적인 구현체는 LatteFactory 에서 생성한 인스턴스이고, 해당 인스턴스는 CoffeeFactory 에 주입되고 있기 때문이다.

⚠️ 여기서 정적 메서드를 정의한 것을 볼 수 있는데, 정적 메서드를 사용하면 두 가지 장점이 있다. 첫번째로 정적 메서드는 객체를 생성하지 않고
클래스의 메서드를 사용할 수 있기 때문에 메모리를 절약할 수 있다. 두번째로 개별적인 인스턴스에 묶이지 않으면서 클래스 내부의 함수를 정의할 수 있다는 장점이 있다. 
