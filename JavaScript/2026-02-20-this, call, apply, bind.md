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