---
layout: post
title: "📚 디자인 패턴"
tags: [pattern, SW, design pattern, GoF, OOP]
excerpt_separator: <!--more-->
sitemap :
  changefreq : daily
  priority : 1.0


---

### ⛓ 패턴이란?

- 패턴의 사전적 의미는 '일정한 형태나 양식 또는 유형이 반복되는 것'

- 패턴은 발명하는 것이 아니라 **발견**하는 것이다. 

  <!--more-->

- 공학분야에서 처음으로 패턴의 개념을 도입한 분야는 건축공학

- 개발 분야에서의 패턴은 <u>많은 사람이 겪은 문제점과 해결방법을 정리해둔 것</u>을 의미

- 즉, 패턴은 <u>문제를 해결하는 과정을 일반화한 것</u>이라고 볼 수 있다

- 디자인 패턴을 구별하는 기준은 <u>각각의 패턴이 어떤 관심사를 가지고 문제를 해결하려 하는가</u> 에 대한 것이다. 패턴은 의도와 목적에 따라 달라지므로 <u>적절한 패턴을 적용하기 위해서는 의도와 목적을 잘 파악하는 것이 중요</u>하다.

### 🖥 소프트웨어 공학

이러한 패턴의 원리는 소프트웨어 공학 분야에 도입되기 시작했다. 소프트웨어적인 문제를 코드로 구현할 때 적용했던 해결 방법을 패턴화하여 정리한 것이다. 

처음으로 소프트웨어 공학에 도입된 부분은 객체지향 개발로, 소프트웨어 개발 방법론이다. 객체지향은 큰 프로젝트 개발과 유지보수를 보다 쉽게 하기 위해 도입된 개발 방법론이다. 최근 프로그램들이 대형화, 분업화되면서 절차지향 개발 방법론에서 객체지향 개발 방법론으로 빠르게 이동하고 있다. 디자인 패턴은 객체지향 개발법이 인기를 얻으면서 함께 주목을 받았다. 

많은 프로그래머가 소프트웨어 개발 분야에서 특정 문제를 유사한 유형으로 해결한다는 것을 알게 되었고, 해결과정에서 발견된 방법들을 패턴화하여 규정하기 시작했다. 디자인 패턴은 객체지향 구현 문제를 해결하기 위해 도입되었으며 해결방법을 재사용하는 것이라고 볼 수 있다.

- 객제치향 개발에서 디자인 패턴이 해결하는 주요 문제는 객체 간 관례와 소통처리 부분이다.
- 디자인 패턴은 객체지향 문제를 해결하기 위한 일련의 코드 스타일로, 디자인 패턴을 적용하기 위해서는 객체 지향적인 코드를 작성할 수 있는 프로그래밍 언어가 필요하다.

### ✏️ 디자인 패턴 설계 원칙

디자인 패턴 외에도 객체지향 코드 개발시 가급적 지켜야할 원칙이 있다. 원칙들의 앞글자를 따서 SOLID 라고 부르기도 한다.

1. 단일 책임의 원칙(Single Responsibility Principle, SRP)
2. 개발 폐쇄 원칙(Open & Closed Principle, OCP)
3. 리스코프 치환원칙(Liskov Substitution Principel, LSP)
4. 인터페이스 분리의 원칙(Interface Segregation Principle, ISP)
5. 의존관계 역전의 법칙(Dependency Inversion Principle, DIP)

이 원칙들은 객체지향 코드를 작성하는 데 도움을 주지만, 시스템을 고려하지 않은 원칙을 적용할 경우 불필요한 일이 발생할 수도 있다. 따라서 각각의 원칙들은 코드의 목적에 맞게 적재적소에 적용하여 사용해야 한다.

### GoF

Gang of Four의 약자로 에릭 감마, 리처드 헬름, 랄프 존슨, 존 블리시데스의 저서인 "GoF의 디자인 패턴" 을 가리키는 용어이다. 즉, <u>처음으로 소프트웨어 공학에서 사용되는 패턴을 정리한 사람들의 별칭</u>이다.

GoF는 객체지향 분야의 문제점을 분석해 **24개의 패턴**으로 분류했다. 기존의 객제치향 설계 시 발생했던 문제들을 카탈로그화하여 패턴으로 정리한 것이다. <u>카탈로그화 된 패턴은 시스템의 유지 보수 문서를 작성할 때 매우 유용하다. 또한 객체 간 상호작용이나 설계 의도 등도 쉽게 확인</u>할 수 있다. 

### ⭐️ <u>패턴의 요소</u>

24개로 분리된 디자인 패턴은 공통된 4가지 요소를 가진다

- **이름(patrtern name)** → 24종류의 디자인 패턴은 각각 고유의 이름을 가지고 있다. 패턴의 이름을 통해 패턴의 용도를 직관적으로 이해할 수 있다.
- **문제(problem)** → 각 패턴이 해결하고자 하는 문제이다. 문제들은 패턴 적용을 고려해야 하는 시점을 암시한다.  코드에서 해결할 문제점을 발견한 후 그와 관련된 배경을 정리하고 다양한 적용 사례를 알아볼 수 있다.
- **해법(solution)** → 문제점을 인식했다면 해결 방법을 찾아야 한다. 이를 위해 객체 요소 간 관계를 정리하고 패턴을 통해 객체들을 추상화하는 과정을 거친다. 또한 해결을 위한 객체를 나열한다.
- **결과(consequence)** → 디자인 패턴은 알려진 문제점들을 해결하는데 효과적이다. 하지만 디자인 패턴을 적용한다고 해서 모든 문제를 완벽하게 제거할 수는 없다. 패턴은 매우 유용하지만 꼭 필요한 경우를 생각해서 적절히 분배하여 사용해야 한다.

### 정리

- 디자인 패턴은 (소프트웨어 공학에서) <u>객체지향 개발 방법론에서 자주 등장하는 문제들을 해결하기 위해 도입된 재사용 가능한 솔루션</u>이다.
- 디자인 패턴에 익숙해진다면 과거의 코드나 새로운 코드를 학습하는 시간을 줄일 수 있다. 따라서 패턴은 문제 해결 방법 외에도 개발 비용과 시간을 절약하는데 매우 유용하다.
- 디자인 패턴과 처리 성능은 별개의 문제이다.
    - 디자인 패턴에서는 코드의 가독성과 유지보수를 위해 객체의 메서드를 분리하며 호출도 자주 발생한다.
    - 패턴을 지나치게 사용하면 잦은 메서드 호출로 인해 성능이 저하될 수 있다.