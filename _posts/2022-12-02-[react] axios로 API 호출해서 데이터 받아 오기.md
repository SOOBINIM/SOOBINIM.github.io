---
categories: react
---

# axios로 API 호출해서 데이터 받아 오기

```powershell
$yarn add axios
```

자바스크립트 HTTP 클라이언트

axios의 특징으로는 HTTP 요청을 Promise 기반으로 처리

```jsx
import { useState } from "react";
import axios from "axios";

const App = () => {
  const [data, setData] = useState(null);
  const onClick = () => {
    axios.get("https://jsonplaceholder.typicode.com/todos/1").then((res) => {
      setData(res.data);
    });
  };
  return (
    <div>
      <div>
        <button onClick={onClick}>불러오기</button>
      </div>
      {data && (
        <textarea
          row={7}
          value={JSON.stringify(data, null, 2)}
          readOnly={true}
        />
      )}
    </div>
  );
};

export default App;
```

가짜 API를 호출하고 이에 대한 응답을 컴포넌트 상태에 넣어서 보여주는 예제

onClick 함수에서 axios.get 함수를 사용

이 함수는 파라미터로 전달된 주소에 GET 요청

결과를 .then을 통해 비동기적으로 확인

```jsx
import { useState } from "react";
import axios from "axios";

const App = () => {
  const [data, setData] = useState(null);
  const onClick = async () => {
    try {
      const response = await axios.get(
        "https://jsonplaceholder.typicode.com/todos/1"
      );
      setData(response.data);
    } catch (e) {
      console.log(e);
    }
  };
  return (
    <div>
      <div>
        <button onClick={onClick}>불러오기</button>
      </div>
      {data && (
        <textarea
          rows={70}
          value={JSON.stringify(data, null, 2)}
          readOnly={true}
        />
      )}
    </div>
  );
};
export default App;
```

async를 적용

화살표 함수에 async/await를 적용할 때는 async () ⇒ {}와 같은 형식으로 적용

# newsapi API 키 발급받기

[South Korea News API - Live top headlines from South Korea](https://newsapi.org/s/south-korea-news-api)

뉴스 API 신청후 API 발급 받기

## 전체 뉴스 불러오기

[https://newsapi.org/v2/top-headlines?country=kr&apiKey=](https://newsapi.org/v2/top-headlines?country=kr&apiKey=0a)API키

## 특정 카테고리 뉴스 불러오기

[https://newsapi.org/v2/top-headlines?country=kr&category=](https://newsapi.org/v2/top-headlines?country=kr&category=)business&apiKey=API키

### 카테고리

business, entertainment, health, science, sports, technology
