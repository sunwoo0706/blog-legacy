---
date: '2022-04-25'
title: 'Yarn Berry PnP에서 cypress-react-selector가 작동하지 않는 경우'
categories: ['YarnBerry', 'Cypress']
summary: 'Yarn Berry의 PnP 환경에서 cypress를 설정하고 cypress-react-selector를 사용하려 했지만 에러가 발생하며 cypress 테스트가 실행되지 않았다. 이유와 해결방법을 알아보자.'
---

## 문제 상황

[YarnPnP + NextJS + EmotionJS + Typescript](https://github.com/sunwoo0706/NextJS_Template) 환경에서 `cypress` 세팅을 하고 [`cypress-react-selector`](https://www.npmjs.com/package/cypress-react-selector) 를 사용하려 `cypress open`을 사용했지만 다음과 같은 에러가 발생하며 테스트가 실행되지 않았다.

```
cy.readFile("../../.yarn/cache/resq-npm-1.10.1-c0eb904091-57458eb232.zip/node_modules/resq/dist/index.js") failed while trying to read the file at the following path:

/Users/francis/d/IdeaProjects/product/.yarn/cache/resq-npm-1.10.1-c0eb904091-57458eb232.zip/node_modules/resq/dist/index.js

The following error occurred:

"ENOTDIR: not a directory, open '..../.yarn/cache/resq-npm-1.10.1-c0eb904091-57458eb232.zip/node_modules/resq/dist/index.js'"
```

이 문제의 정확한 원인은 아직 모르겠지만, 이 이슈를 해결한 사람은 [`cypress`의 오류라고 생각한다고 한다.](https://github.com/abhinaba-ghosh/cypress-react-selector/issues/230)

## 해결 방법

`2.3.15` 이하 버전의 `cypress-react-selector`를 사용한다면, 다음과 같은 커맨드로 `resq` 패키지를 언플러그 시켜주면 된다.

```
yarn unplug resq
```

`2.3.15` 이상 버전의 `cypress-react-selector`를 사용하고 있다면, `yarn unplug resq` 를 사용해도 작동하지 않는다고 한다.

다행히도 다음 코드를 이용하여 패키지를 찾고 `cy.waitForReact()` 에 이름을 지정할 수 있습니다.

```js
// cypress/plugins/index.js

module.exports = (on, config) => {
  on('task', {
    getResqFilePath() {
      const pathName = path.join(process.cwd(), '../../.yarn/unplugged');
      const files = fs.readdirSync(pathName);
      const resqFiles = files.filter(f => f.includes('resq-npm-'));
      if (resqFiles.length != 1) {
        throw new Error(
          `Cannot find the resq module in the unplugged ${pathName} - make sure the 'resq' package is unplugged`,
        );
      }
      const resqFile = path.join(
        pathName,
        resqFiles[0],
        'node_modules/resq/dist/index.js',
      );
      return resqFile;
    },
  });
};
```

그리고 `cy.waitForReact()` 를 써야 할때 다음 코드를 사용하면 된다.

```js
cy.task('getResqFilePath').then((resqPath: string) => {
  cy.waitForReact(1000, undefined, resqPath);
});
```
