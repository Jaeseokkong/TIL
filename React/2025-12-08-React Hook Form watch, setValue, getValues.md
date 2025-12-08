# 📋 React Hook Form 기본: `watch`, `setValue`, `getValues`

React Hook Form(RHF)은 상태를 최소한으로 리렌더링하며 폼을 다룰 수 있게 해주는   라이브러리입니다.  
그중에서도 `watch`,`setValue`,`getValues`는 **폼을 컨트롤하는 핵심 API**입니다.

이 세 가지를 이해하면, state를 직접 다루지 않고도 **폼 값을 실시간으로 읽거나 변경**할 수 있습니다.

---

## 1️⃣ `watch`

> "특정 필드나 전체 폼의 값을 실기간으로 구독하는 함수"

일반적으로 React에서 입력값 변화를 감지하려면 state를 setter로 업데이트해야 하지만, RHF는 내부적으로 uncontrolled 컴포넌트를 기반으로 하기 때문에 `watch()`만 호출하면 **바로 최신 값**을 구독할 수 있습니다.

### 🧐 사용 예시

```tsx
const { watch } = useForm();

const name = watch("name");
```

`watch("name")`은 name 필드가 입력될 때마다 자동으로 리렌더링되어 최신 값을 반영합니다.

### 📌 언제 사용하나?

- 실시간 미리보기
- 조건부 UI 렌더링
- 특정 값에 따라 다른 필드를 활성화/비활성화

---