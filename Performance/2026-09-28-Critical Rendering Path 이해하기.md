# 🛣️ Critical Rendering Path 이해하기

브라우저가 HTML/CSS/JS를 받아 **화면에 첫 픽셀을 그리기까지** 거치는 단계를 이해하면, 어디를 최적화해야 할지 명확해집니다.

---

## 1️⃣ 렌더링 파이프라인 단계

1. **DOM 생성** - HTML을 파싱해 DOM 트리 구성
2. **CSSOM 생성** - CSS를 파싱해 스타일 규칙 트리 구성
3. **Render Tree 생성** - DOM + CSSOM을 결합해 실제로 화면에 그릴 노드만 구성
4. **Layout(Reflow)** - 각 노드의 위치와 크기 계산
5. **Paint** - 실제 픽셀을 그림

---

## 2️⃣ CSS/JS가 렌더링을 막는 이유

- CSS는 **렌더링 차단 리소스**: CSSOM이 완성되기 전까지 브라우저는 Render Tree를 만들 수 없어 화면을 그리지 않음
- `<script>`는 기본적으로 **파싱을 차단**: 실행이 끝날 때까지 이후 HTML 파싱이 멈춤 (`defer`/`async`로 완화 가능)

```html
<script src="analytics.js" defer></script> <!-- HTML 파싱을 막지 않고, 파싱 완료 후 실행 -->
```
