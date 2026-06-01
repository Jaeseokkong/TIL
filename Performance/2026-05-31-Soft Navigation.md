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

## 5️⃣ 왜 최근 중요해졌을까?

SPA가 보편화되면서 사용자는 페이지 이동을 했다고 느끼지만 브라우저는 이를 새로운 페이지 로드로 인식하지 못하는 문제가 생겼습니다.

예를 들어:

```bash
URL 변경
 ↓
새 데이터 요청
 ↓
화면 갱신
```

사용자 입장에서는 새로운 페이지지만 브라우저 입장에서는 단순한 JavaScript 업데이트에 불과합니다.

이 때문에 기존 성능 지표는 다음과 같은 문제를 가집니다.

- LCP 측정 누락
- Navigation Timing 측정 불가
- Page View 집계 누락
- 사용자 체감 성능 분석 어려움

---

## 6️⃣ Chrome의 Soft Navigation 지원

최근 Chrome은 SPA 환경에서도 페이지 이동을 인식하기 위해 Soft Navigation API를 도입하고 있습니다.

이를 통해:

- Soft Navigation 감지
- Navigation 기반 성능 측정
- Core Web Vitals 개선
- SPA 페이지뷰 분석

이 가능해지고 있습니다.

즉,

```bash
URL 변경
 ↓
화면 전환
 ↓
새 콘텐츠 렌더링
```

을 하나의 Navigation으로 취급하려는 것입니다.

---

## 7️⃣ 언제 발생할까?

- React Router의 Link
- React Router의 navigate
- Next.js의 Link
- Next.js의 router.push
- Vue Router
- Angular Router

반대로 다음은 Hard Navigation입니다.

```js
<a href="/about">
```

```js
window.location.href = "/about";
```

```js
window.location.reload();
```

---

## ✍️ 한 줄 정리

```
Soft Navigation은 브라우저가 새로운 문서를 다시 로드하지 않고 JavaScript Router를 통해 화면과 URL만 변경하여 페이지 이동처럼 동작하는 방식입니다.
```