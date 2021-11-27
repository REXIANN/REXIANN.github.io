---
layout: post
title: "NPM(node package manager)"
tags: [npm, node package manager, nodejs]
excerpt_separator: <!--more-->
sitemap :
  changefreq : daily
  priority : 1.0


---

NPM(Node Package Manager)는 자바스크립트 라이브러리들을 설치하고 관리할 수 있는 패키지 매니저이다.

<!--more-->

npm 은 node.js 를 설치하면 같이 설치된다(Node.js 는 브라우저 밖에서 자바스크립트를 실행할 수 있는 환경을 의미한다).

자바스크립트의 강력한 파워 중 하나는 다양한 라이브러리인데, 전 세계에 있는 모든 자바스크립트 개발자들이 자신들이 만든 라이브러리를 공개된 저장소에 올려놓고, 명령어로 다른 사람들이 만든 라이브러리를 편하게 다운로드 받을 수 있다.

명령어는 `npm` 을 사용한다.



## NPM 시작하기

먼저 Node.js가 설치되어 있어야 한다. 

폴더를 하나 만들고  `npm init -y` 를 실행한다. `package.json` 파일이 생성되며 내용은 아마 다음과 같을 것이다.

```jsx
{
  "name": "npm-test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

`package.json` 파일은 반드시 `name` 과 `version` 항목을 포함해야 한다.

- `name` : 소문자, 하이픈(`-`), 언더스코어(`_`)로 이루어진 한 단어가 되어야 한다
- `version` : `x.x.x` 형식을 따라야 하며 **시맨틱 버저닝** 이라는 규칙을 따라야 한다.

위의 구조가 가장 기본적인 구조이며 실제 애플리케이션을 만들 때 자주 사용되는 속성은 

- `scripts` : `npm` 명령어와 함께 사용할 커스텀 명령어 정의
- `dependencies` : 프로덕션 환경에서 어플리케이션이 사용하는 라이브러리
- `devDependencies` : 로컬에서의 개발 및 테스트에만 사용하는 라이브러리

이다.

이제 해당 폴더 안에서는 npm 이 프로젝트에 사용할 라이브러리를 관리한다. 

## 설치

설치를 위한 명령어는 다음과 같다

```bash
npm install vue

# install 대신 i를 사용할 수 있다
npm i vue
```

### 전역설치

어떠한 라이브러리를 특정한 프로젝트에서만 사용하는 것이 아니라 시스템 레벨에서 사용하고자 할 때 사용할 수 있다.

```bash
npm install gulp --global
# --global 대신 -g를 써도 무방하다
npm i -g gulp
```

### 설치 안내에 따라오는 `--save-dev` 나 `--save-prod` 는 무엇을 의미할까?

결론: `--save-dev` 와 `--save-prod` 옵션은 `npm install` 명령어로 라이브러리를 설치할 때 해당 라이브러리를 `devDependencies` 에 넣을지 `dependencies` 에 넣을지 결정해주는 역할을 한다. 

가끔 외부 라이브러리 설치를 하기 위해 문서를 읽다보면 `npm install` 과 함께 `--save-dev` 나 `--save-prod` 명령어를 자주 볼 때가 있다. 특히, `--save-dev` 옵션은 나도 여러번 마주쳤을 정도로 흔하게 보인다. 그런데 해당 옵션을 사용하지 않아도 설치가 되긴 된다. 

`package.json` 파일을 보면 두개의 dependency 가 존재한다. `dependencies` 와 `devDependencies` 가 바로 그것이다. `dependencies` 는 실제로 어플리케이션이 돌아가는 데 필요한 라이브러리들을 적고, 개발 및 테스트시 개발자에게 필요한 라이브러리들은 `devDependencies` 에 적어야 한다.

다시말해, 어떤 라이브러리가 프로젝트의 컴파일(또는 빌드) 타임에 필요하다면 `devDependencies` 에, 빌드된 프로젝트의 런타임에 쓰인다면 `dependencies` 에 넣어주는 것이다. 개발용 라이브러리와 배포용 라이브러리를 구분해주는 역할을 한다고 볼 수 있겠다.

개발할 때만 사용하고 배포할 때는 빠져도 좋은 라이브러리의 예시는 다음과 같다.

- `webpack` : 빌드 도구
- `eslint` : 코드 문법 검사 도구
- `imagemin` : 이미지 압축 도구

`npm` 5 버전부터는 `--save` 옵션을 사용하지 않아도 자동으로 `dependencies` 에 항목을 추가해 준다. 그러므로 해당 라이브러리가 개발시에만 사용된다면 `--save-dev` 옵션을, 아니라면 아무것도 붙이지 않아도 무방하다.

- `-P` or `--save-prod` : `package.json` 의 `dependencies` 에 패키지를 등록 (DEFAULT)
- `-D` or `--save-dev` : `package.json` 의 `devDependencies` 에 패키지를 등록
- `-O` or `--save-optional` : `package.json` 의 `optionalDependencies` 에 패키지를 등록
- `--no-save` : `dependencies` 에 패키지를 등록하지 않음

## 실행

`npm` 은 사용자가 임의로 명령어의 이름과 동작을 정의해서 사용할 수 있도록 도와준다.

`package.json` 의 `scripts` 에 명령어의 이름과 그 이름을 입력했을 때 수행할 동작을 정의할 수 있다. 수행하는 동작은 쉘에서 실행된다.

```jsx
// package.json
{
	...
	"scripts": {
		"hello": "echo hi"
	}
}
```

NPM 커스텀 명령어는 모두 `npm run 명령어이름` 형식으로 실행할 수 있다.

`hello` 라는 이름의 명령어는 다음과 같이 실행할 수 있다.

```bash
npm run hello
# hi
```

이제 매번 긴 명령어를 타이핑 할 필요 없이 커스텀 명령어를 사용해 손쉽게 원하는 작업을 수행할 수 있다.

```jsx
"scripts": {
	"build": "cross-env NODE_ENV=production webpack --progress --hide-modules"
}
```

### 커스텀 명령어의 조합

커스텀 명령어의 또 다른 장점은 해당 명령어에 실행 옵션 뿐만 아니라 다른 커스텀 명령어를 조합할 수 있다는 점이다.

```jsx
"scripts": {
  "build": "webpack",
  "deploy": "npm run build -- --mode production"
}
```

다음과 같이 정의하고 `npm run deploy` 명령어를 실행한다면 

1. 먼저 `build` 에 정의한 `webpack` 명령어가 실행되면서 
2. 뒤쪽에 붙은 실행 옵션들이 수행된다

여기서는 `webpack` 이라는 도구의 `mode` 에 `production` 이라는 값을 넘겨주어서 수행하도록 했다고 볼 수 있다. 

이처럼 커스텀 명령어를 다시 한번 커스텀하여 다양하게 사용할 수도 있다.