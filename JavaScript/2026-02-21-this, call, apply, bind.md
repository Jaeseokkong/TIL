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

## 2️⃣ 화살표 함수의 this (특수 케이스)

> 화살표 함수는 this 바인딩 규칙을 따르지 않음

```js
const obj = {
  name: "Jun",
  greet: () => {
    console.log(this.name);
  }
};
```

✔️ 상위 스코프의 this 사용<br>
✔️ call/apply/bind로 변경 불가능

```js
obj.greet.call({ name: "Kim" }); // 여전히 변경 안 됨
```

---

## 3️⃣ bind() - this를 고정

> 새로운 함수를 반환 (즉시 실행 ❌)

```js
function greet() {
  console.log(this.name);
}

const user = { name: "Jun" };

const bound = greet.bind(user);
bound(); // Jun
```

### 🔹 특징

- this 영구 고정
- 부분 적용 가능

```js
function add(a, b) {
  return a + b;
}

const add5 = add.bind(null, 5);
add5(3); // 8
```

👉 첫 번째 인자 미리 고정

---

## 4️⃣ call() - 즉시 실행

```js
greet.call(user, arg1, arg2);
```

✔️ 즉시 실행<br/>
✔️ 인자 개별 전달

---

## 5️⃣ apply() - 배열 인자 전달

```js
greet.apply(user, [arg1, arg2]);
```

✔️ 즉시 실행<br/>
✔️ 인자를 배열로 전달

---

## 6️⃣ 메서드 빌려쓰기

```js
const arrLike = {
  0: "a",
  1: "b",
  length: 2
};

Array.prototype.forEach.call(arrLike, console.log);
```

✔️ 배열이 아닌 객체에 배열 메서드 사용<br/>
✔️ call/apply 자주 쓰이는 패턴

---

## 7️⃣ 콜백에서 this 깨지는 이유

```js
setTimeout(counter.increase, 1000);
```

→ setTimeout이 함수를 **일반 함수로 호출**

그래서 기본 바인딩 적용<br/>
→ strict mode면 undefined

### 🔹 해결 방법

#### bind 사용

```js
setTimeout(counter.increase.bind(counter), 1000);
```

#### 화살표 함수 사용
```js
setTimeout(() => counter.increase(), 1000);
```

#### 클래스 필드 화살표 메서드
```js
class Counter {
  increase = () => {
    console.log(this.count);
  };
}
```

---