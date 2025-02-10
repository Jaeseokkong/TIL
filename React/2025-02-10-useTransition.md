# `useTransition`
**React 18**에서 새롭게 도입된 `useTransition` 훅은 **비동기 작업 처리** 및 **긴 렌더링**을 최적화하여 사용자 경험을 향상시킬 수 있는 기능힙니다. 페이지 로딩이 완료되지 않거나 긴 렌더링이 필요한 작업이 있을 때, 사용자가 기다리는 동안 UI를 적절히 처리하여 성능을 최적화할 수있습니다.

<br>

## 1️⃣ `useTransition` 개념
`useTransition`은 **긴 렌더링**을 효율적으로 처리하고, **UI 응답성**을 높이는데 사용됩니다. React가 렌더링 우선순위를 자동으로 조정하도록 도와주며, 긴 렌더링 작업을 비동기적으로 백그라운드에서 처리할 수 있도록 합니다.

### 🔹 주요 반환값
- `isPending`: 트랜지션 상태가 진행 중인지 여부를 나타내는 불리언 값. `true`이면 작업이 아직 진행 중이라는 뜻입니다.
- `startTransition`: 상태 업데이트를 트랜지션 내에서 실행할 수 있도록 도와주는 함수. 이를 통해 긴 렌더링 작업을 비동기적으로 실행합니다.

<br>

---

<br>

## 2️⃣ `useTransition` 사용법
### 🔹 기본 사용법
```tsx
import React, { useState, useTransition } from "react";

function Example() {
    const [isPending, startTransition] = useTransition();
    const [value, setValue] = useState("");

    const handleChange = (e) => {
        startTransition(() => {
            setValue(e.target.value);
        })
    }

    return (
        <div>
            <input type="text" onChange={handleChange} />
            {isPending ? <span>Updating...</span> : <span>Updated</span>}
        </div>
    )
}
```
✔️ `startTransition`을 사용하여 비동기적으로 상태를 업데이트하고, `isPending`을 통해 상태 업데이트가 진행 중인지 알려줍니다.  
✔️ 긴 렌더링 UI를 차단하지 않도록 트랜지션을 활용해 **UI 응답성을 유지**합니다.

<br>

- - -

<br>

## 3️⃣ `useTransition` 활용 사례
### 🔹 긴 리스트 필터링 최적화
```tsx
import React, { useState, useTransition } from "react";

function FilterList() {
  const [isPending, startTransition] = useTransition();
  const [query, setQuery] = useState("");
  const [filteredItems, setFilteredItems] = useState([]);

  const items = ["Apple", "Banana", "Cherry", "Date", "Elderberry", "Fig", "Grape"];

  const handleFilterChange = (e) => {
    const newQuery = e.target.value;
    setQuery(newQuery);

    startTransition(() => {
      const result = items.filter(item => item.toLowerCase().includes(newQuery.toLowerCase()));
      setFilteredItems(result);
    });
  };

  return (
    <div>
      <input type="text" value={query} onChange={handleFilterChange} />
      {isPending ? <span>Loading...</span> : <ul>{filteredItems.map(item => <li key={item}>{item}</li>)}</ul>}
    </div>
  );
}
```
✔️ 긴 리스트나 복잡한 연산을 처리할 때, 트랜지션을 사용하여 **UI를 차단하지 않고 비동기적으로 작업**을 처리합니다.

## 4️⃣ useTransition과 함께 사용할 수 있는 최적화 기법
`useTransition`훅 과 `React.lazy()` + `Suspense`를 함께 사용하면, **컴포넌트를 비동기적으로 로딩**하면서도 **UI가 차단되지 않고 원활하게 동작**하게 할 수 있습니다.

### 🔹 사용 예시
```tsx
import React, { useState, useTransition } from "react";

const LazyComponent = React.lazy(() => import("./LazyComponent"));

function App() {
  const [isPending, startTransition] = useTransition();
  const [showComponent, setShowComponent] = useState(false);

  const toggleComponent = () => {
    startTransition(() => {
      setShowComponent((prev) => !prev);
    });
  };

  return (
    <div>
      <button onClick={toggleComponent}>Toggle Component</button>

      {isPending && <div>Loading...</div>}

      {showComponent && (
        <Suspense fallback={<div>Loading...</div>}>
          <LazyComponent />
        </Suspense>
      )}
    </div>
  );
}
```
✔️ `useTransition`을 사용하여 컴포넌트의 렌더링을 트랜지션으로 감싸게 됩니다. 버튼을 클릭하면 `toggleComponent` 함수가 호출되어 컴포넌트의 표시 여부를 토글합니다. 이 상태 변경은 startTransition을 통해 비동기적으로 처리됩니다.  
✔️ **Suspense**는 `LazyComponent`가 로드될 때까지 `"Loading..."` 메시지를 표시하여 사용자가 로딩 중임을 알 수 있게 합니다.  
✔️ **isPending**은 `useTransition`의 반환값 중 하나로, 트랜지션이 진행 중일 때 true를 반환합니다. 이를 통해 UI에서 **로딩 상태**를 쉽게 표시할 수 있습니다.

