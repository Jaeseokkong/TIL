# 🪬 prefetchQuery

TanStack Query의 `prefetchQuery`는 **데이터를 미리 fetch해서 캐리에 저장하는 기능**입니다.

UI에서 바로 사용하지는 않지만 이후 동일한 `queryKey`로 `useQuery`를 호출하면 **즉시 캐시 데이터를 사용 가능**합니다.

---