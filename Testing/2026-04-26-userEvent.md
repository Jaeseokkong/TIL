# 🧪 userEvent

`userEvent`는 Testing Library 생태계에서 제공하는 유틸로, **실제 사용자의 인터렉션을 시뮬레이션하는 테스트 도구**입니다.

👉 기존 `fireEvent`보다 더 현실적인 이벤트 흐름을 만들어줌

---

## 1️⃣ `fireEvent` vs `userEvent`

### 🔹 fireEvent

```js
fireEvent.click(button);
```

- 단일 이벤트만 실행(`click`)
- 동시(sync) 처리
- 빠르지만 현실과 차이 있음

---

### 🔹 userEvent

```ts
const user = userEvent.setup();
await user.click(button);
```

실제 동작:

```bash
pointer → focus → mouseDown → mouseUp → click
```

👉 브라우저에서 발생하는 **이벤트 흐름 전체를 재현**

---

### 🔥 핵심 차이

| 항목     | fireEvent | userEvent |
| ------ | --------- | --------- |
| 관점     | 이벤트 발생    | 사용자 행동    |
| 이벤트 흐름 | ❌ 없음      | ✅ 있음      |
| 실행     | sync      | async     |
| 현실성    | 낮음        | 높음        |

---

## 2️⃣ 주요 사용 예시 

### 🔹 클릭

```ts
await user.click(button);
```

---

### 🔹 입력

```ts
await user.type(input, "hello")
```

- 한 글자씩 입력됨 (실제 타이핑처럼)

---

### 🔹 키보드

```ts
await user.keyboard("{Enter}")
```

---

### 🔹 초기화

```ts
await user.clear(input)
```

---