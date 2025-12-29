# 📋 React Hook Form — useFieldArray 정리

`useFieldArray`는 **동적으로 변하는 폼 필드 리스트**를 관리하기 위한 React Hook Form의 핵심 API입니다.  
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

✔️ `useFieldArray`는 이 문제를 React Hook Form 내부 상태와 완전히 동기화된 방식으로 해결합니다.

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
- `index`는 register 경로에만 사용
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

## 4️⃣ 유용한 API

```ts
const {
  append,
  remove,
  insert,
  swap,
  move,
  update,
  replace
} = useFieldArray();
```

---

### 🔹 `append`

배열의 맨 뒤에 새로운 필드를 추가

```ts
append({ name: "", price: 0 });
```

- 가장 많이 사용됨
- “추가 버튼” 클릭 시 보통 사용
- 기본적인 아이템 추가 케이스

---

### 🔹 `remove`

특정 index의 필드 제거

```ts
remove(index);
```

- 삭제 버튼 클릭 시 사용
- 해당 index의 값 + 에러 상태까지 함께 제거됨
- 여러 개 삭제도 가능: `remove([1, 3])`

---

### 🔹 `insert`

특정 위치(index)에 필드 삽입

```ts
insert(1, { name: "", price: 0 });
```

- 중간에 아이템을 끼워 넣을 때
- 순서가 중요한 폼(우선순위, 단계 등)에 유용

---

### 🔹 `swap`

두 index의 위치를 서로 교환

```ts
swap(0, 1);
```

- 위/아래 이동 버튼 구현 시 사용
- 드래그 앤 드롭 구현 전 단계에서 자주 쓰임

---

### 🔹 `move`

한 필드를 다른 위치로 이동

```ts
move(fromIndex, toIndex);
```

- drag & drop 정렬 구현 시 가장 많이 사용
- `swap`보다 일반적인 이동 패턴

---

### 🔹 `update`

특정 index의 값을 새로운 값으로 교체

```ts
update(2, { name: "수정됨", price: 1000 });
```

- 해당 index의 값 전체를 덮어씀
- 부분 수정이 아니라 **객체 교체**
- 계산된 결과를 반영할 때 유용

---

### 🔹 `replace`

필드 배열 전체를 새로운 배열로 교체

```ts
replace([
  { name: "A" },
  { name: "B" },
]);
```

- 서버에서 받은 배열로 초기화할 때
- `reset`과 유사하지만 `fieldArray`만 교체됨
- 전체 구조를 한 번에 바꾸는 경우 사용