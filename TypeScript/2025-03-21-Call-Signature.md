# 호출 시그니처(Call Signature)
## 1️⃣ 호출 시그니처란?
**호출 시그니처**는 TypeScript에서 **함수 타입을 정의하는 방식**입니다.  
함수의 **매개변수 타입과 반환 타입을 명확하게 표현**할 수 있으며, 인터페이스 또는 타입 별칭과 함께 사용됩니다.
```ts
type Add = (a: number, b: number) => number;
```
✔️ `Add` 타입은 `number` 두 개를 받아 `number`를 반환하는 함수 타입입니다.
- - -

<br>

## 2️⃣ 호출 시그니처 사용 방법
### 🔹 타입 별칭을 이용한 정의
```ts
type Multiply = (x: number, y: number) => number;

const multiply: Multiply = (a, b) => a * b;
console.log(multiply(4, 5)); // 20
```
✔️ `Multiply` 타입을 정의하고, 해당 타입을 가지는 함수를 구현했습니다.

<br>

### 🔹 인터페이스를 이용한 정의
```ts
interface Divide {
  (x: number, y: number): number;
}

const divide: Divide = (a, b) => a / b;
console.loog(divide(10, 2)) // 5
```
✔️ `Divide` 인터페이스를 사용해 함수 타입을 정의할 수도 있습니다.

- - -

<br>

## 3️⃣ 호출 시그니처와 제너릭(Generic)
제너릭을 사용하면 **다양한 타입을 지원하는 유연한 함수 타입**을 정의할 수 있습니다.
### 🔹 제너릭을 활용한 호출 시그니처
```ts
type Identity<T> = (value: T) => T;
```
✔️ `Identity<T>`는 **입력과 동일한 타입을 반환하는 함수**를 정의하는 제너릭 타입입니다.

### 🔹 활용 예시
```ts
const identityString: Identity<string> = (value) => value;
console.log(identityString("Hello")); // "Hello"

const identityNumber: Identity<number> = (value) => value;
console.log(identityNumber(42)); // 42
```

- - -
<br>

## 4️⃣ 여러 개의 제너릭 타입 사용
입력과 출력의 타입이 다를 경우, **두 개 이상의 제너릭을 사용**할 수 있습니다.
```ts
type Mapper<T, U> = (input: T) => U
```
### 🔹 활용 예시
```ts
const stringToNumber: Mapper<string, number> = (value) => value.length;
console.log(stringToNumber("TypeScript")) // 10
```
✔️ `string`을 입력받아 `number`(문자열 길이)로 변환하는 함수입니다.

- - -
<br>

