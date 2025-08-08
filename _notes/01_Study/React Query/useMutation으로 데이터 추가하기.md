
- 📅 학습일: 2025-05-20
- 🧩 관련 태그: #ReactQuery #useMutation

---

## 📘 주제: useMutation으로 데이터 추가하기


##### useMutation

- **mutation** : 사이드 이펙트(데이터베이스에 새로운 값을 추가하거나 수정, 삭제하는 행위)를 가진 함수
- 사이트 이펙트가 발생하는 경우 -> `useMutation()` 사용

- `useQuery()`의 쿼리함수는 <mark style="background: #ADCCFFA6;">컴포넌트가 마운트되면서 자동으로 실행</mark>되지만,
- `useMutation()`은 <mark style="background: #ADCCFFA6;">실제로 뮤테이션하는 함수를 직접 실행</mark>해 줘야 한다.
	- `mutate()` 함수를 통해 `mutationFn`으로 등록했던 함수를 실행 할 수 있음
	- `mutate()`를 하면 백엔드 데이터는 변경되지만, 캐시에 저장된 데이터는 그대로 이기 때문에<mark style="background: #ADCCFFA6;">`refetch`를 해줘야 변경된 데이터가 화면에 보여진다</mark>


##### useMutation() 으로 데이터 추가하기
```jsx
const uploadPostMutation = useMuataion({
	mutationFn : (newPost) => uploadpost(newPost),
});

const handleSubmit = (e) => {
	e.preventDefault();
	const newPost = {usename:'codeit', content};
	uploadPostMutation.mutate(newPost);
	setContent('');
}
```


## ❓ 궁금한 점 / 더 알아보기


## 🔍 참고 자료

- 사이드 이펙드와 useEffect
https://ko.react.dev/learn/keeping-components-pure#side-effects-unintended-consequences