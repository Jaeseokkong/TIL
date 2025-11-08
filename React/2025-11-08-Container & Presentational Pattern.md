# 🧩 Container & Presentational Pattern

**🧩 Container & Presentational Pattern**은 React에서 컴포넌트의 **역할을 분리**하여 코드의 **재사용성, 가독성, 테스트 용이성**을 높이는 설계 패턴입니다.

---

## 1️⃣ 개념 요약
| 구분                           | 역할    | 책임                        | 예시                               |
| :--------------------------- | :---- | :------------------------ | :------------------------------- |
| **Presentational Component** | UI 담당 | 스타일, 구조, props로 받은 데이터 표시 | Button, Card, List               |
| **Container Component**      | 로직 담당 | 상태 관리, 데이터 fetch, 이벤트 처리  | UserListContainer, FormContainer |

---

> 💡 핵심: **Container는 “어떻게 동작할지”**, **Presentational은 “어떻게 보일지”** 만 담당한다.