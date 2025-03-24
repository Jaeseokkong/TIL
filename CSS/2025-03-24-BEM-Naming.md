# BEM 명명법
## 1️⃣ BEM이란?
**BEM(Block, Element, Modifier)**은 CSS 클래스 네이밍 방법론 중 하나로, **유지보수성과 재사용성**을 높이기 위해 고안된 규칙입니다. BEM을 사용하면 CSS 구조가 명확해지고, 코드의 일관성을 유지하는데 도움이 됩니다.
- - -
<br>

## 2️⃣ BEM의 구성 요소
BEM은 세 가지 주요 개념으로 구성됩니다.
### 🔹 Block (블록)
- 독립적인 UI 요소를 의미
- 예: 버튼, 네비게이션 바, 카드 등
- **클래스명 예시**: `.button`, `.navbar`, `.card`

### 🔹 Element (요소)
- Block의 일부로 존재하는 구성 요소
- Block이 없으면 의미를 가질 수 없는 하위 요소
- **클래스명 예시**: `.button__icon`, `.card__title`, `.navbar__item`

### 🔹 Modifier (수정자)
- Block이나 Element의 상태나 변형을 나타냄
- 스타일이나 동작이 다를 때 사용
- **클래스명 예시**: `.button--primary`, `.card--large`, `.navbar__item--active`
- - -
<br>

## 3️⃣ BEM 네이밍 규칙
BEM은 특정 네이밍 규칙을 따릅니다.
- **Block과 Element는 `__`(더블 언더스코어)로 구분**
- **Modifier는 `--`(더블 하이픈)으로 구분**

### 🔹 예제 코드
```html
<button class="button button--primary">
  <span class="button__icon"></span>
  Click me
</button>

<style>
.button {
  background-color: gray;
  color: white;
  padding: 10px 20px;
  border: none;
}

.button__icon {
  margin-right: 5px;
}

.button--primary {
  background-color: blue;
}
</style>
```
<button class="button button--primary">
  <span class="button__icon"></span>
  Click me
</button>

<style>
.button {
  background-color: gray;
  color: white;
  padding: 10px 20px;
  border: none;
}

.button__icon {
  margin-right: 5px;
}

.button--primary {
  background-color: blue;
}
</style>

---
<br>

## 4️⃣ BEM의 장점 및 주의점
### 🔹 장점
✅ **명확한 구조**: 코드의 역할이 직관적으로 보임  
✅ **재사용성 증가**: 요소를 쉽게 재사용할 수 있음  
✅ **유지보수 용이**: 협업할 때 가독성이 뛰어남  
✅ **CSS 우선순위 문제 감소**: 깊은 선택자를 피할 수 있음

### 🔹 사용 시 주의할 점
- 너무 길거나 복잡한 네이밍을 피해야 함
- Block은 최대한 독립적으로 설계해야 함
- 불필요한 Modifier 사용을 줄이는 것이 좋음
---
<br>

## 🎯 결론
BEM은 체계적인 CSS 작성을 도와주는 강력한 네이밍 방법론입니다. 특히 규모가 커지는 프로젝트에서 유용하며, 일관성있는 코드 작성을 위해 널리 사용됩니다.  
앞으로 BEM 네이밍 방법론을 프로젝트에 적극 활용해보도록 하겠습니다!🚀