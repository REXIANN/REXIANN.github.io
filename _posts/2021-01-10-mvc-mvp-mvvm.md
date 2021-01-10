---
layout: post
title: "디자인패턴-MVC, MVP, MVVM 비교"
tags: [pattern, ]
excerpt_separator: <!--more-->
sitemap :
  changefreq : daily
  priority : 1.0



---

Created: Dec 9, 2020 10:35 PM
Created By: Jinhyuck Kim
Last Edited Time: Dec 17, 2020 11:40 AM
Type: 소프트웨어공학

# 1. MVC

mvc패턴은 Model + View + Controller를 합친 용어이다.

## 1) 구조

- Model: 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
- View: 사용자에게 보여지는 UI 부분
- Controller: 사용자의 입력을 받고 처리하는 부분

## 2) 동작

1. 사용자의 action들이 controller에 들어온다
2. controller는 사용자의 action을 확인하고 model을 업데이트한다
3. controller는 model을 나타내 줄 view를 선택한다
4. view는 model을 이용하여 화면을 나타낸다

### mvc에서 view가 업데이트 되는 방법

1. View가 Model을 이용하여 직접 업데이트 하는 방법
2. Model에서 View에게 Notify하여 업데이트 하는 방법
3. View가 Polling으로 주기적으로  Model의 변경을 감지하여 업데이트 하는 방법

## 3) 특징

- Controller는 여러개의 view를 선택할 수 있는 1:N의 구조이다
- Controller는 View를 선택할 뿐 직접 업데이트 하지 않는다(view는 controller를 알지 못한다)

## 4) 장점

널리 사용되고 있는 패턴이며 가장 단순함.

단순함으로 인해 보편적으로 많이 사용되는 디자인 패턴.

## 5) 단점

view와 model 사이의 의존성이 높음. 높은 의존성은 어플리케이션이 커질수록 복잡해지고 유지보수가 어려워진다.

# 2. MVP

MVP 패턴은 Model + View + Presenter를 합친 용어이다. Model과  View는 MVC 패턴과 동일하고 Controller 대신 Presenter가 존재한다.

## 1) 구조

- Model: 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
- View: 사용자에게 보여지는 UI 부분
- Presenter: View에서 요청한 정보로 Model을 가공하여 View에 전달해 주는 부분. View와 Model을 붙여주는 접착제 역할을 한다.

## 2) 동작

1. 사용자의 Action들은 View를 통해 들어온다.
2. View는 데이터를 presenter에 요청한다.
3. presenter는 Model에게 데이터를 요청한다.
4. Model은 Presenter에게서 요청받은 데이터를 응답한다.
5. Presenter는 View에게 데이터를 응답한다.
6. View는 Presenter가 응답한 데이터를 이용하여 화면을 나타낸다.

## 3) 특징

Presenter는 View Model의 인스턴스를 가지고 있어 둘을 연결하는 접착제 역할을 한다. Presenter와 View는 1:1 관계이다.

## 4) 장점

 View와 Model의 의존성이 없다. MVC 모델의 단점이었던 View와  Model의 의존성을 해결하였다

## 5) 단점

View와 Presenter 사이의 의존성이 높아지는 단점이 있다. 어플리케이션이 복잡해 질수록 View와 presenter의 의존성이 강해진다.

# 3. MVVM

MVVM 패턴은 Model + View + ViewModel을 합친 용어이다. Model과 View는 다른 패턴과 동일하다.

## 1) 구조

- Model: 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
- View: 사용자에게 보여지는 UI 부분
- ViewModel: View를 표현하기 위해 만든 View를 위한 Model이다. View를 나타내주기 위한 Model이자 동시에 View를 나타내기 위한 데이터 처리를 하는 부분이다.

## 2) 동작

1. 사용자의 action은 view를    통해 들ㅇㅇㅇ어온다
2. view에 action이 들어오면, command 패턴으로  viewmodel에 action을 전달한다.
3. viewmodel은 model에게 데이터를 요청한다
4. model은 viewmodel에게 요청받은 데이터를 응답한다
5. viewmodel은 응답 받은 데이터를 가공하여 저장한다
6. view는 viewmodel과 data binding을 하여 화면을 나타낸다

## 3) 특징

MVVM패턴은 Command 패턴과 Data Binding 두가지 패턴을 사용하여 구현한다.

두 패턴을 사용하여 View와 ViewModel의 의존성을 없앨 수 있다.

ViewModel과 View는 1:N의 관계이다.

## 4) 장점

View와 Model 사이의 의존성이 없다. 

Command패턴과 DataBinding패턴을 사용하여 View와 ViewModel의 의존성 또한 없다.

각각의 부분은 독립적으로 모듈화하여 개발이 가능하다.(ex: vuex의 모듈화)

## 5) 단점

ViewModel의 설계가 쉽지 않다.

### 추가공부

### 커맨드 패턴

커맨드 패턴이란 요청을 객체의 형태로 캡슐화하여 사용자가 보낸 요청을 나중에 이용할 수 있도록 메서드 이름, 매개변수 등 요청에 필요한 정보를 저장 또는 로깅, 취소할 수 있게 하는 패턴이다. 커맨드 패턴은 명령, 수신자, 발동자, 클라이언트 네개의 용어가 항상 따른다. 커맨드는 수신자 객체를 가지고 있으며 수신자의 메서드를 호출하고, 이에 수신자는 자신에게 정의된 메서드를 수행한다. 커맨드 객체는 별도로 발동자 객체에 전달되어 명령을 발동하게 한다. 발동자 객체는 필요에 따라 명령 발동에 대한 기록을 남길 수 있다. 한 개의 발동자 객체에 다수의 커맨드 객체가 전달 될 수 있다. 클라이언트 객체는 발동자 객체와 하나 이상의 커맨드 객체를 보유한다. 클라이언트 객체는 어느 시점에서 어떤 명령을 수행할지 결정한다. 명령을 수행하려면, 클라이언트 객체는 발동자 객체로 커맨드 객체를 전달한다. 

### 데이터 바인딩

데이터 바인딩은 제공자와 소비자로부터 데이터 원본을 결합시켜 이것들을 동기화하는 기법이다.