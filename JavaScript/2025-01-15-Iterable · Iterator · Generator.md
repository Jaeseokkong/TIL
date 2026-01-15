# 🔁 Iterable · Iterator · Generator

**반복 가능한 객체와 반복 제어의 구조**

JavaScript에서는 `for...of`, 스프레드 무법(`...`), 구조 분해 할당이 가능한 이유는<br>
객체가 **이터러블(iterable)** 이기 때문입니다.

---

## 1️⃣ 이터러블(Iterable)이란?

> **반복 가능한 객체**<br/>
내부에 `Symbol.iterator` 메서드를 가진 객체

```js
const iterable = {
  [Symbol.iterator]() {
    return {
      next() {
        return { value: 1, done: true };
      },
    };
  },
};
```

### 🔹 대표적인 이터러블

- `Array`
- `String`
- `Map`
- `Set`
- `arguments`
- `NodeList`

👉 객체(`{}`)는 기본적으로 **이터러블이 아님**

---

## 2️⃣ 이터레이터(Iterator)란?

> **반복을 실제로 수행하는 객체**
`next()` 메서드를 가지며 `{ value, done }`을 반환

```js
const arr = [1, 2, 3];
const iterator = arr[Symbol.iterator]();

iterator.next(); // { value: 1, done: false }
iterator.next(); // { value: 2, done: false }
iterator.next(); // { value: 3, done: false }
iterator.next(); // { value: undefined, done: true }
```

### 핵심 포인트

- 이터레이터는 **상태(staet)** 를 기억
- 한 번 끝나면 다시 처음부터 ❌

---