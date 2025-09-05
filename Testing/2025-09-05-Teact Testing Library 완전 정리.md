# React Testing Library 완전 정리

## 1️⃣ React Testing Library란?
> **React Testing Library(RTL)**는 React 컴포넌트를 **사용자 관점에서 테스트**하기 위해 만들어진 라이브러리입니다.

- DOM Testing Library를 기반으로 만들어짐
- **구현 세부사항이 아닌 동작(behavior)** 검증에 초점
- **"사용자가 보는 것과 같은 방식"**으로 요소를 찾고 상호작용

---

## 2️⃣ 설치
```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

- `@testing-library/react`: 핵심 기능 제공 (렌더링, 쿼리)
- `@testing-library/jest-dom`: Jest에서 사용할 수 있는 DOM 전용 Matcher 제공
