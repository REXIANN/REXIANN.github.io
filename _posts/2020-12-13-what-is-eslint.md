---
layout: post
title: "ESLint에 대해 알아보자"
tags: [eslint, ecmascript, lint, vscode, eslintrc]
excerpt_separator: <!--more-->

---

## ESLint란 무엇일까?

한마디로 '작성한 JS코드가 EcmaScript재단에서 명시한 Specifiaction에 부합하는지 검사해주는 툴' 이라고 할 수 있다. ESLint는 문자 그대로 ES와 Lint를 합친 것이다. 

<!--more-->

ES는 EcmaScript로서 Ecma재단에서 만든 Script Specification이라 할 수 있고, Lint는 에러가 있는 코드에 표시를 달아주는 것을 의미한다. 나는 이미 Pylint를 사용해본 경험이 있어 ESLint를 보자마자 무엇인지 대충 감을 잡을 수 있었다. 

ESLint는 코드에 특정 스타일과 규칙을 적용해서 문제를 사전에 찾고 패턴을 적용시킬 수 있는 정적 분석 툴로 분류 한다. 기본적인 기능은 에러도출이나 전반적인 코딩스타일(tab설정, `;` 여부, usingSpaces 등)을 직접 정할 수도 있다. 자신의 스타일과 규칙을 정해서 사용할 수도 있고 대기업에서 사용하는 스타일과 규칙이 정해준 rule을 적용시킬 수도 있다. 

`.eslint` 파일은 JavaScript, JSON 또는 YAML 파일을 이용해서 정의할 수 있다. 단 해당 파일은 적용하고자 하는 폴더들이 모인 루트 폴더에 넣어야 한다. 

### Ex)

```json
your-project
├── .eslintrc
├── lib
│ └── source.js
└─┬ tests
  ├── .eslintrc
  └── test.js
```

이렇게 파일 구조를 설정한 뒤 해당 디렉토리에서 eslint를 실행하면 lib 폴더 안에 있는 모든 파일들이 .eslintrc의 설정에 따라 변환된다. eslint가 tests 폴더에 도착했을 때 해당 폴더 안의 .eslintrc 파일이 추가적으로 적용되어 tests폴더 안의 파일들은 `your-project/.selintrc` 와 `your-project/tests/.eslintrc` 두 설정 파일들의 규칙이 전부 적용되게 된다. 만일 중복되는 Rule이 있는 경우 가장 가까운(해당 폴더 안에 들어있는) .eslint 설정을 따르게 된다고 한다.

`package.json`  과 `.eslint` 파일이 공존하는 경우도 마찬가지이다.

```json
your-project
├── package.json
├── lib
│ └── source.js
└─┬ tests
  ├── .eslintrc
  └── test.js
```

이 경우, 해당 `package.json` 파일이 `eslintConfig` 속성을 가지고 루트 디렉토리에 있는 경우 .json 파일이 명시하는 모든 configuration이 하위 폴더와 파일에 적용된다. 하지만 .eslintrc 파일이 존재하는 tests 폴더에 도착했을 경우 tests 폴더의 하위폴더와 파일들에 대해서 package.json과 .eslintrc의 설정이 겹칠 경우 .eslintrc의 설정을 따르며 오버라이딩 된다고 한다. 

## Install using npm

나는 우선 글로벌에 설치를 했다.

```bash
$ npm install -g eslint
/usr/local/bin/eslint -> /usr/local/lib/node_modules/eslint/bin/eslint.js
+ eslint@7.15.0
added 109 packages from 64 contributors in 3.087s
```

설치 후 VSCode를 통해 하나의 프로젝트를 열고 터미널에 다음과 같이 실행..

```bash
$ eslint --init
✔ How would you like to use ESLint? · problems
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · vue
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser
✔ What format do you want your config file to be in? · JSON
The config that you've selected requires the following dependencies:
...

eslint-plugin-vue@latest
✔ Would you like to install them now with npm? · No / Yes
Installing eslint-plugin-vue@latest
```

몇가지 설정을 해 주고 나면 .eslintrc.json 파일이 생성된 것을 볼 수 있다.

