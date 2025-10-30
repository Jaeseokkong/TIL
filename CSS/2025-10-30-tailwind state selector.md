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