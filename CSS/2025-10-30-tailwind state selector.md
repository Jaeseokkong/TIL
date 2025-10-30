# 🎨 Tailwind CSS – State Selector

**State Selector**는 요소의 상태(hover, focus, checked 등)에 따라 스타일을 다르게 지정할 수 있게 해주는 Tailwind CSS의 핵심 기능입니다.  
CSS의 pseudo-class(`:hover`, `:focus`, `:checked`, ...)를 **Tailwind 접두사(prefix)** 형태로 표현합니다.

---

## 1️⃣ 기본 개념

Tailwind에서는 상태 기반 스타일을 **접두사 + 콜론(:)** 형태로 지정합니다.

```html
<button class="hover:bg-blue-500 focus:ring-2">Click me</button>
```

이는 일반 CSS의 아래 코드와 동일합니다.

```css
button:hover { background-color: #3b82f6; }
button:focus { outline: 2px solid #3b82f6; }
```

> **상태 변화 = 접두사로 표현**  
Tailwind는 CSS 상태 선택자를 유틸리티 단위로 추상화합니다.

---
