# Tailwind 기본 이해
## 1️⃣ Tailwind란?
Tailwind CSS는 **유틸리티 퍼스트(Utility-First) 프레임워크**로, HTML 클래스만으로 스타일을 빠르게 적용할 수 있는 CSS 프레임워크입니다.

## 2️⃣ 기존 CSS/프레임워크와의 차이점
### 🔹 일반 CSS (Vanilla CSS)
- CSS 파일에서 클래스를 만들어 사용
- 유지보수가 어렵고, 클래스가 많아질수록 복잡해짐

### 🔹 Bootstrap 등의 CSS 프레임워크
- 미리 정의된 UI 컴포넌트 제공 (예: `.btn-primary`, `.card` 등)
- 커스터마이징이 어렵거나 별도의 CSS 수정이 필요함

### 🔹 Tailwind CSS (유틸리티 퍼스트 방식)
- **기능별로 잘게 나뉜 클래스**(`textcenter`, `bg-blue-500`, `flex` 등)를 조합해 사용
- CSS 파일 없이 HTML에서 바로 스타일 적용 가능
- 유지보수가 쉽고, 반복적인 CSS 작성이 줄어듦

#### 🧐 Tailwind의 유틸리티 클래스 예제
```html
<!-- 일반적인 CSS 방식 -->
<div class="card">
  <h2 class="title">Hello, Tailwind!</h2>
</div>

.card {
  background-color: #f3f4f6;
  padding: 16px;
  border-radius: 8px;
}
.title {
  font-size: 24px;
  font-weight: bold;
  text-align: center;
}

<!-- Tailwind 방식 -->
<div class="bg-gray-100 p-4 rounded-lg">
  <h2 class="text-xl font-bold text-center">Hello, Tailwind!</h2>
</div>
```
✔️ 별도의 CSS 없이 **HTML에서 바로 스타일을 적용**할 수 있어 편리합니다.

<br>

- - -

<br>

## 3️⃣ 실습: Tailwind Play에서 테스트하기
Tailwind Play를 이용하면 설치 없이 실시간으로 Tailwind 클래스를 실습할 수 있음.

[Tailwind Play](https://play.tailwindcss.com/) 

```html
<div class="flex items-center justify-center h-screen bg-gray-200">
  <button class="px-6 py-3 bg-blue-500 text-white font-semibold rounded-lg shadow-md hover:bg-blue-600">
    Click Me
  </button>
</div>
```
✔️ `flex`, `items-center`, `justify-center` 등을 조합해서 **레이아웃 정렬**을 쉽게 설정  
✔️ `hover:bg-blue-600`으로 호버 효과 추가  
✔️ 일반 CSS보다 **더 직관적이고 간결하게** 스타일 
 