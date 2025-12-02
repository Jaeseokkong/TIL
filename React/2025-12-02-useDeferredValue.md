# 🪃 useDeferredValue

`useDeferredValue`는 React 18에서 도입된 훅으로, **값 업데이트의 우선순위를 낮춰 UI 렌더링을 더욱 부드럽게 만드는 역할**을 합니다. 특히 입력값 변경처럼 **고빈도 업데이트**가 발생할 때 유용합니다.

---

## 1️⃣ `useDeferredValue`란?

`useDeferredValue(value)`는 입력된 값을 **즉시 업데이트하지 않고**, React가 여유가 있을 때 업데이트 하도록 "지연된 값"을 반환합니다.

- **사용자가 입력하는 동안 UI의 다른 렌더링 작업이 끊기지 않도록** 해줌
- 고비용 연산이 있는 컴포넌트에 보낼 값을 `deferredValue`로 전달하면, 입력은 즉시 반영되지만 무거운 렌더링은 지연

---

## 2️⃣ 기본 사용 예시

```jsx
import { useDeferredValue } from "react";


function Search() {
	const [query, setQuery] = useState("");
	const deferredQuery = useDeferredValue(query);


	const results = useMemo(() => {
		return heavySearch(deferredQuery);
	}, [deferredQuery]);


	return (
		<>
			<input value={query} onChange={(e) => setQuery(e.target.value)} />
			<SearchResults results={results} />
		</>
	);
}
```

- `query`는 즉시 업데이트돼서 입력 딜레이가 없음
- `heavySearch`는 `deferredQuery` 기반이므로 **입력이 빠르게 바뀌어도 불필요한 렌더링이 줄어듦**

---

## 3️⃣ 언제 사용하면 좋은가?

- **검색 필터링**처럼 고비용 연산이 필요하지만, 입력이 즉시 반영되어야 할 때
- **리스트/테이블 연산**이 무거운 상황
- 즉각적인 UI 반응은 중요하지만, **무거운 작업은 뒤로 미뤄도 되는 경우**

---