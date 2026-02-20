# 🧠 this, bind, call, apply 완전 정리

`this`는 **함수가 어떻게 호출되었는지에 따라 결정되는 값**입니다.

---

## 1️⃣ this란?

> 함수가 실행될 때 결정되는 실행 컨텍스트 객체

### 🔹 객체의 메서드로 호출될 때

```js
const user = {
  name: "Jun",
  greet() {
    console.log(this.name);
  }
};

user.greet(); // Jun
```

✔️ `this` → `user` 객체

---

### 🔹 일반 함수 호출

```js
function greet() {
  console.log(this);
}

greet();
```

- 브라우저: `window`
- strict mode: `undefined`

📌 일반 함수는 호출 주체가 없기 때문에 전역 객체를 가리킴

---

### 🔹 화살표 함수의 this

```js
const user = {
  name: "Jun",
  greet: () => {
    console.log(this.name);
  }
};

user.greet(); // undefined
```

❗ 화살표 함수는 **자기 자신의 this가 없음**

→ 상위 스코프의 this를 그대로 사용(lexical this)

---

## 2️⃣ bind() - this를 고정하는 방법

> this가 고정된 **새로운 함수를 반환**

```js
function greet() {
  console.log(this.name);
}

const user = { name: "Jun" };

const boundGreet = greet.bind(user);

boundGreet(); // Jun
```

### 🔹 문법

```js
const newFunc = func.bind(thisArg, arg1, arg2...)
```

✔️ 실행 ❌<br/>
✔️ 새로운 함수 반환<br/>
✔️ this 영구 고정

---
 
## 3️⃣ apply() - 배열로 인자 전달

```js
function introduce(age, job) {
  console.log(this.name, age, job);
}

const user = { name: "Jun" };

introduce.apply(user, [28, "developer"]);
```

### 🔹 문법

```js
func.apply(thisArg, [argsArray])
```

✔️ 즉시 실행 ❌<br/>
✔️ 새로운 함수 반환<br/>
✔️ this를 영구적으로 고정

---

## 4️⃣ call() - 즉시 실행 버전

> bind와 다르게 **즉시 실행**

```js
function greet() {
  console.log(this.name);
}

const user = { name: "Jun" };

greet.call(user); // Jun
```

### 🔹 문법

```js
func.call(thisArg, arg1, arg2, ...)
```

✔️ 실행 즉시 호출 <br/>
✔️ this를 원하는 객체로 지정 가능

---

## 5️⃣ 왜 필요한가?

### 🔹 콜백에서 this가 깨지는 문제

```js
class Counter {
  constructor() {
    this.count = 0;
  }

  increase() {
    console.log(++this.count);
  }
}

const counter = new Counter();

setTimeout(counter.increase, 1000); // ❌ this 깨짐
```

- `setTimeout`이 함수를 일반 함수로 호출
- 호출 주체가 사라짐
- `this`가 undefined (strict mode)

#### 🔹 해결

```js
setTimeout(counter.increase.bind(counter), 1000);
```

✔️ this를 counter로 고정

---