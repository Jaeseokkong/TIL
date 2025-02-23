# React Query 고급 기능 및 실전 활용
## 1️⃣ React Query 고급 기능
### 🔹 Dependent Query (의존서 쿼리)
```tsx
const { data: user } = useQuery({ 
  queryKey: ["user"], 
  queryFn: fetchUser
});
const { data: orders } = useQuery({
  queryKey: ["orders", user?.id],
  queryFn: fetchOrders,
  enabled: !!user?.id
})
```
✔️ `enable` 옵션을 사용하여 특정 데이터가 준비될 때까지 대기

<br>

### 🔹 Optimistic Update (낙관적 업데이트)
```tsx
const mutaion = useMutation({
  mutationFn: postData,
  onMutate: async (newData) => {
    await queryClient.cancelQueries({ queryKey: ["myData" ]});
    const previousData = queryClient.getQueryData(["myData"]);
    queryClient.setQueryData(["mydata"], (old) => [...old, newData]);
    return { previousData };
  },
  onError: (err, newData, context) => {
    queryClient.setQueryData(["myData"], context.previousData);
  }
})
```
✔️ UI를 즉시 업데이트한 후 서버 응답에 따라 롤백 가능