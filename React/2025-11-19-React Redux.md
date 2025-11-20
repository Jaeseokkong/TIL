# 🚀 React-Redux

React에서 Redux를 직접 사용하려면 `store.subscribe()`로 UI를 직접 갱신해야 합니다.  
하지만, React 앱에서는 이 방식이 비효율적이라, **React-Redux** 라이브러리를 사용해 React 컴포넌트외ㅏ Redux store를 부드럽게 연결합니다.

---

## 1️⃣ 핵심 역할

React-Redux는 다음 두 가지를 해결해주는 라이브러리입니다.

### 🔹 컴포넌트 변화 감지

컴포넌트가 Redux store 변ㅁ화를 효율적으로 감지하도록 도와줍니다.

- 특정 state 조각이 바뀌면 그 state를 사용하는 컴포넌트만 리렌더링 (전체 앱 리렌터링 ❌)

### 🔹 전역 dispatch 가능

React 트리 어디에서든 dispatch를 가능하게 합니다.

- props drilling 없이 어떤 컴포넌트에서도 store에 접근 가능

---

## 2️⃣ Provider

React 앱에서 Redux를 사용하려면 **Provider로 컴포넌트를 감싸 store를 전달**해야 합니다

```tsx
import { Provider } from "react-redux";
import { store } from "./store";

export function App() {
  return (
    <Provider store={store}>
      <RootComponent />
    </Provider>
  );
}
```

### 🔍 내부 동작

- React Context를 사용해 store를 하위 트리에 주입
- useSelector, useDispatch는 모두 이 Context를 사용해 store에 접근
- Context 값이 변경되는 대신, 개별 컴포넌트가 store 변경을 직접 구독해서 불필요한 렌더링 방지

---

## 3️⃣ Redux Hooks

React-Redux에서 가장 많이 사용하는 Hook은 다음 두가지입니다.

### 🔹 `useSelector()`

React store의 상태 중 **일부만 선택해서 가져오는 Hook**입니다.

```js
const count = useSelector((state) => state.counter.value);
```

#### 📌 특징

- 필요한 state 일부만 선택 → 성능 최적화 
- 선택한 값이 바뀌지 않으면 해당 컴포넌트는 리렌더링되지 않음

#### 🔍 내부 동작

- 컴포넌트가 useSelector 실행 → store.subscribe 등록
- state 변경 → selector 실행 → 값 변경 여부 비교
- 바뀌었으면 해당 컴포넌트만 렌더링

---

### 🔹 `useDispatch()`

Redux의 dispatch 함수를 가져오는 Hook입니다.

```js
const dispatch = useDispatch();

dispatch({ type: "INCREMENT" });
```

#### 📌 특징

- 함수 자체는 절대 새로 생성되지 않음 → 안전하게 useCallback 없이 사용 가능
- 이벤트 핸들러로 주로 사용

---

## 4️⃣ connect() vs Hooks

React-Redux는 원래 Hook이 없던 시절에는 `connect()` HOC를 사용해서 Redux와 연결했습니다.

### 🔹 예: connect() 사용 방식 (옛날 방식)

```js
import { connect } from "react-redux";

function Counter({ value, onIncrement }) {
  return (
    <div>
      <p>{value}</p>
      <button onClick={onIncrement}>+</button>
    </div>
  );
}

const mapStateToProps = (state) => ({
  value: state.counter.value,
});

const mapDispatchToProps = (dispatch) => ({
  onIncrement: () => dispatch({ type: "INCREMENT" }),
});

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

#### ⚠️ 단점

- 코드량 많음
- 가독성이 떨어짐
- 로직이 컴포넌트 외부로 분산됨

#### ✅`useSelector`, `useDispatch` 공식 추천

- 함수형 컴포넌트와 가장 자연스럽게 통합
- 코드가 깔끔
- TS 친화적
- 필요한 부분만 구독 가능 → 성능 좋음

---