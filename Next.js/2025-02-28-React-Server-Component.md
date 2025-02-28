# React Server Components (RSC) 기본 이해
## 1️⃣ RSC란?
**React Server Component (RSC)**는 **서버에서 렌더링**되는 React 컴포넌트를 사용하여, **클라이언트 측에서의 자바스크립트 부담을 줄이고 성능을 최적화**하는 새로운 방법입니다. RSC는 클라이언트에서 불필요한 자바스크립트 실행을 최소화하여 **웹 애플리케이션의 로딩 속도**와 **사용자 경험**을 개선하는데 중점을 둡니다.

## 2️⃣ 기존 렌더링 방식과의 차이점
### 🔹 Client-Side Rendering (기존 React)
- **전체 애플리케이션을 클라이언트에서 렌더링**하며, 데이터를 클라이언트에서 비동기적으로 요청
- 렌더링에 필요한 자바스크립트가 클라이언트에서 모두 실행되므로 **초기 로딩 시간이 길어질 수 있음**
- **상태 관리와 데엍 페칭** 등 복잡한 로직이 클라이언트에서 처리

### 🔹 Server-Side Rendering
- **서버에서 HTML을 렌더링**하여 클라이언트로 전달
- 각 페이지 요청 시마다 서버에서 HTML을 다시 생성하므로 서버에 부담이 될 수 있음
- **매번 렌더링을 새로 해야 하므로 선능에 제한**이 있을 수 있음

### 🔹 React Server Components
- **서버에서만 렌더링되는 컴포넌트**는 클라이언트에 HTML로 전달되어 클라이언트에서 별도로 자바스크립트 실행이 필요 없거나 최소화됨
- **클라이언트는 서버에서 렌더링된 HTML을 받아서 화면을 즉시 표시**할 수 있으므로 페이지 로딩 속도가 빨라짐
- **서버에서 데이터를 미리 가져와서 렌더링**하므로 클라이언트에서의 데이터 요청과 처리 부담을 줄일 수 있음

#### 🧐 예시: RSC의 동작
```tsx
//ServerComponent.tsx

function ServerComponent() {
	return (
		<div>
			<h1>This component is rendered on the server</h1>
      <p>It doesn't run on the client.</p>
		</div>
	)
} 

export default ServerComponent;
```
✔️ 서버에서 이 컴포넌트를 렌더링하고, 클라이언트에는 이미 **렌더링된 HTML만 전달**됩니다.  
✔️ 클라이언트에서는 **불필요한 자바스크립트를 실행하지 않아** 성능이 최적화됩니다.

<br>

- - -

<br>

## 3️⃣ 실습: RSC 사용하기
React Server Components는 **Next.js 13 이상**에서 기본적으로 지원됩니다. Next.js에서는 서버에서만 실행되는 컴포넌트를 손쉽게 사용할 수 있습니다. 아래는 Next.js에서 RSC를 사용하는 기본적인 예시입니다.

### 1. 서버 측에서 컴포넌트 작성
먼저, 서버에서 렌더링될 컴포넌트를 작성합니다. RSC는 **서버에서만 실행**되므로 클라이언트 측에서 데이터를 처리하거나 DOM을 조작하는 작업은 할 수 없습니다.
```tsx
// app/serverComponent.tsx
export default function ServerComponent() {
  return (
    <div>
      <h1>This is a Server Component!</h1>
      <p>Rendered on the server, not the client.</p>
    </div>
  );
}
```

### 2. `Suspense`로 비동기 데이터 로딩 처리
RSC는 `Suspense`와 함께 사용하여, 서버에서 비동기적으로 데이터를 가져오고, 데이터를 로드할 때까지 **로딩 화면을 표시**할 수 있습니다.
```tsx
// app/page.tsx
import React, { Suspense } from 'react';
import ServerComponent from './serverComponent';

export default function Page() {
  return (
    <div>
      <h1>React Server Components in Next.js</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <ServerComponent />
      </Suspense>
    </div>
  );
}
```
✔️ `Suspense`를 사용하여 서버에서 데이터를 비동기적으로 가져올 때까지 로딩 UI를 표시합니다.
✔️ `ServerComponent`는 서버에서만 실행되며, 클라이언트에는 이미 렌더링된 HTML만 전달됩니다.

### 3. 클라이언트에서 데이터와 상태 처리
서버 컴포넌트는 클라이언트 상태를 처리할 수 없으므로, 클라이언트에서만 실행되는 컴포넌트는 `useState`와 같은 React 훅을 사용하여 **상태를 관리**해야 합니다.
```tsx
// app/clientComponent.tsx
'use client';

import React, { useState } from 'react';

export default function ClientComponent() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>Client Component</h2>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
    </div>
  );
}
```

✔️ `use client` 디렉티브를 사용하여 이 컴포넌트는 **클라이언트에서만 실행**되게 합니다.  
✔️ 클라이언트에서 상태를 관리하는 컴포넌트와 서버에서만 실행되는 컴포넌트를 분리하여 최적화된 애플리케이션을 만들 수 있습니다.