---
date: '2021-12-07'
title: 'Svelte 프로젝트에 도입하기'
categories: ['Svelte', 'Scss', 'Typescript', 'Tailwindcss']
summary: '친구에게 갈비탕값을 하기 위해서 급식 메뉴 투표 결과를 보여주는 프로젝트를 만들게 되었다.'
---

친구에게 갈비탕값을 하기 위해서 급식 메뉴 투표 결과를 보여주는 프로젝트를 만들게 되었다.
평소에 사용하는 `React` 나 `Next` 를 쓸 예정이었지만, 기능이 되게 단순하기도 했고, 개발을 빨리 해야되는 상황에서 ( 3일안에 완성 ) 새로운 기술을 도입한다는건 어찌보면 미친짓이지만 이번이 아니면 학생때 하는 프로젝트에서 다시는 `svelte` 를 경험하지 못할 것 같아서 이번 기회에 `svelte`를 한번 써보게 되었다.

## 프로젝트 세팅

우선 일분일초가 아까운 세상이라 퍼블리싱에 쏟는 시간을 최소화 하여야 하기 때문에 (디자인도 해야 했다.) `css-framework` 인 `tailwindcss`를 선택하였고, `tailwindcss`로 커버되지 못하는 부분은 `scss`를 이용해 커버하도록 했다. 마지막으로 사랑하는 `yarn-berry` 로 패키지 매니저를 세팅하였다.

혹시 나중에 또 `svelte` 를 쓰게 될지 몰라 [템플릿](https://github.com/sunwoo0706/Svelte_Template)으로 만들어두었다.

## svelte 공부

`svelte` 의 개념 같은것은 일전에 공부해뒀기 때문에, 바로 공부를 시작하였다.
일단 `svelte` 는 굉장히 쉽다. `React` 와는 조금 많이 다르지만 [공식문서](https://svelte.dev)가 굉장히 잘 되어 있고, `repl` 로 코드를 직접 적으면서 바로바로 스킬을 익히기도 쉬웠다. 그리고 공식문서가 죄다 영어지만 그래도 충분히 읽을 수 있는 수준이다.

## 프로젝트 개발

최근에 리디북스에서 개발하는 방법이 인상깊어서, 재사용되는 UI들이 최대한 많도록 디자인하여 ( 요소들이 많지도 않지만.. 그래도 한번.. ) `atomic design pattern` 을 쓰면 좋을것 같다고 판단해서 `atoms/`, `blocks/`, `templates/`, `pages/` 로 커스터 마이징하여 써보았다. ( 제대로 쓴게 맞는지는 약간 의심이 들지만.. )

최대한 `svelte`의 `api` 들을 써보려고 노력했다.

UX를 고려해서 `skeleton ui` 를 구현해볼때 `svelte`의 `await` 블럭을 이용해서 아주 간단하게 구현하였다.

![](https://images.velog.io/images/sunwoo0706/post/15b226e0-b2f9-4230-812a-881d455e355c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-12-03%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.25.02.png)

> ```svelte
> {#await getMealData()}
>   <SkeletonBlock />
> {:then mealDataList}
>   ...
> {:catch err}
>   <ErrorCard />
> {/await}
> ```

위와 같이 `promise` 함수를 실행하고 `pending` 중일때는 `Skeleton ui` 를, `fulfilled` 되었을때는 데이터를 보여주고, `reject` 되었을 경우에는 에러카드를 보여주는 로직을 정말 간단하게 구현이 가능했다.

그리고 약간의 조미료로 급식 리스트 뷰 옆 `paragraph` 에 `scss` 로 `fade in left` 애니메이션을 추가하였다.

그리고 설정 파일들이 너무 많아서 `github`에 `used`가 `js`로 도배되어있는것을 보고 싶지 않아 벤더시켰다.

배포의 경우에는 `vercel`과 `netlify` 중 선택 할 수 있지만 놀랍게도 우리 학교는 vercel이 막혀서 ( next 공식 문서도 안 들어가진다. ) `netlify` 로 배포했다.

> 결과물 : https://golabab-client.netlify.app > <small>학교 와이파이에서만 접속되고, 아직 서버는 배포가 안됐지만 맛뵈기로 보여드리는겁니다 😊</small>

## 무엇을 얻었나

흑.. `svelte` 다시 써보고 싶다. 도구가 굉장히 잘 만들어졌다는게 느껴지고, 속도가 빠른지는 체감하지 못했지만, 개발 속도는 정말 빨랐다. ( `tailwindcss` 써서 그럴지도 ) 아무튼 아주 좋은 경험이 되었고, 정해진 데드라인에 맞춰 개발을 끝맞춰서 기분이 좋아졌다. 앞으로 가벼운 프로젝트에는 `svelte` 를 가끔 사용하게 될 것 같다!!

**_더 열심히 공부해야지 🚀_**
