# 📒 Soft Navigation

기존 웹 사이트에서 페이지를 이동하면 브라우저는 새로운 HTML 문서를 요청하고 화면 전체를 다시 렌더링합니다.

이름 **Hard Naviation** 이라고 합니다.

```bash
Page A
 ↓
새 HTML 요청
 ↓
Page B 로드
```

이 과정에서:

- 새로운 문서 다운로드
- JavaScript 재실행
- 상태(State) 초기화
- 전체 페이지 렌더링

이 발생합니다.

---

반면 **Soft Navigation** 은 전체 페이지를 새로고침하지 않고 필요한 UI와 데이터만 변경하여 페이지가 이동한 것처럼 동작하는 방식입니다.

```bash
Page A
 ↓
Router 처리
 ↓
데이터 요청
 ↓
Page B 렌더링
```

SPA에서 주로 사용되며 React, Next.js, Vue 등의 프레임워크에서 기본적으로 활용됩니다.

---

## 1️⃣ Hard Navigation vs Soft Navigation

| Hard Navigation | Soft Navigation |
| --------------- | --------------- |
| 전체 페이지 새로고침     | 일부 UI만 변경       |
| HTML 다시 다운로드    | 필요한 데이터만 요청     |
| JS 재실행          | 기존 앱 유지         |
| 상태 초기화          | 상태 유지 가능        |
| 상대적으로 느림        | 상대적으로 빠름        |

---

## 2️⃣ React 예시

### 🔹 Hard Navigation

```js
<a href="/about">About</a>
```

브라우저가 새로운 문서를 요청합니다.

---

### 🔹 Soft Navigation

```js
<Link to="/about">About</Link>
```

또는 

```js
navigate("/about");
```

라우터가 URL만 변경하고 필요한 컴포넌트만 다시 렌더링합니다.

---

## 3️⃣ Next.js 예시

```ts
import Link from "next/link";

<Link href="/about">
  About
</Link>
```

`next/link`는 기본적으로 Soft Navigation을 수행합니다.

페이지 전체를 다시 로드하지 않고 필요한 데이터만 가져와 화면을 갱신합니다.

---

## 4️⃣ Soft Navigation의 장점

### 🔹 빠른 사용자 경험

전체 페이지를 다시 다운로드하지 않기 때문에 전환이 자연스럽습니다.

---

### 🔹 상태 유지

예를 들어:

```bash
상품 목록
 ↓
스크롤 위치 500px
 ↓
상품 상세 이동
 ↓
뒤로가기
```

Soft Navigation을 사용하면 이전 상태를 복원하기 쉽습니다.

---

### 🔹 네트워크 비용 감소

필요한 데이터만 가져오기 때문에 불필요한 리소스 다운로드가 줄어듭니다.

---