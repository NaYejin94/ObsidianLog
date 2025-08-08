#ReactQuery #query #useQuery


### 쿼리란?

- 데이터베이스 같은 것에 필요한 데이터를 요청하는 것

### useQuery()

- 필요한 데이터를 백엔드에 요청해서 받아오는 React Hook
- 서버에서 데이터를 조회할 때 사용하는 훅
- API로부터 데이터를 가져온다
- 로딩 중 상태, 에러 상태, 성공 상태를 관리해준다
- 결과를 자동으로 캐시하고 재요청 타이밍을 제어할 수 있다.

```jsx
function Users() {
	const {data, isLoading, error} = useQuery({
		queryKey : ['users'],
		queryFn : fetchUsers
	})
}
```


### useQuery() 리턴 값

- **data**
	- 백엔드에서 받아온 데이터
	- results에 실제 포스트 데이터가 배열로 들어가 있음

- **dataUpdatedAt**
	- 데이터를 받아온 시간
	- 이 시간을 기준으로 언제 데이터를 refetch 할 것인지 등을 정함

