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

## 4️⃣ Controller와 rules (controlled 컴포넌트)

```tsx
<Controller
  control={control}
  name="date"
  rules={{ required: "날짜를 선택하세요." }}
  render={({ field }) => <DatePicker {...field} />}
/>
{errors.date && <p>{errors.date.message}</p>}
```

- `Controller`에서 `rules`를 쓰면 RHF에서 검증을 수행
- 외부 UI 라이브러리와 결함 시 많이 사용

---

## 5️⃣ 스키마 기반 검증 (resolver) — Zod / Yup 예시

### 🔹 Zod + resolver

```bash
## 설치
npm install zod @hookform/resolvers
```

```tsx
import { z } from "zod";
import { zodResolver } from "@hookform/resolvers/zod";

const schema = z.object({
  name: z.string().min(1, "이름은 필수입니다."),
  email: z.string().email("올바른 이메일을 입력하세요."),
});

const { register, handleSubmit, formState: { errors } } = useForm({
  resolver: zodResolver(schema),
});
```

- 장점: 검증 로직을 한 곳에서 선언, 타입 안전성(특히 zod) 확보.
- 단점: 작은 폼엔 오버헤드일 수 있음.

---

## 6️⃣ 서버 에러 처리 (setError / clearErrors)

서버에서 `email already exists` 같은 에러를 받았을 때 필드에 바인딩하는 일반 패턴:

```tsx
const { setError } = useForm();

try {
  await apiRegister(data);
} catch (err) {
  // 예: err.response.data = { field: 'email', message: '이미 사용중' }
  setError("email", { type: "server", message: "이미 사용중인 이메일입니다." });
  // 또는 전역 에러:
  setError("root", { type: "server", message: "서버 에러 발생" });
}
```

- `setError(name, { type, message, ... })`
- `clearErrors('email')` 또는 `clearErrors()`로 에러 제거
- `setError`는 `isValid`/`isDirty` 상태와 별개로 에러를 추가

---

## 7️⃣ 에러 UI 패턴 (접근성과 UX 고려)

- 에러 텍스트는 `<p role="alert">`처럼 접근성 표기 권장.
- 첫 에러에 자동으로 포커스:

```tsx
const { handleSubmit, setFocus } = useForm();

const onSubmit = async (data) => {
  // 서버 에러 처리 후
  setFocus("email"); // 해당 필드로 포커스
};
```

또는 `handleSubmit(onValid, onInvalid)`에서 `onInvalid`로 첫 에러 필드 focus 처리:

```tsx
const onInvalid = (errors) => {
  const firstErrorKey = Object.keys(errors)[0];
  setFocus(firstErrorKey as any);
};
<form onSubmit={handleSubmit(onValid, onInvalid)} />
```

---

## 8️⃣ formState 관련: 에러와 상태

`formState`의 useful props:

- `errors` — 필드 에러 트리
- `isDirty`, `dirtyFields` — 사용자가 변경한 필드
- `isSubmitting` — 제출 중
- `isValid` — (mode 설정에 따라) 모든 검증이 통과했는지

```tsx
const { formState: { errors, isSubmitting } } = useForm({ mode: "onBlur" });
```

- `mode` 옵션: `onSubmit`(기본), `onBlur`, `onChange`, `onTouched`, `all` — 어느 시점에 검증할지 결정

---