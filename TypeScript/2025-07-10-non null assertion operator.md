# `!` 연산자 완전 정리: null 아님을 보장하는 방법
TypeScript를 쓰다 보면 `null`과 `undefined`는 늘 신경 써야 하는 부분입니다.  
하지만 어떤 값이 절대 null이 아님을 개발자가 더 잘 알고 있는 경우도 있습니다.

이럴 때 사용하는 것이 바로 **non-null assertion operator**, 즉 `!` 연산자입니다.

```ts
const name: string | null = getName();
console.log(name!.length); // 컴파일러에게 "이건 null 아냐!"라고 단언
```


