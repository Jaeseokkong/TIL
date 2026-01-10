# 🏘️ Route Group - 공통 레이아웃 분리 패턴

Route Group은 **URL** 구조에는 영향을 주지 않으면서,  
**레이아웃·미들웨어·로딩 전략을 논리적으로 묶기 위한 디렉토리 패턴**입니다.

---

## 1️⃣ Route Group이란?

> URL에는 나타나지 않지만, 라우트 트리를 분리하기 위한 디렉토리

```text
(site)
(auth)
(marketing)
```

- `( )`로 감싼 폴더는 **라우팅 경로에 포함되지 않음**
- 내부 구조는 **일반 라우트와 동일하게 동작**

```bash
app/
├── (site)/
│   ├── page.tsx
│   └── about/page.tsx
```

실제 URL
- `/`
- `/about`

---

## 2️⃣ Route Gruop의 필요성

App Router의 `layout.tsx`는 **하위 라우트에 자동 전파**됩니다.

```txt
app/layout.tsx
```

- 모든 페이지에 공통 적용
- ❌ 특정 페이지에서만 제외하기 어려움

### 문제 상황

- 대부분의 페이지 → Header / Footer 필요
- 특정 페이지 (resume, auth, pdf, landing) → ❌ 공통 레이아웃 제외

👉 **Route Group**으로 트리를 분리하면 해결

---

## 3️⃣ 공통 레이아웃 분리 패턴

### 🔹 구조 예시

```bash
app/
├── (site)/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── posts/
│   └── projects/
│
├── resume/
│   └── page.tsx
```

- `(site)` 그룹에만 공통 레이아웃 적용
- `resume`는 루트 레이아웃 영향 ❌

---

### 🔹 공통 레이아웃

```tsx
// app/(site)/layout.tsx
export default function SiteLayout({ children }: { children: React.ReactNode; }) {
	return (
		<html lang="ko">
			<body>
				<Header />
				{children}
				<Footer />
			</body>
		</html>
	);
}
```

---

### 🔹 레이아웃 제외 페이지

```tsx
export default function SiteLayout({ children }: {children: React.ReactNode; }) {
	return (
		<html lang="ko">
			<body>
				<Header />
				{children}
				<Footer />
			</body>
		</html>
	);
	}
```

📌 루트 layout이 없기 때문에

- `<html>` / `<body>` 직접 포함 필수

---