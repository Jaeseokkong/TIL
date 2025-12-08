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

## 2️⃣ `setValue`

> "특정 필드의 값을 프로그래밍적으로 업데이트하는 함수"

폼 데이터를 직접 조작해야 할 때 사용합니다.  
특히 사용자 입력 없이 값이 자동으로 변경되는 상황에서 자주 등장합니다.

### 🧐 사용 예시

```tsx
setValue("name", "홍길동", {
  shouldValidate: true,
  shouldDirty: true,
  shouldTouch: true,
});
```

- `shouldValidate`: 값 설정 후 즉시 검증 실행
- `shouldDirty`: dirty 상태 업데이트
- `shouldTouch`: touched 플래그 업데이트

실무에서 특히 유용한 옵션들입니다.

### 📌 언제 사용하나?

- 자동 입력 (자동완성, 주소 API 등)
- 특정 값에 기반한 계산 결과 채우기
- reset 이후 특정 값만 다시 업데이트

---