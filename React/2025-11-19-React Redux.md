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