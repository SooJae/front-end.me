---
title: 'npm과 npx의 차이'
date: 2020-10-09 21:40:00
category: Tool
thumbnail: './images/npm-npx.png'
draft: false
---

이 글은 Magdalena Siljanoska의 [Yes, it’s npx, not npm — the difference explained](https://medium.com/javascript-in-plain-english/yes-its-npx-not-npm-the-difference-explained-58cbb202ec33) 를 번역한 게시물입니다.   
Thank you Magdalena Siljanoska! Thanks to your writing, I can grow further as a developer.

---

![npm_npx](./images/npm_npx.gif)

나는 최근에 [리액트를 배우면서](https://medium.com/javascript-in-plain-english/how-to-build-a-simple-task-manager-with-react-8895d4526b2e) 내가 잘 알고 있는 `npm`이 아닌 `npx`를 보고 혼란스러워했고, 많은 사람들이 혼란스러워하는 모습을 봤다.  
우리들 중 일부는 그것이 이상하다고 생각했지만 별 생각이 없었고, 또 다른 사람들은 오타라고 생각하고 `npm`을 `npx`로 **수정** 하는 바람에 어려움을 겪었다.  
  
몇번 더 이런 상황을 겪게 되니 이것에 대해 설명할 가치가 있다고 생각했다. 그래서 이 게시물을 작성하게 되었다. 나와 같이 npx에 대해 오해하고 있었던 모든 사람들을 위해서.  
  

> npm이 아니라 npx가 맞아! 오타가 아니야!😃

## NPM

  
우리가 알고있듯이, [npm](https://www.npmjs.com/)은 **의존성(dependency)** 과 **패키지 관리(package management)**를 자동화 하기 위한 Node.js의 패키지 매니저이다.  
  
우리는 프로젝트를 위한 모든 의존성 패키지를 `package.json` 파일에 지정할 수 있다. 그리고 언제든지, 그리고 누구라도 그 의존성 패키지들을 설치해야 할때는 단순히 `npm install` 만 타이핑하면 된다!  
  
그것은 또한 **versioning**을 제공한다. **versioning**이란 우리 프로젝트에 의존하는 의존성 패키지들의 버전을 지정할 수 있게 해주는 것을 말한다.  
그래서 대부분의 경우, 패키지의 버전 업데이트로 인해 우리의 프로젝트가 깨지는 것을 방지하거나, 우리가 선호하는 버전을 이용할 수 있게 해준다.  
  

## NPX

  
그와 반대로 [npx](https://www.npmjs.com/package/npx)는 **Node 패키지들을 실행하는 도구**다. 그리고 npm 5.2버전부터 npm 안에 포함되어있다.  

`npx`는 다음과 같이 동작한다.  

> 1\. npx는 우선 기본적으로, 실행할 패키지가 실행 가능한 경로에 있는지 확인한다. (예를 들면, 우리의 프로젝트내에서 확인한다.)  
> 2\. 만약에 있다면, 그것을 실행한다.  
> 3\. 아니라면 패키지가 설치가 되지 않았다는 것으로 판단하여, npx가 가장 최신 버전의 패키지를 설치하고 실행한다.  

  
위의 행동들은 기본 설정일때의 `npx` 동작방식이다. flags를 설정하면 사용자에 맞게 변경할 수 있다.  
  
예를 들면, 만약에 `npx some-package --no-install` 을 입력하면, `npx`는 `some-package`가 **기존에** **설치되어있지 않더라도** **some-package**를 **실행**시킨다.

> npx flags에 대해 자세히 알고 싶다면 [여기로](https://www.npmjs.com/package/npx) 가라.  

## Example

  
예를들어 우리의 패키지가 `my-package`고, 이 패키지를 실행시키고 싶다고 생각해보자.  
  
글쎄, `npx`가 없이 패키지를 실행하려면, 우리는 지역 경로(local path)로부터 my-package를다음과 같이 실행해야할 것이다.  

```
$ ./node_modules/.bin/my-package
```

  
  
또는 `package.json` 파일의 scripts 부분에 작성하는 방법도 있다. 다음과 같이.

```
{
  "name": "something",
  "version": "1.0.0",
  "scripts": {
  "my-package": "./node_modules/.bin/my-package"
  }
}

```

  
  
그리고 `npm run my-package` 로 실행하면 된다.  
  
반대로 `npx`를 이용하면 우리는 단순히 `npx my-package` 만 타이핑 하면 된다.  
  

## 결론  

  
덧붙이자면 `npm !== npx` 라는 것이고, 이 짧은 글이 당신의 `npx`에 대한 오해를 정리하는데 도움이 되었기를 바란다.  
  
만약 다른 질문이 있으면 언제든지 댓글을 달면 된다.  
  
해피코딩! 😊

---

글에 번역 오류가 있으면 알려주세요 감사합니다.
