# 📋 React Hook Form — useFieldArray 정리

`useFieldArray`는 **동적으로 변한는 폼 필드 리스트**를 관리하기 위한 React Hook Form의 핵심 API입니다.  
단일 input을 다루는 것과 달리, 배열 형태의 필드를 추가/삭제/정렬해야 하는 경우에 사용합니다.

예를 들면 다음과 같은 상황입니다:

- 주소를 여러 개 추가하는 폼
- 상품 옵션을 동적으로 추가/삭제
- 스케줄, 멤버, 연락처 리스트
- 장바구니 아이템 목록

---

## 1️⃣ 왜 `useFieldArray`가 필요한가?

일반적인 폼은 필드 구조가 **정적**입니다.

```tsx
<input {...register("name")} />
<input {...register("email")} />
```

하지만 동적 폼은 **필드 개수와 구조가 런타임에 변합니다.**

```tsx
addresses[0].street
addresses[1].street
addresses[2].street
```

이걸 `useState`로 관리하면:

- index 꼬임
- key 관리
- validation 연동
- 에러 동기화

등이 매우 복잡해집니다.

✔️` useFieldArray`는 이 문제를 React Hook Form 내부 상태와 완전히 동기화된 방식으로 해결합니다.

---

## 2️⃣ 기본 사용법

### 🔹 기본 구조

```tsx
const { control, register } = useForm();

const { fields, append, remove } = useFieldArray({
  control,
  name: "addresses",
});
```

- `control`: `useForm`에서 내려받은 `control` 객체
- `name`: 배열로 관리할 필드 이름
- `fields`: 현재 필드 배열 상태
- `append`: 항목 추가
- `remove`: 항목 삭제

### 🔹 예제

```tsx
type FormData = {
  addresses: { street: string }[];
};

const { register, control, handleSubmit } = useForm<FormData>({
  defaultValues: {
    addresses: [{ street: "" }],
  },
});

const { fields, append, remove } = useFieldArray({
  control,
  name: "addresses",
});

return (
  <>
    {fields.map((field, index) => (
      <div key={field.id}>
        <input
          {...register(`addresses.${index}.street`, {
            required: "주소는 필수입니다.",
          })}
        />
        <button type="button" onClick={() => remove(index)}>
          삭제
        </button>
      </div>
    ))}

    <button
      type="button"
      onClick={() => append({ street: "" })}
    >
      주소 추가
    </button>
  </>
);
```

#### 🔑 핵심 포인트

- `field.id`를 **key로 반드시 사용**
- `index는` register 경로에만 사용
- `append/remove`는 RHF 상태를 직접 조작

---

## 3️⃣ key 관리가 중요한 이유

### ❌ 잘못된 예:

```tsx
key={index}
```

이렇게 하면 삭제/정렬 시:

- 입력 값이 다른 필드로 이동
- 에러 메시지가 엉뚱한 곳에 표시

### ✅ 올바른 예:

```tsx
key={field.id}
```

`useFieldArray`는 내부적으로 안정적인 id를 생성해줍니다.

---