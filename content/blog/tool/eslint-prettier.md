---
title: 'ESLint와 Prettier'
date: 2020-10-7 17:33:00
category: Tool
thumbnail: './images/eslint.jpeg'
draft: false
---

안녕하세요. 오늘은 ESLint와 Prettier의 개념과, 사용법, 그리고 차이점에 대해 포스팅하려고 합니다.

## ESLint
![eslint](./images/eslint.jpeg)

### Linter의 기능

느슨한 형식의 언어인 Javascript에서는 코드 에러가 자주 발생합니다.    
하지만 JavaScript는 동적 분석(프로그램을 직접 실행해서 코드를 분석 <-> 프로그램을 실행하지 않고 분석하는 정적 분석)을 하기 때문에  에러를 찾기 위해서는 코드를 직접 실행해서 확인을 해봐야 합니다.

이를 도와주는 것이 Linter입니다.   
Linter는 코드를 정적으로 분석하기 때문에, 프로그램을 실행하지 않고도 코딩 컨벤션에 위배되는 코드나 안티 패턴을 자동으로 검출해줍니다. 추가적으로 간단한 코드 포맷팅 기능도 있습니다.  
  

### ESLint 설치  

```shell script
$ npm i -D eslint
```

  
이제 eslint를 실행해봅시다.

```shell script
$ ./node_modules/.bin/eslint -v
```

그런데 `./node\_modules/.bin/eslint` 를 직접 작성하기에는 번거로울 뿐만 아니라 보기에도 좋지 않다는 게 느껴질 것입니다. 이를 해결하기 위해 글로벌로도 설치해주세요.

```shell script
$ npm i -g eslint
```

이제 eslint 명령어만로도 실행이 가능합니다.

```shell script
$ eslint -v
```

