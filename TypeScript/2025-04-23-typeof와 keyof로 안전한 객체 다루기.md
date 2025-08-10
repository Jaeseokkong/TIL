# typeof와 keyof를 활용한 타입 안전한 객체 사용법
`typeof`와 `keyof`를 활용해 **타입 안전성을 갖춘 객체를 정의하고 활용**할 수 있습니다.  
실수 없이 안전하게 키와 값을 다루며, 디자인 시스템 같은 곳에 유용하게 사용할 수 있습니다.

## 1️⃣ 핵심 키워드 요약
|키워드|설명|
|:---|:---|
|`typeof`|값을 기반으로 타입을 추출|
|`keyof`|객체 타입의 키만 뽑아서 유니언 타입으로 만들기|

---

## 2️⃣ 실습 예시 - 테마 객체에서 타입 추출하기
```ts
const theme = {
	colors: {
		primary: '#3498db',
    secondary: '#2ecc71',
    danger: '#e74c3c'
	}
}
```

### 🔹 `typeof`로 객체의 타입을 추출
```ts
type Theme = typeof theme;

// ✅ Theme 타입
// type Theme = {
//     colors: {
//         primary: string;
//         secondary: string;
//         danger: string;
//     };
// }
```
✔️ `Theme` 타입은 `theme` 객체의 구조와 **동일한 타입**을 갖습니다.

### 🔹 `keyof`로 유효한 키만 추출해서 사용
```ts
type ColorKeys = keyof Theme['colors'];

// ✅ 해당 키 값들을 갖는 유니온 타입으로 지정
// type ColorKeys = "primary" | "secondary" | "danger" 
```


### 🔹 특정 키 제한
```ts
function getColor(key: ColorKeys) {
  return theme.colors[key];
}

getColor('primary');   // ✅
getColor('warning');   // ❌ 컴파일 에러! ('warning'은 없는 키)
```

---

## 3️⃣ 사용하는 이유
- 🔒 **키 값 오타를 타입 수준에서 방지**
- 🧠 자동완성 지원 → 생산성 향상
- 📦 `theme` 객체가 커져도 타입이 자동으로 따라감
- 🔁 타입과 값이 동기화됨 (값이 바뀌면 타입도 같이 변경됨)


