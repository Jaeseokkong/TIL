# 📌 useEffect 사용 기준 정리

React 개발 실무에서 `useEffect`는 자주 사용되지만 **잘못 사용되는 경우가 훨씬 많습니다.** 이번 TIL에서는 "사용해야 할 때"와 "사용하면 안 되는 때"를 명확히 구분해서 정리합니다.

---

## 1️⃣ useEffect를 사용하면 안 되는 경우

### ❌ 값 계산을 위해 사용하는 경우

```tsx
useEffect(() => {
	setTotal(price * count);
}, [price, count]);
```

- 렌더 → effect → 렌더 재실행 → 비효율

⭕ 대안: `useMemo`

```tsx
const total = useMemo(() => price * count, [price, count]);
```

---

### ❌ props → state 동기화

```tsx
useEffect(() => {
	setUser(props.user);
}, [props.user]);
```

- state 중복 보관 → 버그 발생률 증가
- props를 직접 사용하면 해결

⭕ 대안: props 직접 사용 또는 derive 값은 `useMemo`

---

### ❌ API 요청을 useEffectfh 처리해야 한다고 생각할 때

```tsx
useEffect(() => {
	fetch('/api/todos').then(...);
}, [])
```

- 렌더 후 실행되기 때문에 지연 발생
- 캐싱·중복요청 방지·에러 핸들링도 직접 구현해야 함

⭕ 대안: RTK Query, React Query 사용

---

### ❌ 이벤트 핸들러가 최신 값을 참조하기 위해 effect 사용

```tsx
useEffect(() => {
	window.addEventListener('scroll', () => console.log(count));
}, [count]);
```

- 매 렌더마다 이벤트 다시 등록됨 → 비효율

⭕ 대안: useEvent(React 18+), useCallback

---

### ❌ 비즈니스 로직 실행을 useEffect에 넣는 경우

useEffect는 **부수효과**를 위한 훅이지 로직 실행기가 아님

---

## 2️⃣ useEffect를 사용해야 하는 경우

### 🔹 외부 시스템 구독 / 연결 관리

- WebSocket 연결·해제
- 이벤트 리스너 등록·해제
- observer, third-parth library mount/unmount

```tsx
useEffect(() => {
	socket.connect();
	socket.on('data', onData);
	return () => {
		socket.off('data', onData);
		socket.disconnect();
	};
}, []);
```

--- 

### 🔹 DOM 수동 조작

React가 담당하지 않는 브라우저 API 사용 시

```tsx
useEffct(() => {
	inputRef.current.focus():
}, [])
```

---

### 🔹 타이머/인터벌 관련 + 정리(cleanup) 필요할 때

```tsx
useEffect(() => {
	const id = setInterval(() => tick(), 1000);
	return () => clearInterval(id);
}, [])
```

---

### 🔹 외부 비동기 작업 + 중단 로직 필요할 때

```tsx
useEffect(() => {
	const controller = new AbortController();

	fetch('api/data', { signal: controller.signal });

	return () => controller.abort();
}, [])
```

---

## 3️⃣ 요약 정리

- **React** 내부 상태 계산 → useEffect ❌
- **React** 외부 세계와 동기화할 때 → useEffect ⭕

useEffect는 "렌더 이후 동작"을 관리하는 훅이지, 로직 또는 데이터를 처리하는 훅이 아닙니다.

---

## 4️⃣ 실무 체크리스트

✔ state 계산하려는 건가? → useMemo로 바꿔보기

✔ props를 다른 state에 복제하려는 건가? → 복제하지 말기

✔ API 요청을 넣으려는가? → RTK Query/React Query가 맞음

✔ eventListener/WebSocket/canvas/DOM 조작인가? → useEffect가 맞음

✔ cleanup이 필요한 작업인가? → useEffect가 맞음

---