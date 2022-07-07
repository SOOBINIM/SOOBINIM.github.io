![](https://velog.velcdn.com/images/lsoob/post/7dd372f9-a5d2-4441-952f-14c1973fd69b/image.png)

> 리액트 axios로 외부 API get 하려고 하니,
> CORS 정책으로 데이터를 가져올 수 없었다.

# 해결방법 2개

- proxy 설정
- http-proxy-middleware setupProxy.js

---

## 1. package.json 설정

```javascript
"proxy": "api 주소"
// https://api.com
```

```javascript
axios.get("api 주소 이후 경로");
// /api/1 작성
// https://api.com/api/1
```

## 2. http-proxy-middleware

yarn add http-proxy-middleware 설치 후
src/setupProxy.js 에 작성

```javascript
const { createProxyMiddleware } = require("http-proxy-middleware");

module.exports = (app) => {
  app.use(
    "api 주소를 구분할 경로",
    // /api
    createProxyMiddleware({
      target: "api 주소",
      changeOrigin: true,
    })
  );
};
```

```javascript
axios.get("api 주소 이후 경로");
// /api/1 작성
// setupProxy.js에서 app.use뒤
// api라고 작성한 값 가져옴
```
