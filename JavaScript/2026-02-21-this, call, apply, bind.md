# 🧠 this, bind, call, apply 완전 정리

`this`는 **함수가 호출되는 방식(call-site)에 따라 결정됩니다.**

---

## 1️⃣ this 결정 규칙

JavaScript에서 this는 아래 **4가지 규칙**으로 결정됩니다.

### ① 기본 바인딩 (Default Binding)

```js
function greet() {
  console.log(this);
}

greet();
```

- 브라우저: `window`
- strict mode / ES Module: `undefined`

✔️ 호출 주체가 없으면 기본 바인딩

---

### ② 암시적 바인딩 (Implicit Binding)

```js
const user = {
  name: "Jun",
  greet() {
    console.log(this.name);
  }
};

user.greet();
```

✔️ `.` 앞의 객체가 this → user

---

### ③ 명시적 바인딩 (Explicit Binding)

👉 call / apply / bind 사용

```js
greet.call(user);
```

✔️ 개발자가 직접 this 지정

---

### ④ new 바인딩 (Constructor Binding)

```js
function User(name) {
  this.name = name;
}

const u = new User("Jun");
```

✔️ new로 호출되면 → 새로 생성된 인스턴스가 this

---

### 🎯 우선순위

```bash
new 바인딩
  > 명시적 바인딩 (call/apply/bind)
    > 암시적 바인딩
      > 기본 바인딩
```

new가 가장 강력

---