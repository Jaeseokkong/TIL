# 🗂️ Feature-based Folder Structure

프로젝트가 커질수록 컴포넌트, 훅, 유틸리티, 타입 등의 파일 수가 빠르게 증가합니다.

이를 파일 종류별로 관리하면 관련 코드가 여러 디렉터리에 흩어져 유지보수가 어려워질 수 있습니다.

이러한 문제를 해결하기 위한 대표적인 방식이 **Feature-based Folder Structure**입니다.

---

## 1️⃣ Feature-based Folder Structure란?

Feature-based Folder Structure는 기능(Feature)을 기준으로 파일을 구성하는 프로젝트 구조입니다.

즉, 파일의 종류가 아니라 **하나의 기능을 중심으로 관련 파일을 함께 관리**하는 방식입니다.

예를 들어 사용자(Profile) 기능이 있다면 다음과 같이 구성할 수 있습니다.

```bash
profile/
├── ProfileCard.tsx
├── ProfileForm.tsx
├── useProfile.ts
├── api.ts
├── types.ts
└── constants.ts
```

모든 파일이 `profile` 기능과 관련되어 있으므로 하나의 디렉터리에서 관리합니다.

---

## 2️⃣ 파일 종류별 구조와 비교

초기 프로젝트에서는 다음과 같이 파일 종류별로 관리하는 경우가 많습니다.

```bash
components/
hooks/
utils/
types/
api/
```

예를 들어 사용자 정보를 수정하는 기능이라면 관련 파일이 여러 위치에 흩어질 수 있습니다.

```bash
components/
└── ProfileForm.tsx

hooks/
└── useProfile.ts

api/
└── profile.ts

types/
└── profile.ts
```

기능을 수정하려면 여러 디렉터리를 이동해야. 합니다.

---

반면 Feature-based 구조에서는 관련 파일을 하나의 폴더에서 관리합니다.

```bash
profile/
├── ProfileForm.tsx
├── useProfile.ts
├── api.ts
└── types.ts
```

기능 하나를 이해하거나 수정할 때 필요한 파일을 한곳에서 확인할 수 있습니다.

---