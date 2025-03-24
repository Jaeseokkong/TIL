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
