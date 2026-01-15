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

### 🔹 핵심 포인트

- 이터레이터는 **상태(staet)** 를 기억
- 한 번 끝나면 다시 처음부터 ❌

---

## 3️⃣ 이터러블과 이터레이터의 관계

```bash
Iterable
  └─ Symbol.iterator()
       └─ Iterator
            └─ next()
```
- **이터러블** → 이터레이터를 만들 수 있는 객체
- **이터레이터** → 실제 반복을 수행하는 객체

---

## 4️⃣ for...of의 내부 동작

```js
for (const v of iterable) {
  console.log(v);
}
```

내부적으로는 👇

```js
const iterator = iterable[Symbol.iterator]();

while (true) {
  const { value, done } = iterator.next();
  if (done) break;
  console.log(value);
}
```

---
 
## 5️⃣ 제너레이터(Generator)란?

>**이터레이터를 생성하는 함수**<br/>
실행을 중간에 멈췄다가 다시 이어서 실행 가능

```js
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

const g = gen();

g.next(); // { value: 1, done: false }
g.next(); // { value: 2, done: false }
g.next(); // { value: 3, done: false }
g.next(); // { value: undefined, done: true }
```

### 🔹 제너레이터의 특징

- `function*` 문법
- `yeild`로 값 반환
- **이터러블 + 이터레이터** 둘 다 만족

```js
g[Symbol.iterator]() === g; // true
```

## 6️⃣ 제너레이터가 해결하는 문제

### ❌ 일반 이터레이터 구현

```js
const iterator = {
  i: 0,
  next() {
    return this.i < 3
      ? { value: this.i++, done: false }
      : { done: true };
  },
};
```

### ✅ 제너레이터

```js
function* counter() {
  let i = 0;
  while (i < 3) {
    yield i++;
  }
}
```

👉 상태 관리, 종료 조건을 **언어 레벨에서 해결**

---