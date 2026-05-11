# 📒 XMLParser

XML (Extensible Markup Language)은 데이터를 구조화해서 저정하고 전달하기 위한 마크업언어입니다.

```xml
<user>
	<name>John</name>
	<age>20</age>
</user>
```

HTML과 비슷한 형태이지만, 화면을 그리기 위한 목적보다는 **데이터 저장 및 전송**에 초점이 맞춰져 있습니다.

`XMLPaerser`는 이러한 XML 데이터를 JavaScript에서 다루기 쉬운 형태로 변환해주는 도구입니다.

즉 XML 문자열을:

```xml
<book>
	<title>Harry Potter</title> 
</book>
```

다음과 같은 JavaScript 객체로 변환한다.

```js
{ 
	book: { 
		title: "Harry Potter" 
	} 
}
```

이 과정을 Parsing(파싱) 이라고 합니다.

---

## 1️⃣ 대표 라이브러리: fast-xml-parser

### 🔹 기본 사용법

```js
import { XMLParser } from "fast-xml-parser";

const xmlData = `
<user>
	<name>John</name>
	<age>20</age>
</user>
`;

const parser = new XMLParser();

const result = parser.parse(xmlData);

console.log(result);
```

출력:

```js
{
	user: {
		name: "John",
		age: 20
	}
}
```

---

### 🔹 옵션 설정

대표 옵션

```js
const parser = new XMLParser({ 
	ignoreAttributes: false, 
	attributeNamePrefix: "@_", 
});
```

- `ignoreAttributes`: `false` 설정 시 기본 속성까지 포함
- `attributeNamePrefix`: 속성 앞에 붙일 텍스트 설정

---

## 2️⃣ fast-xml-parser 기타 기능

### 🔹 XMLBuilder

`fast-xml-parser`는 XML → 객체 변환뿐만 아니라
객체 → XML 변환 기능도 제공합니다.

```ts
import { XMLBuilder } from "fast-xml-parser";

const builder = new XMLBuilder();

const obj = {
	user: {
		name: "John",
		age: 20
	}
};

const xml = builder.build(obj);

console.log(xml);
```

출력:

```xml
<user>
	<name>John</name>
	<age>20</age>
</user>
```

---

### 🔹 XMLValidator

XML 문법이 올바른지 검사할 수도 있습니다.

```js
import { XMLValidator } from "fast-xml-parser"; 

const result = XMLValidator.validate(`
<user>
	<name>John</name>
</user> 
`);

console.log(result); // true
```

- 정상일 경우 true
- 잘못된 XML이면 에러 정보를 반환

---

## 3️⃣ DOMParser vs fast-xml-parser

| DOMParser            | fast-xml-parser        |
| -------------------- | ---------------------- |
| 브라우저 내장 API          | 외부 라이브러리               |
| XML/HTML을 DOM 형태로 변환 | XML을 JavaScript 객체로 변환 |
| 브라우저 환경 중심           | Browser + Node.js 지원   |
| DOM 탐색 필요            | 객체처럼 바로 접근 가능          |
| XML 문서 구조 분석에 적합     | 데이터 처리에 더 적합           |


### 🧐 예시

```js
const parser = new DOMParser();

const xmlDoc = parser.parseFromString(xmlData, "text/xml"); 

console.log(xmlDoc.querySelector("name").textContent);
```

DOMParser는 DOM 탐색 방식으로 데이터를 가져와야 합니다.

반명 `fase-xml-parser`는 

```js
const parser = new XMLParser();

const result = parser.parse(xmlData);

console.log(result.user.name);
```

처럼 객체 접근 방식으로 사용할 수 있어서 더 직관적입니다.

---

## ✍️ 한 줄 정리

> `XMLParser`는 XML 데이터를 JavaScript 객체로 변환해주는 도구이며,
`fast-xml-parser`는 이를 빠르고 간편하게 처리할 수 있는 대표적인 라이브러리입니다.