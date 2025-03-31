# CSS 선택자 (class vs id)
## 1️⃣ class (`.`) 선택자
- **재사용 가능**: 여러 요소에 동일한 스타일을 적용할 때 사용
- **특정 스타일을 위해 명명 가능**: 의미 있는 클래스명을 사용하여 스타일을 관리
- **가장 많이 사용**: 유지보수와 확장성이 뛰어나기 때문에 권장
### 🔹 예시
```html
.btn-primary {
  background-color: blue;
  color: white;
}
<button class="btn-primary">클릭1</button>
<button class="btn-primary">클릭2</button>
```
<style>
.btn-primary {
  background-color: blue;
  color: white;
}
</style>

<button class="btn-primary">클릭1</button>
<button class="btn-primary">클릭2</button>
---
<br>

## 2️⃣ id (`#`) 선택자
- **페이지 내에서 단 하나의 요소에만 적용**: 같은 id를 여러 요소에 사용하지 않음
- **특정 요소에만 스타일을 적용할 때 사용**
- **JavaScript와 연동할 때 주로 사용**
### 🔹 예시
```html
#header {
  background-color: gray;
}

<div id="header">헤더 영역</div>
```
<style>
#header {
  background-color: gray;
}
</style>

<div id="header">헤더 영역</div>