# 🎨 Tailwind CSS – State Selector

**State Selector**는 요소의 상태(hover, focus, checked 등)에 따라 스타일을 다르게 지정할 수 있게 해주는 Tailwind CSS의 핵심 기능입니다.  
CSS의 pseudo-class(`:hover`, `:focus`, `:checked`, ...)를 **Tailwind 접두사(prefix)** 형태로 표현합니다.

---

## 1️⃣ 기본 개념

Tailwind에서는 상태 기반 스타일을 **접두사 + 콜론(:)** 형태로 지정합니다.

```html
<button class="hover:bg-blue-500 focus:ring-2">Click me</button>
```

이는 일반 CSS의 아래 코드와 동일합니다.

```css
button:hover { background-color: #3b82f6; }
button:focus { outline: 2px solid #3b82f6; }
```

> **상태 변화 = 접두사로 표현**  
Tailwind는 CSS 상태 선택자를 유틸리티 단위로 추상화합니다.

### 🔹 주요 상태 셀렉터

| 상태                 | Tailwind 접두사  | 설명                                     |
| :----------------- | :------------ | :------------------------------------- |
| `hover:`           | 마우스 오버 상태     | `hover:bg-gray-200`                    |
| `focus:`           | 포커스 되었을 때     | `focus:ring-2 focus:ring-blue-500`     |
| `active:`          | 클릭 중 상태       | `active:scale-95`                      |
| `disabled:`        | 비활성화 상태       | `disabled:opacity-50`                  |
| `checked:`         | 체크박스, 라디오 선택됨 | `checked:bg-blue-500`                  |
| `focus-visible:`   | 키보드로 포커스된 경우  | `focus-visible:ring-2`                 |
| `read-only:`       | 읽기 전용 입력 필드   | `read-only:bg-gray-100`                |
| `required:`        | 필수 입력 필드      | `required:border-red-500`              |
| `invalid:`         | 유효성 검사 실패 상태  | `invalid:border-red-500`               |
| `first:` / `last:` | 목록의 첫/마지막 요소  | `first:rounded-t-lg last:rounded-b-lg` |
| `odd:` / `even:`   | 짝수/홀수 항목      | `odd:bg-gray-50 even:bg-white`         |

---

## 3️⃣ 고급 셀렉터 - `group` & `peer`

### 🧩 `group`

부모 요소에 `group` 클래스를 지정하고, 자식 요소가 `group-hover:` 등의 상태를 감지합니다.

```html
<div class="group">
  <button class="opacity-0 group-hover:opacity-100">보기</button>
</div>
```

- 부모에 마우스를 올리면 자식 버튼이 나타남

---

### 🧩 `peer`

입력요소 상태에 따라 형제 요소 스타일 변경 시 사용합니다.

```html
<label class="inline-flex items-center cursor-pointer">
  <input type="checkbox" class="peer hidden" />
  <span class="w-5 h-5 border border-gray-400 peer-checked:bg-blue-500"></span>
  <span class="ml-2">동의합니다</span>
</label>
```

- 체크 시 `span`의 배경색 변경

---

## 4️⃣ 상태 조합 가능

Tailwind는 여러 상태를 **연속적으로 연결**할 수 있습니다.

```html
<button class="hover:focus:bg-green-500 active:scale-95">
  Save
</button>
```
- 마우스 오버 중이며 포커스일 때만 색상 초록색으로 변경

```html
<input class="focus:invalid:border-red-500" />
```

- 포커스 중이며 invalid일 때만 빨간 테두리.

---

## 5️⃣ 사용 시 유용한 패턴

| 패턴       | 예시                              | 설명                |
| :------- | :------------------------------ | :---------------- |
| 기본 + 상태  | `bg-gray-100 hover:bg-gray-200` | 기본값과 상태를 함께 정의    |
| 부모 상태 반응 | `group-hover:text-blue-500`     | `group` 부모 상태에 반응 |
| 형제 상태 반응 | `peer-checked:bg-green-500`     | 체크 상태에 따라 스타일 변경  |
| 조합 상태    | `hover:focus:bg-red-500`        | 복합 상태 처리 가능       |

---

## 6️⃣ 장단점

### ✅ 장점

- CSS 작성 없이 **상태 기반 스타일링을 유틸리티 단위로 관리**
- **조합 자유도 높음** (`hover:focus:active:` 등)
- **JS 없이도 인터랙션 구현** 가능
- 컴포넌트 단위에서 **명확한 상태 시각화** 가능

### ⚠️ 단점
- 복잡한 상태 조합 시 **클래스가 길어짐**
- 커스텀 애니메이션/조건부 로직은 JS와 병행 필요
- 임의 값(`bg-[url(...)]`)은 정적 문자열만 허용 (JS 변수 불가)

---