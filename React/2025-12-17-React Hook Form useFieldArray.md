# 📋 React Hook Form — useFieldArray 정리

`useFieldArray`는 **동적으로 변한는 폼 필드 리스트**를 관리하기 위한 React Hook Form의 핵심 API입니다.  
단일 input을 다루는 것과 달리, 배열 형태의 필드를 추가/삭제/정렬해야 하는 경우에 사용합니다.

예를 들면 다음과 같은 상황입니다:

- 주소를 여러 개 추가하는 폼
- 상품 옵션을 동적으로 추가/삭제
- 스케줄, 멤버, 연락처 리스트
- 장바구니 아이템 목록

---

## 1️⃣ 왜 `useFieldArray`가 필요한가?

일반적인 폼은 필드 구조가 **정적**입니다.

```tsx
<input {...register("name")} />
<input {...register("email")} />
```

하지만 동적 폼은 **필드 개수와 구조가 런타임에 변합니다.**

```tsx
addresses[0].street
addresses[1].street
addresses[2].street
```

이걸 `useState`로 관리하면:

- index 꼬임
- key 관리
- validation 연동
- 에러 동기화

등이 매우 복잡해집니다.

✔️` useFieldArray`는 이 문제를 React Hook Form 내부 상태와 완전히 동기화된 방식으로 해결합니다.

---