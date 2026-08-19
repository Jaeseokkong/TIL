# ⛓️ Optional Chaining과 Nullish Coalescing

깊은 객체 속성에 접근하거나 기본값을 지정할 때 자주 쓰는 ES2020 문법 두 가지를 정리합니다.

---

## 1️⃣ Optional Chaining (?.)

```js
const city = user?.address?.city; // address가 없으면 undefined, 에러 없이 종료
const result = obj.method?.(); // 메서드가 없을 수도 있을 때
```

- 중간에 `null`/`undefined`를 만나면 즉시 `undefined`를 반환하고 이후 접근을 멈춤
- `user.address.city`처럼 직접 접근하면 address가 없을 때 TypeError 발생

---

## 2️⃣ Nullish Coalescing (??)

```js
const count = data.count ?? 0;      // data.count가 null/undefined일 때만 0
const count2 = data.count || 0;     // data.count가 0, "", false여도 0으로 대체됨 (의도치 않은 동작)
```

- `||`는 falsy 전체를 대체하지만, `??`는 **null과 undefined만** 기본값으로 대체
- 숫자 0이나 빈 문자열도 유효한 값으로 다뤄야 할 때는 반드시 `??` 사용

---

## 3️⃣ 함께 사용하기

```js
const nickname = user?.profile?.nickname ?? "익명";
```

- 깊은 경로 접근과 기본값 지정을 한 번에 안전하게 처리 가능

---

## ✍️ 한 줄 정리

> **Optional Chaining은 중간 값이 없을 때 에러 없이 undefined를 반환하고, Nullish Coalescing은 null/undefined일 때만 기본값을 적용한다.**
