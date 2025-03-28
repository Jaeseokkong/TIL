# Virtualization
## 1️⃣ Virtualization(가상화)란?
**Virtualization(가상화)**은 **대량의 UI 요소(리스트, 테이블 등)를 렌더링할 때 필요한 요소만 표시하고 나머지는 제거하는 기법**입니다. 이를 통해 DOM 크기를 줄이고, 성능을 최적화할 수 있습니다.
- **메모리 사용 절감** - 불필요한 요소를 메모리에 올리지 않음
- **렌더링 성능 향상** - 스크롤 시 불필요한 DOM 업데이트 방지
- **Repaint/Reflow 감소** - 브라우저의 성능 부담 최소화

> 💡 **예제 상황**
10,000개의 리스트 아이템을 렌더링하는 경우, Virtualization이 없으면 모든 요소가 DOM에 추가되어 성능이 저하 발생
--- 
<br>

## 2️⃣ Virtualization 기본 개념
### 🔹 기존 방식 vs Virtualization 방식
#### ❌ 기존 방식 (모든 아이템을 렌더링)
```jsx
function List({ items }) {
  return (
    <div>
      {items.map((item) => (
        <div key={item.id}>{item.text}</div>
      ))}
    </div>
  );
}
```
❗ 모든 아이템을 한 번에 렌더링하므로 **메모리 사용량이 증가하고, 성능이 저하**됩니다.

#### ✅ Virtualization 적용 (화면에 보이는 부분만 렌더링)
```tsx
import { FixedSizeList as List } from "react-window";

const Row = ({ index, style }) => (
  <div style={style}>📌 Item {index}</div>
);

function VirtualizedList() {
  return (
    <List height={400} itemCount={1000} itemSize={50} width={300}>
      {Row}
    </List>
  );
}
```
✔️ `react-window`를 사용하여 **스크롤 영역 내의 요소만 렌더링**하여 성능을 최적화됩니다.
- - -
<br>

### 3️⃣ Virtualization 라이브러리
|라이브러리|특징|
|---|---|
|react-window|가볍고 성능이 뛰어난 Virtualization 라이브러리|
|react-virtualized|테이블, 리스트 등 다양한 UI 가상화 지원|
|TanStack Virtual|최신 Virtualization 라이브러리로, 유연성이 뛰어남|