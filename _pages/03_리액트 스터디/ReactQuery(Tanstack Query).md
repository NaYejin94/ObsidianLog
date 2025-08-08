1. 소개

- 상태관리 라이브러리의 하나임 공식 사이트-[TanStack Query](https://tanstack.com/query/latest)
- 상태관리는 크게 클라이언트 서버 상태관리가 있는데 리액트 쿼리는 서버상태 관리 라이브러리
- 리덕스는 서버상태 클라이언트 상태 다 가능하지만 애매하며 클라이언트로만 쓰라고 권장함

1. 장점
    
    →한줄요약: 서버상태 라이브러리는 그냥 이거 쓰세요
    

- 편함(당연히 라이브러리를 가져다 쓰는거니까 편할수밖에 없음)
    
- 코드가 단순해짐
    
- api 호출 통제가 가능함
    
- 캐쉬관리 호출한 데이터를 캐쉬에 저장할 수 있음
    
    ```
    → 쓸모없는 캐쉬 데이터를 알아서 수거함(가비지 컬렉션처럼 )
    ```
    

```
    gcTime:5000,//5초뒤 캐쉬 데이터 수거
    //설정하지 않으면 기본값은 5분임 

```

- 코드 예시
    
    ```jsx
    import React from 'react';
    import { useQuery, QueryClient, QueryClientProvider } from '@tanstack/react-query';
    
    // API 요청 함수
    const fetchUserData = async () => {
      const response = await fetch('<https://jsonplaceholder.typicode.com/users>');
      if (!response.ok) throw new Error('Network response was not ok');
      return response.json();
    };
    
    const Users = () => {
      // React Query useQuery 훅을 사용하여 데이터를 가져오고, 자동으로 캐시 관리
      const { data, error, isLoading } = useQuery(['users'], fetchUserData, {
        // 캐시 데이터를 5분간 유지
        cacheTime: 5 * 60 * 1000,
        // 데이터가 변경되었을 때 재요청할 수 있도록 설정
        refetchOnWindowFocus: false,  // 윈도우 포커스 시 자동으로 refetch를 막음
      });
    
      if (isLoading) return <div>Loading...</div>;
      if (error) return <div>An error occurred: {error.message}</div>;
    
      return (
        <div>
          <h1>User List</h1>
          <ul>
            {data.map((user) => (
              <li key={user.id}>{user.name}</li>
            ))}
          </ul>
        </div>
      );
    };
    
    const App = () => {
      const queryClient = new QueryClient();  // QueryClient 생성
    
      return (
        <QueryClientProvider client={queryClient}>
          <Users />
        </QueryClientProvider>
      );
    };
    
    export default App;
    ```
    

1. 캐쉬관리

- 그래서 캐쉬관리해서 데이터를 캐쉬에 저장하면 뭐가좋은데??
    
    → 사이트를 옮겨다닐때 다시 api를 호출해서 데이터를 불러오는데 캐쉬에 저장된 데이터가 있으면 일단 캐시된 데이터를 불러와서 보여주기 때문에 로딩을 안보여줌(뒤에선 api 호출)
    
    → api호출 완료가 되면 다시 캐시 업데이트
    

1. api 호출통제

```jsx
const { isLoading, data, isError, error, refetch } = useQuery({
    queryKey: ["posts"],
    queryFn: fetchPost,
    //<<-- 여기에 집어넣고있다고 보면 됨
  });
  console.log("ddd", data, isLoading);
  console.log("error", isError, error);
 
 
StaleTime:60000,//1분뒤 호출 
//1분중에 서버 데이터가 바뀌면 어떻게되나요?? -> 그래도 1분뒤에 호출됨...
//말그대로api 호출을 하지 않기 때문 캐시에 있는 데이터를 가져다 씀
//오래된 데이터를 봐도 괜찮다 할때 사용 
//staleTime이 길고 gcTime이 짧으면 누가이기나요?? -> gcTime이 이김
//60초 지속 10초 수거로 설정하면 staletime도 여지없이 50초가 남았음에도
//api호출 해야함.. 그렇지만 이런식으로 하면 안되기 때문에 staleTime을 더 길게 설정해야함

refetchInterval: 3000, // 3초마다 호출
refetchOnMount:false//홈페이지 "다시" 오면 호출 안함 true면 매번 호출
refetchOnWindowFocus:true // 창을 되돌아오면 다시 호출 유튜브 보다가 B다시 볼 때 
//혹은 게임하다가 다시 B 홈페이지 볼 때 실시간 데이터가 필요할 때 주식창 채팅창 등

<button onClick={refetch}>버튼클릭</button> // refetch: 버튼을 누를 때 마다 호출
enabled:false // 맨처음에 호출 안하고 버튼을 클릭했을 때 호출하는 법 
//enabled를 사용할 땐 data?<<-- 를 써야함 안하면 데이터가 없기 때문에 오류
//검색을 할 때 주로 사용함 조건식은 보통 이것을 사용 기본값은 true로 되어있음 

```

외 내용 참고: [useQuery | TanStack Query React Docs](https://tanstack.com/query/latest/docs/framework/react/reference/useQuery#usequery)

1. 훅 만들어서 모듈 분리

실제로는 한 파일에 다 집어넣으면 보기 힘들어지니까 모듈을 분리해야함

```jsx
 const fetchUserData = async () => {
  const response = await fetch('<https://jsonplaceholder.typicode.com/users>');
  if (!response.ok) throw new Error('Network response was not ok');
  return response.json();
};

const Users = () => {
  // React Query useQuery 훅을 사용하여 데이터를 가져오고, 자동으로 캐시 관리
  const { data, error, isLoading } = useQuery(['users'], fetchUserData, {
    // 캐시 데이터를 5분간 유지
    cacheTime: 5 * 60 * 1000,
    // 데이터가 변경되었을 때 재요청할 수 있도록 설정
    refetchOnWindowFocus: false,  // 윈도우 포커스 시 자동으로 refetch를 막음
  });

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>An error occurred: {error.message}</div>;

  return (
    <div>
      <h1>User List</h1>
      <ul>
        {data.map((user) => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
};
```

- 파일구조
    
    src/ ├── api/ │ └── userApi.js // API 요청 함수 ├── components/ │ └── Users.js // Users 컴포넌트 └── App.js // 전체 애플리케이션
    
- userApi.js
    
    ```jsx
    // src/api/userApi.js
    
    export const fetchUserData = async () => {
      const response = await fetch('<https://jsonplaceholder.typicode.com/users>');
      if (!response.ok) throw new Error('Network response was not ok');
      return response.json();
    };
    ```
    
- user.js
    
    ```jsx
    // src/components/Users.js
    import React from 'react';
    import { useQuery } from '@tanstack/react-query';
    import { fetchUserData } from '../api/userApi';  // API 함수 가져오기
    
    const Users = () => {
      const { data, error, isLoading } = useQuery(['users'], fetchUserData, {
        cacheTime: 5 * 60 * 1000,
        refetchOnWindowFocus: false,
      });
    
      if (isLoading) return <div>Loading...</div>;
      if (error) return <div>An error occurred: {error.message}</div>;
    
      return (
        <div>
          <h1>User List</h1>
          <ul>
            {data.map((user) => (
              <li key={user.id}>{user.name}</li>
            ))}
          </ul>
        </div>
      );
    };
    
    export default Users;
    ```
    
- app.js
    

```jsx
// src/App.js
import React from 'react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import Users from './components/Users';

const queryClient = new QueryClient();

const App = () => {
  return (
    <QueryClientProvider client={queryClient}>
      <Users />
    </QueryClientProvider>
  );
};

export default App;
```

- 0. 리액트 쿼리를 왜 알아야 하는가?
    
    ![GXMFCI5bsAAzFiQ.jpg](attachment:68f3a4b7-7abd-47c2-848c-92215767f6f3:GXMFCI5bsAAzFiQ.jpg)
    
    ![사다리.png](attachment:722e1bec-4dba-4092-bb94-de7b7760c1af:%EC%82%AC%EB%8B%A4%EB%A6%AC.png)
    
    ![취업이힘듦.png](attachment:5c328dce-7e8b-4d19-9cb3-c4b9a55a139d:%EC%B7%A8%EC%97%85%EC%9D%B4%ED%9E%98%EB%93%A6.png)
    
    - 리액트 쿼리 툴 환경
        
        ![스크린샷 2025-03-24 오후 2.16.47.png](attachment:809c50b6-f195-4562-96fd-21acdec952b5:%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-03-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.16.47.png)
        
        ![스크린샷 2025-03-24 오후 2.16.34.png](attachment:2760bd33-b2c0-4293-92c0-f97f8740886d:%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-03-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.16.34.png)
        
    - 사용 할 수도 안할수도 있음
        
        "서버는 **stateless(상태가 없게끔)** 짜야 한다"는 말은,
        
        **서버가 클라이언트의 상태를 직접 유지하지 않고, 매 요청마다 독립적으로 처리해야 한다**는 뜻이에요.
        
        ---
        
        ## ✅ **Stateless 서버란?**
        
        **Stateless(무상태)** 서버는 **각 요청(request)이 독립적으로 처리되며, 서버가 클라이언트의 상태를 저장하지 않는 구조**입니다.
        
        즉, 서버는 **"이전 요청이 무엇이었는지 기억하지 않고"** 매번 새로운 요청으로 간주합니다.
        
        ### 🔹 예제 (Stateless 방식)
        
        1. 클라이언트가 `GET /user/1` 요청을 보냄
        2. 서버는 DB에서 사용자 데이터를 가져와 응답
        3. 다음 요청에서 서버는 **이전 요청을 기억하지 않고**, 동일한 방식으로 처리
        
        🚀 **이 방식의 장점:**
        
        - 서버 확장(Scalability)이 쉬움 → 여러 서버로 로드 밸런싱 가능
        - 세션 정보 저장 불필요 → 서버 리소스 절약
        - 요청이 독립적이므로 유지보수가 용이
        
        ---
        
        ## ✅ **Stateful(상태 유지) 서버와의 차이**
        
        반대로 **Stateful(상태 유지) 서버**는 **클라이언트의 상태를 서버가 직접 저장하는 방식**입니다.
        
        예를 들어, **로그인 세션을 서버에서 직접 관리하는 경우**가 이에 해당해요.
        
        ### 🔹 예제 (Stateful 방식)
        
        1. 클라이언트가 로그인하면 서버가 세션을 저장
        2. 이후 요청에서는 세션 정보를 기반으로 클라이언트를 구분
        3. 서버가 세션을 유지해야 하므로, 확장성이 떨어질 수 있음
        
        🚀 **Stateful 방식의 문제점:**
        
        - 서버가 클라이언트 정보를 유지해야 해서 **서버 부하 증가**
        - 여러 서버가 존재할 경우 **로드 밸런싱이 어려움**
        
        ---
        
        ## ✅ **그렇다면 상태는 어디에 저장해야 할까?**
        
        "서버는 상태가 없게끔 짜야 한다"는 말은,
        
        👉 **서버가 직접 상태를 저장하는 것이 아니라, 데이터베이스(DB)나 캐시(Redis 등)를 활용해야 한다**는 의미예요.
        
        ✔ **React Query 같은 상태관리 라이브러리는 클라이언트 측에서 서버 데이터를 캐싱하고 동기화하는 역할**을 하기 때문에,
        
        ✔ 서버가 굳이 상태를 관리할 필요 없이, **클라이언트가 필요할 때마다 API 요청을 통해 최신 상태를 가져올 수 있음**
        
        💡 즉, **"상태(State)는 서버가 아니라 DB에서 관리하고, 서버는 요청을 받아 처리하는 역할만 수행해야 한다!"** 라는 의미입니다. 🚀
        
        ![image.png](attachment:1c53be6a-acaa-4b3c-8afb-1d99b7a2c478:image.png)
        
        ![image.png](attachment:44e42372-cbdd-47a1-a73b-6e27bb008be9:image.png)
        
        ![image.png](attachment:08acde3d-0bf5-499d-aca4-d5b1690dc544:image.png)