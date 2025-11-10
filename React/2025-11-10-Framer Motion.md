# 🎞️ Framer Motion

**Framer Motion**은 React 기반의 강력한 애니메이션 라이브러리로,  
CSS 트랜지션보다 **자연스럽고 선언적인 애니메이션 제어**를 가능하게 합니다.  
단순한 페이드인부터, 조건부 렌더링 시 화면 전환, 제스처 반응까지 모두 손쉽게 구현할 수 있습니다.

---

## 1️⃣ 주요 개념

|개념|설명|예시|
|:---|:---|:---|
|**motion**|애니메이션 가능한 컴포넌트를 생성하는 래퍼|`motin.div`, `motion.button`|
|**variants**|여러 상태(초기, 실행, 종료 등)에 따른 애니메이션 정의|`initial`, `animate`, `ext`|
|**AnimatePresence**|조건부 렌더링 시 컴포넌트의 **퇴장 애니메이션** 지원|단계 전환, 모달 닫기|
|**transition**|애니메이션의 지속 시간, 가속도 곡선 등을 제어|`duration`, `ease`, `type`|
|**whileHover** / **whileTap**|사용자 인터랙션(hover, click)에 따른 즉시 애니메이션|버튼 반응 효과|
|**layout**|DOM 변경 시 레이아웃 이동을 부드럽게 보정|리스트 재정렬, 스텝 전환|

---

## 2️⃣ 기본 문법

```tsx
import { motion } from 'framer-motion';

export default function Example() {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}   // 처음 상태
      animate={{ opacity: 1, y: 0 }}    // 애니메이션 도착 상태
      transition={{ duration: 0.5 }}     // 전환 시간
    >
      Hello Motion 👋
    </motion.div>
  );
}
```

> ✅ motion.div는 div와 동일한 역할을 하지만,  
props로 애니메이션 속성을 직접 부여할 수 있습니다.

---

## 3️⃣ Variants 패턴
`variants`는 여러 애니메이션 상태를 **객체로 선언**하고,  
컴포넌트 간 **재사용**하거나 **조건별 전환**을 쉽게 관리할 수 있습니다.

```tsx
const variants = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 },
  exit: { opacity: 0, y: -20 },
};

<motion.div
  variants={variants}
  initial="hidden"
  animate="visible"
  exit="exit"
  transition={{ duration: 0.4 }}
>
  Fade In & Out
</motion.div>
```

> 💡 `variants`를 쓰면 상태별 스타일을 깔끔하게 구분할 수 있어,  
“페이지 전환”이나 “리스트 항목 순차 등장”에 특히 유용합니다.

---

