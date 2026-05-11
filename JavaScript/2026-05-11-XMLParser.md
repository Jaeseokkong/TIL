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
