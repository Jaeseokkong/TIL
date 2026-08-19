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
