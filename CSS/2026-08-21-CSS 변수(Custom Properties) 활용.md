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
