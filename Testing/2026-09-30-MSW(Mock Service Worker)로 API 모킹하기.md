# 🎭 MSW(Mock Service Worker)로 API 모킹하기

MSW는 네트워크 계층에서 요청을 가로채 **실제 fetch/axios 코드를 그대로 둔 채** 가짜 응답을 돌려주는 모킹 라이브러리입니다.

---

## 1️⃣ 기존 모킹 방식과의 차이

- `jest.mock("axios")`처럼 모듈 자체를 모킹하면, 테스트 코드가 "axios를 쓴다"는 구현 세부사항에 묶임
- MSW는 실제 네트워크 요청을 가로채기 때문에, **어떤 HTTP 클라이언트를 쓰든 동일한 방식으로 모킹** 가능

---

## 2️⃣ 기본 사용법

```js
import { http, HttpResponse } from "msw";
import { setupServer } from "msw/node";

const server = setupServer(
  http.get("/api/user", () => {
    return HttpResponse.json({ id: 1, name: "재석" });
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

- 테스트별로 `server.use(...)`를 호출해 특정 케이스(에러 응답 등)만 다르게 모킹 가능
