---
date: '2022-10-12'
title: 'PWA Nextjs 프로젝트에 도입하기'
categories: ['Nextjs', 'PWA', 'PWAbuilder']
summary: 'PWAbuilder를 사용하여 Nextjs 프로젝트를 pwa로 만들어보자.'
---

## PWA는 무엇인가

PWA 는 Google I/O 2016 에서 소개된 개념으로 모바일 웹 앱을 네이티브 앱스럽게 만들고자하는 방법론이다. 이 말만 들어서는 하이브리드 앱과 똑같은거 아니냐? 라고 할 수 있다.

하지만 PWA와 하이브리드앱은 차이점이 있는데 PWA는 기존의 기술들을 활용하여 모바일 웹 앱을 네이티브 앱처럼 사용할 수 있도록하고, 반면 하이브리드 앱은 모바일 웹 앱 기술로 앱을 개발하고, 프레임워크를 사용해서 네이티브 앱으로 패키징을 해주어야한다는 점이다.

MDN 에서는 PWA 는 새로운 기술이 아닌 웹 앱을 개발하는 새로운 **방법론**이라는 것을 여러 차례에 걸쳐 말하고 있다.

한줄로 "기존에 존재하던 기술로 웹앱을 네이티브앱처럼 개발하는 방법" 이라 설명할 수 있다.

## 그럼 만들어보자

PWA를 만들기 위해선 다음과 같은 준비물이 필요하다.

1. 반응형 웹 (추가하지 않아도 만들수있지만 앱으로서 사용할 수 없으니 반응형은 필수다)
2. HTTPS 사용 (서비스 워커와 같은 핵심 pwa 기능에는 https 프로토콜이 꼭 필요하다)
3. manifest.json 정보
4. 서비스 워커 (Nextjs에서는 next-pwa라는 라이브러리를 보편적으로 이용한다)

여러 툴이 있지만 이 글에서는 [pwabuilder.com](https://www.pwabuilder.com/)을 이용한다

사이트를 들어가보면 수달 또는 해달이 해맑게 환영해준다. 아래와 같이 URL을 입력하는 폼에 준비물 1, 2번에 만족하는 사이트 URL을 입력하고 start 버튼을 눌러주면 PWA 시작이다.

![PWAbuilder 사이트 입력 위치 표시 사진](https://cdn.discordapp.com/attachments/899521631569977375/1030000298548277258/2022-10-13_3.10.39.png)

대부분의 경우 manifest.json 파일이 없기때문에 manifest 파일을 찾지 못하였다고 뜨게 된다. (본인의 경우 이미 작성하였기 때문에 정보가 뜬다.)

![pwabuilder 메인 화면](https://cdn.discordapp.com/attachments/899521631569977375/1030004465312473149/2022-10-13_3.29.50.png)

### Manifest

Edit Your Manifest 버튼을 클릭하면 manifest.json을 생성할 수 있다.

![manifest.json 설정 탭 화면](https://cdn.discordapp.com/attachments/899521631569977375/1030005512814743603/2022-10-13_3.34.01.png)

탭을 이동하며 필수 요소만 작성해도 PWA를 만들기에는 충분하다. 본인은 setting 부분의 language 부분만 korean으로 선택설정하였다. 다른 옵션들도 설명을 보며 본인 마음대로 수정하면 된다.

screenshot 옵션은 스토어 등록 정보로 최대 8개의 페이지 스크린샷을 추가 할 수 있다. 넣고 싶은 페이지의 경로만 작성하여 generate 버튼을 클릭하면 알아서 data:image 형식으로 데스크톱, 모바일 스크린샷 이미지 데이터를 manifest에 추가해준다.

여기까지 진행되었다면 download manifest 버튼을 클릭하여 manifest.json 파일을 다운로드 받고, 프로젝트의 public 폴더 안으로 이동시켜주면 된다.

icon은 앱의 로고를 업로드 해주면 된다. icon의 경우 사용자가 직접 manifest.json 파일을 수정해줘야 하는데 우선 이미지 업로드를 하면 여러 사이즈와 플랫폼에 대응하는 icon 파일들이 자동으로 생성된다. zip file로 다운로드 받고 압축을 풀면 아래와 같은 폴더가 생성된다.

![PWAbuilder icon zip file folder](https://cdn.discordapp.com/attachments/899521631569977375/1030009107429724230/2022-10-13_3.48.20.png)

위의 폴더를 Nextjs 프로젝트의 public 폴더로 그대로 이동시켜주자 그리고 icons.json 파일을 열어 안에있는 코드를 복사하고 삭제한후 manifest.json 파일의 icons 옵션 부분을 복사한 코드로 바꿔준다. 이 후 이미지 경로를 알맞게 수정해주면 manifest.json 파일은 완성된다.

완성했다면 manifest.json을 사이트에 적용시켜줘야 한다. Nextjs 프로젝트에 \_document 파일에 head 태그 안에 아래 코드를 작성하면 manifest.json 파일이 적용된다.

```jsx
<link rel="manifest" href="/manifest.json" />
```

> [vscode extension](https://marketplace.visualstudio.com/items?itemName=PWABuilder.pwa-studio)으로도 PWA Manifest.json을 생성할 수 있다.

### service worker

여기서 주의할점이 있다. 본인은 이 부분을 개발할때 nextjs의 example 코드를 참고하고 진행하였지만 왜인지 작동하지 않아 이유를 찾아보니 다른 블로그와 nextjs example 코드 전부 옛버전의 next-pwa 설정 방법을 사용하고 있었다. 이 블로그의 방법 (5.6.0) 도 추후 레거시가 될 수 있으니 [next-pwa](https://www.npmjs.com/package/next-pwa) 그때그때 사용법을 알아보자.

이제 nextjs의 pwa example 코드는 이 몸이 최신 버전의 next-pwa를 사용하도록 했으니 걱정말라구 !

https://github.com/vercel/next.js/pull/41386

사용하는 패키지 매니저에 알맞게 Nextjs 프로젝트에 [next-pwa](https://www.npmjs.com/package/next-pwa) 패키지를 설치해준다.

next.config.js 파일에 아래와 같이 작성해준다.

```js
/** @type {import('next').NextConfig} */
const withPWA = require('next-pwa')({
  dest: 'public',
});

module.exports = withPWA({
  // config
});
```

기존에 사용하고 있던 config 옵션들이 있다면 `// config` 부분에 삽입해주면 된다.

dev server에서도 서비스 워커를 디버깅할 필요가 없다면 아래 옵션을 추가해주면 된다.

```js
/** @type {import('next').NextConfig} */
const withPWA = require('next-pwa')({
  dest: 'public',
  disable: process.env.NODE_ENV === 'development',
});

module.exports = withPWA({
  // config
});
```

배포 후 사이트를 들어가게 되면 아래처럼 URL input 우측에 새로운 아이콘이 생성된것을 확인할 수 있다.

![pwa 다운로드 버튼](https://cdn.discordapp.com/attachments/896396380862554154/1030362531912491028/2022-10-14_3.12.06.png)

다운로드를 받아 정상 작동하는지 확인해보자

정상 작동하면 완성이다 ! 🎉

다른 플랫폼의 패키지가 필요하다면 아래의 pwabuilder의 package for stores 버튼을 클릭하여 생성해주면 된다.

![pwabuilder package for stores button position](https://cdn.discordapp.com/attachments/896396380862554154/1030364653525676082/2022-10-14_3.20.47.png)
