# 🛣️ Critical Rendering Path 이해하기

브라우저가 HTML/CSS/JS를 받아 **화면에 첫 픽셀을 그리기까지** 거치는 단계를 이해하면, 어디를 최적화해야 할지 명확해집니다.

---

## 1️⃣ 렌더링 파이프라인 단계

1. **DOM 생성** - HTML을 파싱해 DOM 트리 구성
2. **CSSOM 생성** - CSS를 파싱해 스타일 규칙 트리 구성
3. **Render Tree 생성** - DOM + CSSOM을 결합해 실제로 화면에 그릴 노드만 구성
4. **Layout(Reflow)** - 각 노드의 위치와 크기 계산
5. **Paint** - 실제 픽셀을 그림
