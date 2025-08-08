## ✅ 1. `fetch` 사용의 장점과 추천 상황

### 🔸 장점

|항목|설명|
|---|---|
|**브라우저 내장 API**|추가 설치 없이 바로 사용 가능|
|**가볍고 단순함**|불필요한 기능 없이 순수하게 HTTP 요청만 가능|
|**유연함**|요청을 커스터마이징하기 쉬움 (raw request 구성에 유리)|

### 🔸 추천 상황

- **작은 프로젝트**에서 의존성을 줄이고 싶을 때
    
- 브라우저 기본 API만 사용해서 동작해야 하는 **환경 제약**이 있을 때
    
- 응답 포맷이 다양하거나 아주 세세하게 커스터마이징해야 할 때 (예: 이미지, blob 등)
    

### 🔸 예시 코드
`const fetchExperiences = async () => {   const res = await fetch('/api/experiences');   if (!res.ok) throw new Error('네트워크 오류');   const data = await res.json();   return data; };`

---

## ✅ 2. `axios` 사용의 장점과 추천 상황

### 🔸 장점

|항목|설명|
|---|---|
|**자동 JSON 변환**|응답을 자동으로 파싱해줌 (`res.data`)|
|**HTTP 에러 자동 throw**|응답 상태가 400 이상이면 자동으로 예외 발생|
|**기본 설정, 인스턴스화**|baseURL, headers 등을 공통 설정으로 관리 가능|
|**인터셉터 지원**|요청/응답 가로채기 기능 → 토큰 자동 첨부 등 가능|
|**타입스크립트 친화적**|타입 지정, 응답 타입 추론 등 더 편리함|

### 🔸 추천 상황

- **중~대규모 프로젝트**에서 유지보수성을 높이고 싶을 때
    
- **API 엔드포인트가 많고 반복적인 설정**이 필요한 경우
    
- **인증, 로깅, 에러 핸들링** 같은 공통 로직이 필요한 경우
    
- 팀 프로젝트에서 **공통 axios 인스턴스를 통한 일관성**을 주고 싶을 때
    

### 🔸 예시 코드
`import axios from 'axios';  const api = axios.create({   baseURL: '/api',   headers: { 'Content-Type': 'application/json' }, });  const fetchExperiences = async () => {   const { data } = await api.get('/experiences');   return data; };`

---

## ✅ 요약 비교표

|항목|fetch|axios|
|---|---|---|
|**설치 필요 여부**|❌ (브라우저 내장)|✅ (npm 설치 필요)|
|**사용 간편성**|기본적이지만 verbose|더 간결하고 사용 편리|
|**기본 설정**|없음, 수동 설정 필요|`axios.create()`로 공통 설정|
|**자동 JSON 처리**|❌ 수동으로 `.json()` 호출|✅ 자동 처리 (`res.data`)|
|**에러 처리**|수동으로 `res.ok` 체크|자동으로 예외 발생|
|**인터셉터 지원**|❌|✅ 요청/응답 가로채기 가능|
|**TypeScript 지원**|가능하지만 수동 설정 많음|친화적, 제네릭 타입 활용 쉬움|

---

## ✅ 어떤 걸 쓰면 좋을까?

|상황|추천|
|---|---|
|브라우저 환경에서 간단한 요청만 필요할 때|`fetch`|
|API 수가 많고 공통 설정이 필요한 프로젝트일 때|`axios`|
|팀 협업 프로젝트에서 API 일관성을 유지하고 싶을 때|**`axios` 강력 추천**|
|React Query, Zustand 등과 함께 조합해서 쓸 때|`axios`가 더 편함|
