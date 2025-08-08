#ReactQuery #QueryStatus #FetchStatus


### 리액트 쿼리의 state

- ==**query status**==
	- 실제로 받아 온 data 값이 있는지 없는지를 나타내는 상태 값

- ==**fetch status**==
	- `queryFn()`함수가 현재 실행되는 중인지 아닌지를 나타내는 상태 값


### Query Status

- `pending`
	- 아직 데이터를 받아오지 못했을 때
- `success`
	- 데이터를 성공적으로 받아왔을 때
- `error`
	- 데이터를 받아오는 중에 에러가 발생했을 때


### Fetch Status
- **쿼리 함수의 실행 상태를 말해주는 값**

- `fetching`
	- 현재 쿼리 함수가 실행되는 중일 때
- `paused`
	- 쿼리 함수가 시작은 했는데 실제로 실행되고 있지 않을 때
	- 보통 네트워크가 오프라인이 된 경우
- `idle`
	- 쿼리 함수가 어떤 작업도 하고 있지 않은 상황

#### 각각의 status의 흐름
![[Pasted image 20250516213616.png]]

- **이상적인 흐름**
	`pending & fetching` **->** `success & idle`
	
- **에러 발생**
	`error & idle`

- **refetch**
	`success & idle` **->** `success & fetching`