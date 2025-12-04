# 📋 React Hook Form 기본 개념 정리

React Hook Form은 **React에서 폼 상태를 최소화의 리렌더링으로 관리**하도록 도와주는 라이브러리입니다.

---

## 1️⃣ React Hook Form이란?

> "폼 값을 React state로 게속 들고 있지 않아도 되며, 최소한의 렌더링으로 폼을 관리할 수 있도록 도와주는 라이브러리"

즉, React의 re-render 비용을 줄이면서도 검증/에러 처리/값 추적/폼 제출 흐름을 **간단한 API로 제공**합니다.

---

## 2️⃣ 기존 폼 관리의 문제점

일반적으로 input을 이렇게 관리합니다.

```jsx
const [value, setValue] = useState("");

<input value={value} onChange={(e) => setValue(e.target.value)} />;
```

이 방식의 문제

- 입력할 때마다 **컴포넌트가 리렌더링됨**
- 큰 폼(form wizard, 다단계 폼)에서는 성능 저하 발생
- validation을 직접 작성해야 해서 코드가 복잡해짐

React Hook Form은 이런 비효율을 피하기 위해 **uncontroller 기반**으로 동작합니다.

---

## 3️⃣ React Hook Form의 핵심 훅: `useForm`

```jsx
cosnt {
    register,
    handleSubmit,
    formState: { errors },
} = useForm();
```

### 🔹 각 API 역할

- **register**

    input을 React Hook Form에 연결하는 함수
    → ref 기반이라 input의 변화를 자체적으로 추적합니다.

- **handleSubmit**

    제출 시 데이터 수집 + validation 실행 + 성공/실패 콜백 진행

- **errors**
    
    각 필드의 validation 에러 정보

---

## 4️⃣ 기본 사용 예시

```jsx
const { register, handleSubmit, formState: { errors } } = useForm();

const onSubmit = (data) => {
  console.log(data);
};

return (
  <form onSubmit={handleSubmit(onSubmit)}>
    <input
      {...register("email", { required: "이메일은 필수입니다" })}
    />
    {errors.email && <span>{errors.email.message}</span>}

    <button type="submit">제출</button>
  </form>
);
```

### ⚙️ 동작 방식

- input의 값은 state로 관리하지 않아도 됩니다.
- 입력 시 **리렌더링 발생하지 않습니다.**
- 제출할 때 한 번에 data가 모입니다.
- 에러 메시지는 formState에서 자동으로 관리됩니다.

---