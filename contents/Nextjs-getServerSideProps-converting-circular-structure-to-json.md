---
date: '2022-06-11'
title: 'Nextjs getServerSideProps에서 concircular structure 에러가 발생할때'
categories: ['Nextjs', 'getServerSideProps', 'Error']
summary: '후배들이 하는 프로젝트 hello gsm이 개발 도중에 Nextjs getServerSideProps에서 TypeError: Converting circular structure to JSON 에러가 발생했다.'
---

## circular structure error?

`circular structure error` 는 순환 참조 객체를 직렬화하려 할때 주로 발생하는 오류이다.

```js
var a = {};
a.b = a;
```

위와 같은 객체를 순환 참조 객체라고 하는데, 직렬화를 할때 주로 사용하는 `JSON.stringify`로는 이런 객체를 직렬화 할 수 없다.

이 상황에서 발생하는 오류가 바로 `TypeError: Converting circular structure to JSON` 이다.

[참고 자료](https://stackoverflow.com/questions/4816099/chrome-sendrequest-error-typeerror-converting-circular-structure-to-json)

## 에러 발생 이유

`getServerSideProps` 의 자세한 설명은 [공식문서](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props)를 참조하자

`getServerSideProps` 는 props를 반환해주는데, 반환해줄때 "직렬화"를 하여 반환해준다. 즉, 순환 참조 객체를 반환할경우 사이트 서버에서 오류가 발생해버린다.

![TypeError: Converting circular structure to JSON 에러 참조 사진](https://cdn.discordapp.com/attachments/942789685216944158/985133904258277406/2022-06-11_7.40.53.png)

왜 순환 참조를 반환하게 되었을까?

그 이유는 `axios` 에 있다.

후배의 코드를 살펴보면 다음과 같이 응답 data를 반환해주었다.

![후배 코드 사진](https://cdn.discordapp.com/attachments/942789685216944158/985142547867385896/2022-06-11_8.24.43.png)

`axios` 의 `response` 데이터를 바로 반환하는 로직인데, 문제는 바로 이곳이다.

`axios` 의 [응답](https://www.npmjs.com/package/axios#response-schema) 속성을 살펴보자

```js
// `data` is the response that was provided by the server
data: {..your real json response..},
// `status` is the HTTP status code from the server response
status: 200,
// `statusText` is the HTTP status message from the server response
statusText: 'OK',
// `headers` the HTTP headers that the server responded with
headers: {},
// `config` is the config that was provided to `axios` for the request
config: {},
// `request` is the request that generated this response
request: {}
```

json으로 변환할 전체 `axios` 응답 객체를 보내는 경우 실제 `response`의 일부 속성은 상태, 데이터 구성 등과 같은 개인 axios 속성의 이름을 순환하여 참조 가능하다.

그렇기 때문에 `axios` 의 `response` 데이터는 순환 참조 객체가 될 수 있다.

## 에러 해결

해결 방법은 아주 간단한데, 아래와 같이 원하는 `reponse` 에서 원하는 데이터만 쏙쏙 골라서 반환해주면 된다.

```js
const { data } = await axios.get('https://...com');
```
