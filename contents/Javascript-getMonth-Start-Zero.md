---
date: '2004-07-06'
title: 'Javascript의 Month은 왜 0부터 시작할까?'
categories: ['Javascript', 'getMonth']
summary: 'Javascript getMonth를 사용하면 Date 객체의 월 값을 현지 시간에 맞춰 반환한다. 그리고 인덱스가 1씩 밀려있다. 이유가 무엇인지 알아보자'
---

## Javascript의 Month은 왜 0부터 시작할까??

javascript는 Date의 월 인수가 0부터 시작해서 11로 끝난다.
이렇게 말하면, "index 같이 그런거 아니야? 문제는 없어보이는데" 라고 말하는 사람이 있을 수 있다.
하지만 일 인수는 1부터 시작헤서 31 사이의 숫자이다. 왜일까?

이 문제는 javascript의 Date가 Java의 JDK1.0 (1995)의 `java.util.Date`를 카피하였기 때문에 발생하는 일이다.

[자바스크립트를 창시한 브렌든 아이크의 트윗](https://twitter.com/BrendanEich/status/481939099138654209)
