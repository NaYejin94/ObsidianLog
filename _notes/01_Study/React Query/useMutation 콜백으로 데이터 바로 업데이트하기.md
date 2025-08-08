
- 📅 학습일: 2025-05-20
- 🧩 관련 태그: #ReactQuery #useMutation 

---

## 📘 주제: useMutation 콜백으로 데이터 바로 업데이트하기

##### invalidateQueryies()

- <mark style="background: #ADCCFFA6;">캐시에 있는 모든 쿼리 혹은 특정 쿼리들을 invalidate(무효화)하는 함수</mark>
- 쿼리를 invalidate하면 해당 쿼리를 통해 받아 온 데이터를 stale time과 상관없이 **무조건 stale 상태로** 만들고, **해당 데이터를 백그라운드에서 refetch하게 된다**.

```jsx
import {useQueryClient} from '@tanstack/react-query'

const queryClient = useQueryClient(); // 쿼리 클라이언트 가져오기

// ....

queryClient.invalidateQueries();
```


##### useMutation() 함수의 콜백 옵션

- `onMutate` 
	- **mutation 함수 실행 직전에 호출**
	- optimistic update(낙관적 UI 업데이트)에 사용
	
- `onSuccess`
	- **mutation 성공 시 호출**
	- 서버 응답을 바탕으로 캐시 갱신이나 알람 표시 등에 사용

- `onError`
	- **mutation 실패 시 호출**
	- onMutate에서 만든 context를 활용할 수 있음

- `onSettled`
	- **성공이든 실패든 무조건 마지막**에 호출
	- 로딩 상태 종료 처리, 공통 cleanup 등에 사용


```jsx
const queryClient = useQueryClient();

// ...

const uploadPostMutation = useMutation({
	mutationFn : (newPost) => uploadPost(newPost),
	onSuccess : () => {
		queryClient.invalidateQueries({queryKey:['posts']})
	}
})
```



##### mutate() 함수의 콜백 옵션

- `onSuccess` / `onError` / `onSettled`
- `useMutation()` 에 등록한 콜백 함수들이 먼저 실행되고, 그 다음에 `mutate()`에 등록한 콜백 함수들이 실행
- useMutation()에 등록된 콜백 함수들은 컴포넌트가 언마운트되더라도 실행되지만,
- <mark style="background: #ADCCFFA6;">mutate()의 콜백 함수들은 뮤테이션이 끝나기 전에 해당 컴포넌트가 언마운트되면 실행되지 않는다</mark>
	- query invalidation과 같이 뮤테이션 과정에서 꼭 필요한 로직은 useMutation()을 통해 등록한다
	- 리다이렉트하거나 결과를 토스트로 띄워주는 것과 같이 <mark style="background: #ADCCFFA6;">해당 컴포넌트에 종속적인 로직은 mutate()를 통해 등록한다</mark>


```jsx
const uploadPostMutation = useMutation({
	mutationFn : (newPost) => uploadpost(newPost),
	onSuccess : () => {
		queryClient.invalidateQueries({queryKey:['posts']});
	},
});

const handleUploadPost = (newPost) => {
	uploadPostMutation.mutate(newPost,{
		onSuccess : () => {
			toast('포스트가 성공적으로 등록되었습니다')
		}
	})
}
```




## ❓ 궁금한 점 / 더 알아보기


## 🔍 참고 자료

