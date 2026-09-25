# 🌀 React Portal 활용

Portal은 컴포넌트를 **부모 DOM 트리 바깥의 다른 DOM 노드**에 렌더링할 수 있게 해주는 기능입니다.

---

## 1️⃣ 왜 필요한가

- 모달, 툴팁, 드롭다운처럼 부모 요소의 `overflow: hidden`이나 `z-index` 스택 컨텍스트에 갇히면 안 되는 UI에 사용
- React 트리 상으로는 여전히 부모의 자식이지만, 실제 DOM 위치는 별도의 노드로 옮길 수 있음

---

## 2️⃣ 예시 코드

```jsx
import { createPortal } from "react-dom";

function Modal({ children }) {
  return createPortal(
    <div className="modal-overlay">{children}</div>,
    document.getElementById("modal-root") // index.html에 미리 준비된 노드
  );
}
```

```html
<body>
  <div id="root"></div>
  <div id="modal-root"></div>
</body>
```

- DOM 위치는 `modal-root`로 이동하지만, 이벤트 버블링은 React 트리(실제 부모)를 따라 그대로 전파됨
