# 🩹 hydration

Next.js에서 Hydration은 **서버에서 생성된 HTML에 React가 이벤트와 상태를 연결해 "동적인 앱"으로 만드는 과정**입니다.

즉, 단순한 정적 HTML을 **인터렉티브한 React 앱으로 변환하는 단계**

---

##  1️⃣ Hydration이 필요한 이유

React는 기본적으로 클라이언트에서 동작하지만,<br/>
Next.js는 **서버 사이트 렌더링(SSR)**을 사용합니다.

### 🔹 동작 흐름

1. 서버에서 HTML 생성 (SSR)
2. 브라우저가 HTML 먼저 렌더링
3. JS 번들 다운로드
4. React가 HTML에 이벤트/상태 연결
👉 Hydration 완료

이 연결 과정이 바로 **Hydration**

---

## 2️⃣ 핵심 동작

### 🔹 기존 HTML을 재사용

React가 새로운 DOM을 그리는 것이 아니라,<br/>
**서버에서 만들어진 HTML을 그대로 활용**합니다.

---

### 🔹 이벤트와 상태만 연결

```html
<button>클릭</button>
```

서버에서 내려온 상태 (동작 안 함)

---

```html
<button onClick={handleClick}>클릭</button>
```

Hydration 이후 (클릭 가능)

즉, 화면은 이미 존재하고 React가 기능만 붙입니다.

---