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