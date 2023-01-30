---
categories: nextjs
---

# NEXT.JS

<aside>
💡 node.js 버전 10.13 이상 필요

</aside>

### 앱 설치 명령어

```powershell
npx create-next-app@latest nextjs-intro
```

### 앱 실행 명령어

```powershell
npm run dev
```

### 라우팅

pages 디렉터리 안에 파일 만들면 자동으로 경로 설정 가능(패턴)

```jsx
// pages/about.js
export default function soob() {
  return "soob인줄 알았지? about 페이지야";
}

// http://localhost:3000/about
// soob인줄 알았지? about 페이지야
```

컴포넌트의 이름은 중요하지 않다.

중요한건 `export default function` 과 `파일명` 이다.

페이지는 **파일 이름** 을 기반으로 경로와 연결되어 있기 때문이다.

### JSX

이용 쌉 가능

### SSR (Server Side Rendering)

api 호출은 오래걸릴 수 있어도 html 소스는 먼저 볼 수 있다.

서버에서 보여질 HTML을 미리 준비해 클라이언트에게 보여준다.

### hydration

리액트를 프론트엔드 안에서 실행하는 것

next.js는 리액트를 백앤드에서 동작시킨다.

### Head 태그 사용

```jsx
import Head from "next/head";

<Head>
  <title>First Post</title>
  <script src="https://connect.facebook.net/en_US/sdk.js" />
</Head>;
```

### Script 태그 사용

```jsx
import Head from "next/head";
import Script from "next/script";

// 비추!!!
<Head>
  <title>First Post</title>
  <script src="https://connect.facebook.net/en_US/sdk.js" />
</Head>

// 추천!!!
<Script src="https://connect.facebook.net/en_US/sdk.js"
        strategy="lazyOnload"
        onLoad={() =>
	        console.log(`script loaded correctly, window.FB has been populated`)
        }/>
```

특정 스크립트가 렌더링을 차단하고 페이지 콘텐츠 로드를 지연시킬 수 있는 경우

성능에 상당한 영향을 미칠 수 있음 따라서 `Script 태그 사용`
