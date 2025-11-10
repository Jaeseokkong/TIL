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