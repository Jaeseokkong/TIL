# 🎨 CSS 변수 (Custom Properties) 활용

CSS 변수(`--변수명`)는 값을 재사용하고, 런타임에 JS로도 값을 바꿀 수 있는 CSS 네이티브 기능입니다.

---

## 1️⃣ 기본 문법

```css
:root {
  --primary-color: #4f46e5;
  --spacing-md: 16px;
}

.button {
  background: var(--primary-color);
  padding: var(--spacing-md);
}
```

- `:root`에 정의하면 전역에서 사용 가능
- Sass 변수와 달리 **브라우저가 런타임에 해석**하므로 값이 동적으로 바뀔 수 있음

---

## 2️⃣ 다크모드에 활용하기

```css
:root {
  --bg-color: #ffffff;
  --text-color: #111111;
}

[data-theme="dark"] {
  --bg-color: #111111;
  --text-color: #ffffff;
}

body {
  background: var(--bg-color);
  color: var(--text-color);
}
```

- `data-theme` 속성만 바꾸면 하위 요소 전체의 색상이 자동으로 갱신됨

---

## 3️⃣ JavaScript로 값 변경하기

```js
document.documentElement.style.setProperty("--primary-color", "#22c55e");
```

- 사용자가 테마 색상을 직접 고르는 기능 등을 구현할 때 CSS 재컴파일 없이 즉시 반영 가능
