# 📋 React Hook Form — Validation & 에러 처리

폼 검증은 단순한 `required`를 넘어서 비동기 검사, 스키마 기반 검증, 서버 에러 매핑, nested/array 피드 처리까지 다양합니다.  

---

## 1️⃣ 핵심 개념 요약

- **Validation 근간**: RHF는 기본적으로 `register`의 rules, `Controller`의 rules, 또는 `resolver`(schema)로 검증을 수행합니다.

- **에러 보관소**: 모든 필드의 에러는 `formState.errors`에 트리 구조로 저장됩니다.

- **동기/비동기**: `validate`에서 Promise를 반환하면 비동기 검증도 지원됩니다.

- **서버 에러**: 서버에서 넘어온 에러는 `setError`로 필드에 매핑하거나 폼 전역 에러로 처리합니다.

- **포커싱**: `setFocus`로 첫 번째 에러 필드에 자동 포커스 가능합니다.

---

## 2️⃣ 기본 검증 (register의 rules)

```tsx
const { register, handleSubmit, formState: { errors } } = useForm();

<input
  {...register("email", {
    required: "이메일은 필수입니다.",
    pattern: {
      value: /\S+@\S+\.\S+/,
      message: "이메일 형식이 아닙니다."
    },
    minLength: { value: 5, message: "최소 5자 이상" }
  })}
/>
{errors.email && <p>{errors.email.message}</p>}
```

- `required`, `min`, `max`, `minLength`, `maxLength`, `pattern`, `validate` 등을 제공
- `validate`는 커스텀 로직(동기/비동기 모두 가능)

---

## 3️⃣ validate로 커스텀(동기 + 비동기)

### 🔹 동기 validate

```tsx
register("username", {
	validate: (value) => value !== "admin" || "사용할 수 없는 아이디입니다."
})
```

### 🔹 비동기 validate (Promise 반환)

```tsx
register("username", {
	validate: async (value) => {
		const available = await apiCheckUsername(value); // 서버 확인
		return available || "이미 사용 중인 아이디입니다.";
	}
})
```

- 비동기 validate 사용 시 제출(submit)은 Promise가 resolve될 때까지 대기

---
