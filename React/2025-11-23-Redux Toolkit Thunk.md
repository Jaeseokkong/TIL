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

## 3️⃣ Slice에서 비동기 상태 처리

Redux Toolkit에서는 `extraReducers`로 비동기 상태를 처리합니다.

```js
const userSlice = createSlice({
	name: "user",
	initialState: {
		data: null,
		loading: false,
		error: null,
	},
	extraReducers: (builder) => {
		builder
		.addCase(fetchUser.pending, (state) => {
			state.loading = true;
			state.error = null;
		})
		.addCase(fetchUser.fulfilled, (state, action) => {
			state.loading = false;
			state.data = action.payload;
		})
		.addCase(fetchUser.rejected, (state, action) => {
			state.loading = false;
			state.error = action.error;
		});
	},
});
```

- `pending` 상태

	- 비동기 요청 **시작될 때** 자동 호출
	- `loading`을 `true` 로 변경, 기존 오류 상태 초기화

- `fulfilled` 상태

	- 요청이 **성공했을 때** 실행
	- 서버에서 받아온 데이터를 저장

- `rejected` 상태

	- 요청이 **실패했을 때** 실행
	- 네트워크 에러, 서버 에러 등 다양한 오휴 정보를 `action.error`에서 받을 수 있음

---