# Redux Toolkit Thunk - `createAsyncThunk`

Redux Toolkit(RTK)은 Redux의 비동기 흐름을 표중화한 도구이며,  
`createAsyncThunk`는 그중 **비동기 로직을 가장 안정적으로 관리하는 공식 방식**입니다.

---

## 1️⃣ `createAsynThunk`가 필요한 이유

### 🔹 기존 Redux + Thunk의 문제점

- 액션 타입 3개(FETCH_START, FETCH_SUCCESS, FETCH_ERROR) 직접 관리
- 액션 크리에이터 따로, 리유서 따로, 비동기 로직 따로
- 로딩/성공/에러 상태 관리 코드 반복
- 타입스크립트 적용 시 추가 보일러플레이트 증가

👉 RTK의 `createAsyncThunk`는 이 모든 과정을 자동화합니다.

---

## 2️⃣ `createAsyncThunk` 기본 개념

`createAsyncThunk`는 **비동기 요청을 하나의 thunk로 추상화**하고 아래 3가지 "라이프사이클 액션"을 자동 생성합니다.

1. **pending** (요청 시작)
2. **fulfilled** (요청 성공)
3. **rejected** (요청 실패)

### 🧐 예시

```js
export const fetchUser = createAsyncThunk(
	"user/fetchUser",
	async (userId) => {
		const res = await fetch(`/api/users/${userId}`);
		return await res.json();
	}
);
```

**자동 생성되는 액션**

- user/fetchUser/pending
- user/fetchUser/fulfilled
- user/fetchUser/rejected

---
