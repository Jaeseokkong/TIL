# `!` 연산자 완전 정리: null 아님을 보장하는 방법
TypeScript를 쓰다 보면 `null`과 `undefined`는 늘 신경 써야 하는 부분입니다.  
하지만 어떤 값이 절대 null이 아님을 개발자가 더 잘 알고 있는 경우도 있습니다.

이럴 때 사용하는 것이 바로 **non-null assertion operator**, 즉 `!` 연산자입니다.

```ts
const name: string | null = getName();
console.log(name!.length); // 컴파일러에게 "이건 null 아냐!"라고 단언
```

## 1️⃣ 왜 이 연산자가 필요할까?
```ts
const el = document.getElementById("input") as HTMLInputElement;
el.value = "hello" // ❌ 오류: el이 null일 수도 있음
```

TypeScript는 `getElementById`가 null을 반환할 수도 있다고 경고합니다.  
하지만 개발자는 해당 요소가 **반드시 존재한다는 걸 알고 있을 수** 있습니다.  
이럴 때 `!` 연산자를 쓰며, 타입스크립트가 경고하지 않도록 만들 수 있습니다.

---
<br>

## 2️⃣ `!` 연산자란?
### ✅ 정의
`!` 연산자는 변수 뒤에서 붙여서 **"이건 절대 null 또는 undefined가 아니야!"** 라고 컴파일러에게 알려주는 역할을 합니다.
```ts
function getLength(str: string | null): number {
	return str!.length; // null 아님을 보장
}
```
- 컴파일러는 null이 아님을 믿으며, 런타임에 null이면 에러 발생
> 즉 `!` 연산자는 **타입 체크를 우회하는 강단 단언**입니다. 믿고 쓰되 책임은 개발자에게 있습니다.
 
 ---
 <br>