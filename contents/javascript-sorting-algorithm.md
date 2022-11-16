---
date: '2022-11-16'
title: 'js의 sort()는 어떤 알고리즘을 사용할까'
categories: ['Javascript', 'Algorithm', 'sort']
summary: 'javascript의 sort() 메서드는 어떤 알고리즘으로 동작하는지, 그리고 tim sort가 무엇인지 알아보자'
---

## Array.prototype.sort()

js에서 배열의 요소를 정렬하고 싶을 때 우리는 `sort()` 라는 메서드를 사용한다. `sort()` 는 파라미터로 정렬 함수를 받는데 제공되지 않으면 기본적으로 문자열로 변환하여 오름차순 정렬된다.

오름차순 기준으로 첫 번째 요소가 두 번째 요소보다 우선순위가 높으면 음수, 낮으면 양수, 같을 경우 0을 반환해주는 식으로 정렬함수르 작성하면 된다.

## Javascript에서 사용하는 정렬 알고리즘

Javascript 명세에는 `sort` 구현에 어떤 알고리즘을 사용해야 한다고 따로 특정하지 않는다. 이 때문에 구현체에 따라 정렬에 사용되는 알고리즘이 달라 질 수 있다.

가장 많이 쓰이는 V8엔진에서는 아래의 설명과 함께 tim sort라는 정렬 방법을 사용한다.

```md
// This file implements a stable, adapative merge sort variant called TimSort.
```

> 파이어폭스의 js 엔진인 spidermonkey는 merge sort를 사용한다.

## Tim sort

이 정렬 알고리즘은 'Insertion Sort'과 'Merge Sort'를 결합하여 만든 정렬이라고 한다. merge sort를 기반으로 엄청나게 최적화를 한 알고리즘이다. 시간복잡도 O(n\log n)O(nlogn), stable, in-place 정렬이다.

O(n\log n)O(nlogn) 정렬 알고리즘의 단점을 최대한 극복한 알고리즘으로, Java와 Python 등 에서도 사용된다.
