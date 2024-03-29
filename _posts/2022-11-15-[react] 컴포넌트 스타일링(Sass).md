---
categories: react
---

# 가장 흔한 방식, 일반 CSS

CSS 클래스를 중복되지 않게 작성

이름을 지을 때 특별한 규칙을 사용, CSS Selector를 활용

---

## 이름 짓는 규칙

---

App-header 와 같이 “컴포넌트 이름-클래스”

비슷한 방식으로 BEM 네이밍 방식은 CSS 방법론 중 하나로,

이름을 지을 떄 일종의 규칙을 준수하여 해당 클래스가 어디에서 어떤 용도로 사용되는지 명확하게 작성 예를 들면 .card\_\_title-primary

---

## CSS Selector

---

CSS 클래스가 특정 클래스 내부에 있는 경우에만 사용가능

```jsx
// App.css
/*.App 안에 들어 있는 .logo*/
.App .logo {
	animation: App-logo-spin infinite 20s linear;
	height: 40vmin;
}

/* .App 안에 들어 있는 a 태그 */
.App a {
color: #61dafb;
}

// App.js
<div className="App">
	<header>
		<img src={logo} className="logo" alt="logo" />
			<p>
			Edit <code>src/App.js</code> and save to reload.
			</p>
			<a
			href="https://reactjs.org"
			target="_blank"
			rel="noopener noreferrer"
			>Learn React
		</a>
	</header>
</div>
```

# Sass 사용하기

Sass (Syntactically Awesome Style Sheets)(=문법적으로 매우 멋진 스타일시트)

create-react-app 구버전에는 추가 작업이 필요했지만, v2 부터는 설정 없이 바로 사용 가능

Sass는 .scss 와 .sass를 지원

```css
//.sass
$font-stack: Helvetica, sans-serif
$primary-color : #333

body
	font: 100% $font-stack
	color: $primary-color

// .scss
$font-stack: Helvetica, sans-serif;
$primary-color : #333;

body {
	font: 100% $font-stack;
	color: $primary-color;
}
```

주요 차이점으로는 .sass 확장자는 중괄호({})와 세미콜론(;)을 사용하지 않음

Sass를 사용하기 위해서는

```css
// node-sass 라는 라이브러리 설치
$yarn add node-sass
```

```css
// SassComponent.scss

// 변수 사용하기
$red: #fa5252;
$orange: #fd7e14;
$yellow: #fcc419;
$green: #40c057;
$blue: #339af0;
$indigo: #5c7cfa;
$violet: #7950f2;

// 믹스인 만들기(재사용되는 스타일 블록을 함수처럼 사용할 수있음)
@mixin square($size) {
  $calculated: 32px * $size;
  width: $calculated;
  height: $calculated;
}

.SassComponent {
  display: flex;

  .box {
    // 일반 CSS에서는 .SassComponent .box와 마찬가지
    background: red;
    cursor: pointer;
    transition: all 0.3s ease-in;

    &.red {
      // .red 클래스가 .box와 함께 사용되었을 때
      background: $red;
      @include square(1);
    }

    &.orange {
      background: $orange;
      @include square(2);
    }

    &.yellow {
      background: $yellow;
      @include square(3);
    }

    &.green {
      background: $green;
      @include square(4);
    }

    &.blue {
      background: $blue;
      @include square(5);
    }

    &.indigo {
      background: $indigo;
      @include square(6);
    }

    &.violet {
      background: $violet;
      @include square(7);
    }

    &:hover {
      // .box에 마우스를 올렸을 때
      background: black;
    }
  }
}
```

```jsx
import React from "react";
import "./SassComponent.scss";

const SassComponent = () => {
  return (
    <div className="SassComponent">
      <div className="box red" />
      <div className="box orange" />
      <div className="box yellow" />
      <div className="box green" />
      <div className="box blue" />
      <div className="box indigo" />
      <div className="box violet" />
    </div>
  );
};
export default SassComponent;
```

---

## utils 함수 분리

---

여러 파일에서 사용될 수 있는 Sass 변수 및 믹스인을

다른 파일로 따로 분리하여 작성후 불러와 사용

src 디렉터리에 styles라는 디렉터리를 생성 후

그 안에 utils.css파일을 생성 그 안에 변수와 믹스인을 넣어 주고 불러와서 사용

```jsx
@import './styles/utils';
```

---

## sass-loader 설정 커스터마이징

---

필수 사항은 아니지만, 해두면 유용

구조가 깊어졌을 때

```jsx
@import '../../../../../../styles/utils';
```

이 문제점을 웹팩에서 Sass를 처리하는 sass-loader의 설정을 커스터마이징하여 해결 할 수 있음

커스터마이징 하기 위해서는 프로젝트 디렉터리에서

```powershell
$yarn eject
```

명령어를 통해 세부 설정을 밖으로 꺼내주어야 한다.

create-react-app은 기본적으로 Git설정이 되어있으므로

반드시 Git 커밋 이 후 진행

```jsx
//webpack.config.js -sassRegex 찾기

{
  test: sassRegex,
    exclude: sassModuleRegex,
      use: getStyleLoaders({
        importLoaders: 2,
        sourceMap: isEnvProduction && shouldUseSourceMap
      }).concat({
        loader: require.resolve('sass-loader'),
        options: {
          includePaths: [paths.appSrc + '/styles'],
          sourceMap: isEnvProduction && shouldUseSourceMap,
        }
      }),
        sideEffects: true
},
```

설정 후 서버를 재시작 하면,

```css
// 복잡
@import "../../../../../../styles/utils";

// 간단하게 사용가능
@import "utils.scss";
```

---

## node_modules에서 라이브러리 불러오기

---

```css
// 복잡
@import "../../../node_modules/library/styles";

// 간단하게 사용가능
@import "~library/styles";
```

yarn을 통해 설치한 라이브러리를 사용하려면 node_modules까지 들어가서 불러와야 하는데

~ 문자를 사용하면 자동으로 node_modules에서 라이브러리 디렉터리를 탐지하여 스타일을 불러올 수 있음

### 라이브러리 사용

```css
$ yarn add open-color include-media

// utils.scss
@import '~include-media/dist/include-media';
@import '~open-color/open-color';

// SassComponent.scss
.SassComponent {
	display: flex;
	background: $oc-gray-2;
	@include media('<768px') {
		background: $oc-gray-9;
	}
	(...)
}
```

open-colors 팔레트 라이브러리에서 불러온 후 설정

화면 가로 크기가 768px 미만이 되면 배경색을 어둡게 설정
