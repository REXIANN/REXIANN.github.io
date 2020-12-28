---
layout: post
title: "HTTP와 REST API"
tags: [HTTP, REST API, collections, document]
excerpt_separator: <!--more-->

---

## HTTP

HyperText Transfer Protocol의 약자로 주로 웹에서 사용되는 통신 규약 중 하나이다. 

<!--more-->

서버와 클라이언트와 요청과 응답을 주고받는 통신 규약(프로토콜)이다. 기본포트로 80번을 사용한다(포트를 80 또는 443 으로 지정하면 URL 뒤에 포트번호를 지정하지 않아도 접속된다).

요청은 URL로 보낸다.

7계층인 어플리케이션에서 동작하며 4계층 전송계층의 TCP/IP 프로토콜 위에서 동작한다. 따라서 우리가 브라우저를 사용하여 어딘가에 접속한다는 것은 TCPIP 통신이 베이스에 있다는 의미!

### 요청메시지

- 요청 내용 (ex: `GET http://google.com HTTP/1.1`)
- 헤더 → (ex: `Accept-Language: en, Authorization: Token abcdefg123456` )
- 빈줄
- 메시지

```jsx
GET /restapi/v1.0 HTTP/1.1
Accept: application/json
Authorization: Bearer UExBMDFUMDRQV1MwMnzpdvtYYNWMSJ7CL8h0zM6q6a9ntw
```

### 응답메시지

- 상태표시행: 상태코드와 reason message를 포함한다 (ex: `HTTP/1.1 200 OK`)
- 응답헤더필드 (ex: `Date: Mon, 28 2020 15:30:43 GMT + 9`)
- 빈줄
- 메시지

```jsx
HTTP/1.1 200 OK
Date: Mon, 23 May 2005 22:38:34 GMT
Content-Type: text/html; charset=UTF-8
Content-Encoding: UTF-8
Content-Length: 138
Last-Modified: Wed, 08 Jan 2003 23:11:55 GMT
Server: Apache/1.3.3.7 (Unix) (Red-Hat/Linux)
ETag: "3f80f-1b6-3e1cb03b"
Accept-Ranges: bytes
Connection: close

<html>
<head>
  <title>An Example Page</title>
</head>
<body>
  Hello World, this is a very simple HTML document.
</body>
</html>
```

### HTTP의 두가지 특징

- 서버는 요청에 대한 응답이 끝나면 연결을 끊는다(connectless)
- 서버는 사용자를 식별할 수 없다(stateless) → http 통신 자체로는 사용자 식별이 불가능하다는 의미. 요청 헤더에 있는 쿠키로 판별하는 것은 추가적인 기술이다!

### 응답코드

자주 보는 응답코드에 대해 간단히 정리하고 넘어가자

- 2XX(Success): 성공. 데이터 전송이 성공적으로 이루어졌거나 이해되었거다 수락되었음
    - 200(OK): 오류 없이 전송 성공
- 3XX(Redirection): 자료의 위치가 바뀌었음
    - 301: 요구한 데이터를 변경된 URL에서 찾음
- 4XX(Client Error): 클라이언트 측의 오류. 주소를 잘못 입력하였거나 요청이 잘못 되었음
    - 401(Bad request): 요청 실패. 문법상의 오류가 있어 서버가 요청사항을 이해하지 못함
    - 401(Unauthorized): 권한 없음. 요청사항이 서버에 기록된 권한과 맞지 않음.
    - 403(Forbidden): 접근 금지. 해당 유저에게 사용권한(인가)이 없음
    - 404(Not Found): 서버가 요청한 파일이나 스크립트를 찾을 수 없음
- 5XX(Server Error): 서버 측의 오류로 인해 올바른 요청을 처리할 수 없음
    - 500(Internal Server Error): 서버 내부 오류

### HTTP 메서드

HTTP는 메서드를 통해 다양한 전송방식을 제공한다.

- GET → 가져오다
- POST → 게시하다
- PUT → 집어넣다
- PATCH → 고치다
- DELETE → 지우다

HTML 폼에서 주로 사용되는 메서드는 GET과 POST이나 HTTP는 기본적으로 다양한 메서드를 지원한다. 이 메서드와 URL의 표현방식을 사용하여 요청을 구성하는 방법이 바로 REST API이다.

---

## REST API

Representational State Transfer, 즉 **자원의 표현에 의한 상태전달** 이라 할 수 있다. REST는 웹(정확하게는 월드 와이드 웹)에서 사용되는 소프트웨어 아키텍쳐의 한 형식이다. 웹 상의 자료를 HTTP위에서 별도의 전송계층 없이 전송하기 위한 간단한 인터페이스이다. 

**URI를 사용하여 자원을 명시하고 메서드를 사용하여 행위를 정의한다.** 

### URI와 URL

URI를 사용하여 자원의 상태를 정의한다.

URI(Uniform Resource Identifier) → 굳이 번역을 하자면 통합자원식별자 이다. 인터넷에 있는 자원을 나타내는 유일한 주소이다. URI는 인터넷에서 요구되는 기본 조건으로 인터넷 프로토콜에 항상 붙어 다닌다. URI의 하위개념으로는 URL, URN등이 있다.

URL(Uniform Resource Locator) → 네트워크 상에 자원이 어디있는지 알려주기 위한 규약이다. 웹 리소스를 찾기 위한 규약으로 쉽게 말해서 웹 페이지를 찾기 위한 주소를 말한다. 웹사이트 주소로 찾각할 수 있지만 URL은 웹 사이트 주소 뿐만 아니라 컴퓨터 네트워크상의 자원을 모두 나타낼 수 있다. 해당 주소에 접속하기 위해서는 URL에 맞는 프로토콜을 알아야 하고 그와 동일한 프로토콜로 접속해야 한다(HTTP의 경우 웹 브라우저를 사용해야 한다. FTP나 SMTP같은 HTTP가 아닌 프로토콜도 있음!)

```jsx
GET /articles/1
GET /accounts/123
// 컬렉션과 도큐먼트 규칙을 적용!
GET /movies/genres/horror
GET /sports/soccer/players/14524
```

### 🅰️ 컬렉션과 도큐먼트!

URI를 통해 자원을 명시할 때 보다 직관적으로 명시하기 위해 사용되는 방법이다. 

- 컬렉션(복수형태) → 컬렉션들이나 도큐먼트들의 집합이다.
- 도큐먼트(단수형태) → 하나의 문서 또는 객체이다.

### 메서드

네 개의 HTTP 메서드를 사용하여 행위를 정의한다.

- GET → 해당 자원을 조회한다. 해당 리소스의 자세한 정보를 가져온다.
- POST → 리소스를 생성한다.
- PUT → 해당 리소스를 수정한다.
- DELETE → 해당 리소스를 삭제한다.

```jsx
GET /movies/1234 // id가 1234인 영화의 정보를 요청한다
POST /articles // 새로운 기사를 생성(작성)한다
PUT /articles/1234 // 1234번 id를 가진 기사의 내용을 수정한다
DELETE /articles/1234 // 1234번 id를 가진 기사를 삭제한다
```

### REST API의 장단점

장점

- HTTP 프로토콜의 인프라를 그대로 사용하므로 별도의 인프라 구축이 필요하지 않다.
- HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용가능하다
- REST API 메시지가 의도하는 바를 명확하게 나타낼 수 있다
- 서버와 클라이언트의 역할을 명확하게 분리한다

단점

- 사용할 수 있는 메서드가 4가지 밖에 없다.
- PUT, DELETE를 지원하지 않는 구형 브라우저에서 사용이 불가능하다.