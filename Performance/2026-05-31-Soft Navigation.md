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