※ 글로벌로 설치하기 싫거나, 설치를 하지 못하는 상황이고, 로컬에 있는 eslint만 사용하고 싶은데 `./node\_modules/.bin/eslint`를 작성하기 힘들 때는 npx를 사용하면 됩니다. ([npx란?](https://soojae.tistory.com/40))

```shell script
$ npx eslint -v
```

이제 ESLint 패키지 설치가 완료되면 초기화를 해주세요.

```shell script
$ eslint --init
```

그럼 다음과 같은 단계별 질문이 나옵니다.

```shell script
? How would you like to use ESLint? … 
To check syntax only
❯ To check syntax and find problems
To check syntax, find problems, and enforce code style

// ESLint의 사용 목적은 무엇인가요?

? What type of modules does your project use? … 
❯ JavaScript modules (import/export)
CommonJS (require/exports)
None of these

// 어떤 타입의 모듈을 사용하나요?

? Which framework does your project use? … 
React
Vue.js
❯None of these

// 어떤 프레임워크에서 사용할 것인가요? (앵귤러는 왜 없나요...)

? Does your project use TypeScript? › No / Yes

// 타입 스크립트를 사용하나요?

? Where does your code run? … (Press <space> to select, <a> to toggle all, <i> to invert selection)
✔ Browser
✔ Node

// 어떤 환경에서 코드가 동작하나요?

? What format do you want your config file to be in? … 
JavaScript
YAML
❯ JSON

// 어떤 포맷으로 ESLint 설정파일을 생성할까요?
```

완료되면 .eslintrc.json 파일이 생성되며, 이 파일에는 ESLint에 대한 설정들이 있습니다.   
.eslintrc.json 파일을 만들기 싫다면, package.json 파일 내의 eslintConfig 부분에서도 설정할 수 있습니다.

.eslintignore 파일을 생성하여 해당 파일, 폴더들에 대한 ESLint를 무시할 수도 있습니다. (프로젝트 내의 /node\_modules/\* and /bower\_components/\* 는 기본적으로 제외되어 있습니다.)

그럼 이제 예제를 통해 ESLint를 적용해 봅시다. test.js 파일을 생성해 주세요.

```js
// test.js

const abc = 0
```

  
  
test.js 파일 생성후, 커맨드 창에 아래와 같이 입력하면 다음과 같은 메시지를 확인할 수 있습니다.  

```shell script
$ eslint test.js


/Users/soojae/IdeaProjects/Blog/Tools/Example/test.js
1:7 error 'abc' is assigned a value but never used no-unused-vars

✖ 1 problem (1 error, 0 warnings)
```

내용을 보니 변수를 선언한 후, 사용을 하지 않아서 발생한 에러인 것을 확인할 수 있습니다.

`console.log(abc)`를 추가해서 abc를 사용해주세요.

```js {5}
// test.js

const abc = 0

console.log(abc)
```

다시 `eslint test.js`를 입력하면 에러가 발생하지 않는 것을 확인할 수 있습니다.

이제 간단한 에러를 자동으로 수정해 주는 `--fix`를 사용해봅시다.

```json
// package.json

[
  rules: {
  "semi": "error",
  }
]

```

  
`"semi"`는 세미콜론(;)이 필요한 위치에 없을 시 에러를 발생시킵니다.

```js
$ eslint test.js --fix
```

test.js 파일을 확인해보면, 세미콜론(;)이 자동적으로 추가된 것을 확인할 수 있습니다.  

```js
// test.js

const abc = 0;

console.log(abc);
```

이제 ESLint와 한 세트로 붙어 다니는 Prettier에 대해 알아봅시다.

---

![prettier](./images/prettier.png)

## Prettier

  
Prettier는 어느 경우에 사용할까요? 사실 실 생활에서도 Pretter가 적용되어 있습니다.  
탕수육(Code)을 먹을 때 부먹이냐 찍먹이냐의 싸울 때, 결국 사주는 사람(Pritter)의 취향으로 가게 됩니다.  
코드로 본다면 한 개발자는 indent 값을 2로 주고 싶은데, 또 다른 개발자는 4로 주고 싶을 경우가 있을 것입니다. 이를 해결해 주는 것이 Prettier입니다.

 
Prettier는 2016년에 등장한 코드 포맷터입니다. Prettier를 사용하면 코드를 완전히 다시 작성해줍니다.(변경이 필요한 부분만 바꾸는 것이 아니라, 코드 전체를 새로 작성합니다.)  
새로 작성한다고 해도 코드 내용은 변하지 않고, 구조적 뷰만 변경됩니다.

```
$ npm i -g -E prettier
$ npm i -D -E prettier
```

\-E는 --save-exac의 축약어로서 해당 패키지를 정확한 버전으로 설치해줍니다. Prettier에서는 버전이 달라지면서 스타일이 변할 수 있기 때문에 이 옵션을 붙이는 것을 권장합니다.       
이제 설정 파일인 .prettierrc.json 파일을 생성하고, Prettier 옵션을 [https://prettier.io/docs/en/options.html](https://prettier.io/docs/en/options.html) 에 들어가서 알맞게 변경해주세요. 아래는 예시입니다.  

```json
//.prettierrc.json

{
  "arrowParens": "avoid",
  "bracketSpacing": true,
  "htmlWhitespaceSensitivity": "css",
  "insertPragma": false,
  "jsxBracketSameLine": false,
  "jsxSingleQuote": false,
  "printWidth": 80,
  "proseWrap": "preserve",
  "quoteProps": "as-needed",
  "requirePragma": false,
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "none",
  "useTabs": false
}
```

이제 test.js 파일을 다음과 같이 수정해주세요.

```js
// test.js

const abc
= 0
    
console.
log(abc)
```

prettier를 적용하면 다음과 같은 결과가 나옵니다.

```shell script
$ prettier test.js --write
```

```js
// test.js

const abc = 0;
    
console.log(abc);
```

## ESLint VS Prettier

ESLint는 코드 포맷터의 역할도 하지만, 주로 코드 에러를 잡아내고 코드 문법을 강제하는 등 코드 품질을 개선에 중점을 두었습니다.  
Prettier는 코드의 최대 길이, 함수에서, 작은따옴표(')를 사용할 것인지 아니면 큰 따옴표(")를 사용할 것인지 등 코드가 예쁘게 보이도록 하는지에 중점을 두었습니다. 하지만 코드의 에러를 잡아내진 못합니다.  
Prettier 공식 홈페이지에는 아래와 같은 문구가 있습니다.

> use Prettier for formatting and linters for catching bugs!
  
즉 ESLint에도 코드 포맷팅이 있긴 하지만 Prettier의 코드 포맷팅에 비해 특화되어있지 않으므로, 코드 에러와 문법을 정적으로 탐지할때는 ESLint를 사용하고 포맷팅에는 Prettier를 사용하면 됩니다.  
  
---

## ESLint + Prettier

  
이제 ESLint와 Prettier를 같이 사용해봅시다.  
하지만 위에서 말씀드렸듯이, ESLint가 코드 포맷터의 역할도 하기 때문에 해당 Prettier와 충돌할 수도 있습니다.  
이를 방지하기 위해서 다음 패키지들을 추가합니다.  

```shell script
$ npm i -D eslint-config-prettier
```

eslint-config-prettier 패키지는 ESLint 규칙에서 Pretter 규칙과 충돌하는 규칙들을 OFF 시키는 역할을 합니다.  


```shell script
$ npm i -D eslint-plugin-prettier
```
  
eslint-plugin-prettier는 Prettier 규칙들을 ESLint 규칙에 추가하는 패키지입니다. 즉, eslint --fix 만 실행해줘도 prettier --write를 사용할 필요 없이 prettier까지 적용시켜줍니다.  


.eslintrc.json 에 다음을 추가해주세요.

```json
// .eslintrc.json

{
    "plugins": [
        "prettier"
    ],
    "rules": {
        "prettier/prettier": "error"
    }
}
```

이제 ESLint만 실행해도 Prettier의 포매팅 기능들을 포함해서 실행합니다.  
  
### Git 저장소에 푸시 전 ESLint, Prettier를 강제 적용
  
매번 ESLint를 실행하는 것은 번거로울 뿐만 아니라 같은 팀 내 개발자의 실수로 ESLint, Prettier를 적용하지 않은 코드들이 저장소에 푸시될 수도 있습니다.  
그렇게 되면 깃 로그에 'ESLint, Prettier 적용' 같은 생산성 없는 로그들이 쌓이게 됩니다.  
이런 경우를 방지하기 위해 commit 전에 코드들에게 ESLint와 Prettier이 강제적으로 적용되도록 만들어 봅시다.  
  
이를 위해서는 **husky**와 **lint-staged** 패키지가 추가로 필요합니다.  


## Husky

```shell script
$ npm i -D husky
```
  
Git Hook을 편리하게 관리해주는 툴입니다.  
git의 특정 이벤트(commit, push, pull 등)가 발생하려고 하면, 그 순간에 Hook을 해서 스크립트가 실행되도록 해주는 것이 Git Hook입니다.  
  
Git Hook에 대한 자세한 설명은 [훅으로 Git에 훅 들어가기](https://woowabros.github.io/tools/2017/07/12/git_hook.html) 에서 확인하시면 됩니다.  
  
간단하게 사용할 용어들만 정리했습니다.

- pre-commit : 커밋 메시지를 작성하기 전에 호출됨  
- prepare-commit-msg : 커밋 메시지 생성 후 편집기 실행 전에 호출됨  
- commit-msg : 커밋 메시지와 관련된 명령을 넣을 때 호출됨  
- post-commit : 커밋이 완료되면 호출됨  
- pre-push : 푸시가 실행되기 전에 호출됨

Husky도 .huskyrc, .huskyrc.json, .huskyrc.js, husky.config.js와 같은 별도의 설정 파일을 만들 수도 있습니다.  
package.json 파일에 husky를 추가해주세요.

```json
// package.json

"husky": {
    "hooks": {
      "pre-commit": "eslint --fix && prettier --write",
      "pre-push": "npm test"
    }
  },
```

### lint-staged

```shell script
$ npm i -D lint-staged
```

husky만 사용한다면 프로젝트 내의 모든 코드를 검사하기 때문에 비효율 적입니다. lint-staged는 git에 staged 상태인 파일만 lint 해주는 도구입니다.

```json
// package.json

"husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "pre-push": "npm test"
    }
  },
  "lint-staged": {
    "*.js": [
      "eslint --fix"
      "prettier --write"
    ]
  }
```

사실 위에서 eslint-plugin-prettier 패키지를 설치했기 때문에 **eslint --fix**만 입력 **prettier --write**까지 적용이 됩니다. 이제 에러를 잘 잡아내는지 확인하기 위해 `console.log(abc)`를 제거해주세요.

```js
// test.js

const abc
= 0
```

이제 commit을 해주세요.

```shell script
$ git commit -m "[Tools] eslint husky lint-staged 테스트"

husky > pre-commit (node v12.18.0)
✔ Preparing...
⚠ Running tasks...
❯ Running tasks for *.js
✖ eslint --fix [FAILED]
◼ prettier --write
↓ Skipped because of errors from tasks. [SKIPPED]
✔ Reverting to original state because of errors...
✔ Cleaning up... 

✖ eslint --fix:

/Users/soojae/IdeaProjects/Blog/Tools/Example/test.js
1:7 error 'abc' is assigned a value but never used no-unused-vars

✖ 1 problem (1 error, 0 warnings)

husky > pre-commit hook failed (add --no-verify to bypass)
```

  
변수 abc를 사용하지 않아 에러가 발생했고, commit이 발생되지 않았습니다.  
Prettier까지도 적용이 되지 않는 것을 보아, ESLint 단계에서 에러를 잡아내자마자 실행이 멈췄다는 것을 알 수 있습니다. 이제 ESLint 규칙에 위배되는 코드들이 Git 저장소에 올라 올 일은 없을 것입니다.  
이제 다시 `console.log(abc)`를 추가해주세요.

```js {6,7}
// test.js

const abc
= 0

console.log(abc)
```
  
다시 커밋을 하면 아래와 같은 메시지가 나옵니다.  

```shell script
$ git commit -m "[Tools] eslint husky lint-staged 테스트"

husky > pre-commit (node v12.18.0)
✔ Preparing...
✔ Running tasks...
✔ Applying modifications...
✔ Cleaning up... 
[master 17e09f6] [Tools] eslint huskey lint-staged 테스트
3 files changed, 17 insertions(+), 15 deletions(-)

```

commit이 완료되면 test.js에 **prettier**까지 자동적으로 적용된 것을 확인할 수 있습니다.

```js
// test.js
// prettier까지 적용된 것을 확인 할 수 있습니다.

const abc = 0;

console.log(abc);
```

현재 회사에서는 TSLint 만 사용하고 있습니다. 하지만 이번 대형 프로젝트에 투입되면서, 제대로 Lint가 제대로 적용이 안된 코드들이 올라오고 코드 스타일도 각각 달라서 혼란스러웠습니다.  
다음 프로젝트에는 ESLint (2019년 이후로 TSLint 업데이트가 중단되고 ESLint에 추가된다고 합니다. [TSLint in 2019](https://medium.com/palantir/tslint-in-2019-1a144c2317a9)를 참고해주세요.)와 prettier, husky, 그리고 lint-staged을 사용할 수 있도록 건의를 드려보려고 합니다.  
  
그리고 앞으로 진행할 저의 토이 프로젝트에는 **무조건**적용시킬 겁니다. ESLint와 Prettier는 그만큼 강력하고 개발자가 코드에만 집중할 수 있도록 도와주는 유용한 도구입니다.

글에 오류가 있으면 알려주세요. 감사합니다.

---

## REFERENCES

  
JBEE, 「 Code Formatting 자동화 」,  [https://jbee.io/web/formatting-code-automatically/](https://jbee.io/web/formatting-code-automatically/)    

Chameera Dulanga, 「 ESLint vs. Prettier 」,  [https://medium.com/better-programming/ESLint-vs-prettier-57882d0fec1d](https://medium.com/better-programming/ESLint-vs-prettier-57882d0fec1d) 

subicura, 「 linter를 이용한 코딩스타일과 에러 체크하기 」, [https://subicura.com/2016/07/11/coding-convention.html](https://subicura.com/2016/07/11/coding-convention.html)

feynubrick, 「 VS Code에서 ESlint와 Prettier 함께 사용하기 」, [https://feynubrick.github.io/2019/05/20/eslint-prettier.html](https://feynubrick.github.io/2019/05/20/eslint-prettier.html) 

라태웅, 「 훅으로 Git에 훅 들어가기 」, [https://woowabros.github.io/tools/2017/07/12/git\_hook.html](https://woowabros.github.io/tools/2017/07/12/git_hook.html) 

Palantir, 「 TSLint in 2019 」, [https://medium.com/palantir/tslint-in-2019-1a144c2317a9](https://medium.com/palantir/tslint-in-2019-1a144c2317a9)