![img//Screen_Shot_2020-12-14_at_3.01.26_PM.png](/assets/img/posts/2020-12-13-what-is-eslint/Screen_Shot_2020-12-14_at_3.01.26_PM.png)

- `env` : 사용 환경을 의미한다.
- `extends` : 사용할 확장 기능에 대해 명시하는 부분. 보통 airbnb-base나 prettier를 추가하여 사용하는 사람들을 볼 수 있었다.
- `parserOptions` : 버전과 모듈 사용 여부
- `plugins` : 사용되는 플러그인.
- `rules` : 세부 설정. 여기에 자신만의 규칙을 추가할 수 있다.

## ESLint Rules

먼저 공식 사이트는 [여기]([https://eslint.org/docs/rules/](https://eslint.org/docs/rules/))로 가면 볼 수 있다.

공식 사이트에 게재된 Rule들을 사용하기 위해서는 `"extends"` 에 `eslint:recommended` 속성이 추가되어 있어야 한다. 사이트에 보면 작은 렌치마크가 있는 속성들이 있는데 문제가 있으면 해당 속성은 코드를 자동으로 수정한다고 한다. Rule은 오브젝트 형태로 키는 속성의 이름이 되고 값은  `"off"`, `"warn"`, `"error"`  세가지가 될 수 있다.

- `"off"` : 해당 Rule을 적용하지 않는다(끈다)
- `"warn"` : 해당 Rule에 위배되면 경고를 띄운다(코드에는 영향을 미치지 않는다)
- `"error"` : 해당 Rule에 위배되면 에러를 발생시킨다(에러가 발생하면 1을 exit code로 반환한다)

터미널에서 eslint를 실행할 때 옵션을 줄 수도 있지만 configuration files에 적용하는 것이 훨씬 보기 쉽다. 적용하면 다음과 같이 된다.

```json
{
    "rules": {
        "eqeqeq": "off",
        "curly": "error",
        "quotes": ["error", "double"]
    }
}
```

또한 추가적인 플러그인들을 설치했을 경우 해당 플러그인에서의 Rule 규칙을 수정할 수도 있다.

```json
{
    "plugins": [
        "plugin1"
    ],
    "rules": {
        "eqeqeq": "off",
        "curly": "error",
        "quotes": ["error", "double"],
        "plugin1/rule1": "error"
    }
}
```

때로는 다른 몇몇 설정파일에서 Rule이 동작하지 않게 수정해야 할 수도 있다. 이럴 때는 `overrides` 와 `files` 키를 같이 사용하여 적용 가능하다.

```json
{
  "rules": {...},
  "overrides": [
    {
      "files": ["*-test.js","*.spec.js"],
      "rules": {
        "no-unused-expressions": "off"
      }
    }
  ]
}
```

## 사용해보자

먼저 두가지 규칙을 추가해서 돌려보기로 했다. `"no-mixed-spaces-and-tabs"` 와 `"no-multiple-empty-lines"` 두가지 이다. 각 속성은 "스페이스와 탭을 혼용하지 말 것", "태그간 한 줄 이상, 또는 불필요한 공백을 두지 말 것" 을 의미하며 둘다 warning으로 설정했다.

![img//Screen_Shot_2020-12-14_at_3.03.32_PM.png](/assets/img/posts/2020-12-13-what-is-eslint/Screen_Shot_2020-12-14_at_3.03.32_PM.png)

수정 전과 수정 후의 파일 모습이다. 두칸 이상 띄워진 부분은 자동적으로 ESLint가 수정해준다.

![img//Screen_Shot_2020-12-14_at_3.02.53_PM.png](/assets/img/posts/2020-12-13-what-is-eslint/Screen_Shot_2020-12-14_at_3.02.53_PM.png)

![img//Screen_Shot_2020-12-14_at_3.04.40_PM.png](/assets/img/posts/2020-12-13-what-is-eslint/Screen_Shot_2020-12-14_at_3.04.40_PM.png)

간략하게 몇 개의 좋은 Rules를 소개한다.

- `semi` : `"always"`, `"never"` 두 개의 옵션이 있다. 문장의 뒤에 세미콜론(`;`)을 항상 붙일지 붙이지 않을지 선택할 수 있다. 기본값은 `"always"` 이다.
- `quotes` : 싱글쿼트, 더블쿼트 또는 백틱으로의 스트링표기법 일치를 권장한다. 기본값을 더블쿼트로 설정되어 있다.

`.eslintrc.json` 파일에서 다음과 같이 설정이 가능하다

```json
{
  "rules": {
    // 세미콜론은 사용하지 않으며 사용할시 경고를 띄움
    "semi": ["warn", "never"]
	  // 더블쿼터(")만을 사용하며 백틱(TemplateLiterals)은 허용됨. 어길시 에러를 발생시킴.
    "quotes": ["error", "double", { "allowTemplateLiterals": true }] 
  }
}
```

## 꿀팁

추가적으로 ESLint 검사에서 제외하고 싶은 파일들을 설정하기 위해서는 `.eslintignore` 파일을 만들면 된다고 한다! `.gitignore` 와 동일한 역할을 하는 듯 하다.

```bash
$ touch .eslintignore 
# 파일을 열고 ./node_modules/** 을 적어주었다
```