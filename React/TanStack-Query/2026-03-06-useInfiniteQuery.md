# 🚚 useInfiniteQuery

`useInfiniteQuery`는 **페이지 기반 서버 데이터를 연속적으로 가져오기 위한 TanStack Query 훅**입니다.

일반 `useQuery`가 **단일 데이터**를 관리한다면<br/>
`useInfiniteQuery`는 **페이지 단위 데이터 리스트를 누적 관리**합니다.

대표적인 사용 예:

- 무한 스크롤
- "더 보기" 버튼
- 댓글 / 피드 / 타임라인
- cursor 기반 pagination

---