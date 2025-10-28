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