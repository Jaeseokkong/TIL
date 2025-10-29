# ⚛️ Atomic Design

**Atomic Design**은 UI 구성 방법론으로, UI를 "원자(Atom) → 분자(Molecule) → 유기체(Organism) → 템플릿(Template) → 페이지(Page)"의 계층으로 나누어 설계합니다. 이 접근은 재사용성, 일관성, 확장성을 높여 디자인 시스템을 체계적으로 관리하게 해줍니다.

---

## 1️⃣ 계층과 의미

|계층|의미|예시|
|:---|:---|:---|
|**Atom**|UI의 가장 작은 단위. 스타일/기능이 단일 책임인 컴포넌트|Button, Input, Label, Icon|
|**Molecule**|Atoms의 조합으로 하나의 작은 UI 기능을 수행|SearchBar(Input + Button), AvatarWithName|
|**Organism**|여러 Molecule/Atome으로 구성된 독립적 섹션|Header (로고 + 내비 + 검색), CardList|
|**Template**|Oraganism들을 배치한 레이아웃 골격 (콘텐츠는 플레이스홀더)|MainLayout, ArticleTemplate|
|**Page**|실제 데이터를 넣어 완성된 화면|HomePage, ArticlePage|


---

## 2️⃣ 장단점

### ✅ 장점

- **재사용성**: 동일한 Atom을 여러 곳에서 재사용해 일관성 유지
- **유지보수 용이**: 작은 단위로 분리되어 변경 범위가 작음
- **디자인-개발 협업**: 디자이너는 패턴을, 개발자는 컴포넌트를 매핑하기 쉬움
- **확장성**: 새로운 UI를 만들 때 이미 존재하는 컴포넌트 조합으로 빠르게 구성

### ❗ 단점 / 고려사항

- 작은 프로젝트에서는 오버엔지니어링이 될 수 있음
- 처음 구조 설계(폴더/네이밍, 책임 분리)를 잘해야 함
- 모든 UI를 엄격히 나누면 오히려 복잡해질 수 있음 - 실용적인 균형 필요

---

## 3️⃣ 폴더 구조 예시

```bash
src/
└── components/
	├── atoms/
	│ 	├── Button.tsx
	│ 	├── Input.tsx
	│ 	└── Label.tsx
	│
	├── molecules/
	│ 	├── SearchBar.tsx
	│ 	└── LoginForm.tsx
	│
	├── organisms/
	│ 	├── Header.tsx
	│ 	├── CardList.tsx
	│ 	└── Footer.tsx
	│
	├── templates/
	│ 	├── MainLayout.tsx
	│ 	└── ArticleTemplate.tsx
	│
	└── pages/
		├── HomePage.tsx
		└── ArticlePage.tsx
```

---

## 4️⃣ 설계 가이드

- 파일명과 폴더명은 컴포넌트 이름과 일치시켜 관리
- Atom은 프레젠테이셔널 컴포넌트로 작성 (상태(`state`) 없음)
- Molecule/Organism은 내부 상태를 가질 수 있으나, 가능하면 상위에서 props로 제어
- 각 계층은 상위 게층에만 의존하도록 구성 (순환 참조 방지)

---

## 5️⃣ React 예시

```tsx
// 🧩 Atomic Design Example


const Atom = () => <button>Button</button>;


const Molecule = () => (
	<div>
		<input placeholder="Search..." />
		<Atom />
	</div>
);


const Organism = () => (
	<header>
		<h1>MyApp</h1>
		<Molecule />
	</header>
);


const Template = ({ children }: { children: React.ReactNode }) => (
	<div>
		<Organism />
		<main>{children}</main>
	</div>
);


export default function Page() {
return (
	<Template>
		<h2>Home Page</h2>
	</Template>
);
```

- **Atom**: 가장 작은 단위의 UI 요소 (`Button`)
- **Molecule**: Atom을 조합한 작은 기능 단위 (`Input` + `Button`)
- **Oranism**: 여러 Molecule로 구성된 독립 세션 (`Header`)
- **Template**: 페이지의 레이아웃 골격 (`Header` + `Content` 영역)
- **Page**: 실제 데이터를 담아 완성된 화면 (Home Page)

각 단계는 하위 요소를 조합하며 점점 더 복잡한 UI를 형성합니다.

---