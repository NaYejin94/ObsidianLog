## ☑️ 사용자 정의 훅(user custom hooks)

서로 다른 컴포넌트 내부에서 **같은 로직을 공유**하고자 할 때 주로 사용되는 커스텀 훅

hook이기 때문에 반드시 `use`로 시작하는 이름이어야 한다. 아래는 react 공식 문서에 나온 Hook 네이밍 정의다.

![image.png](attachment:30219e8f-b695-42d4-809f-e0c95863452f:image.png)

## ☑️ `use`를 사용하지 않으면 어떻게 될까?

1. **함수가 어떤 기능을 제공하는 지를 판단할 수 없다.**
    1. `use`를 사용하지 않는다면 앞글자를 컴포넌트처럼 대문자로 활용해야 한다. 예를 들어 `useFetch`에서 `use`를 사용하지 않고 `Fetch`만 보여준다면 어떨까? 이건 컴포넌트인지 훅인지 구분하기가 어려울 것이다.
2. **`react-hooks/rules-of-hooks`가 허락하지 않는다.**
    1. 1번의 경우를 회피하기 위해 `Fetch`가 아닌 `fetch`로 네이밍을 하면 어떨까? 에러가 발생한다. 훅은 함수 컴포넌트 내부 또는 사용자 정의 훅 내부에서만 사용할 수 있는데, 이를 제대로 전달하는 매개체가 아래의 방법이다.
        1. `fetch` 앞부분을 대문자로 바꿔 함수 컴포넌트라고 알린다.
        2. `use`를 붙여 커스텀 훅임을 알려주는 방법이다.

## ☑️ 코드 예제

아래의 코드는 API 요청을 수행하고 데이터를 관리하는 로직이다.

- 사용자 정의 훅을 사용하지 않았을 때
    
    ```jsx
    import React, { useEffect, useState } from "react";
    
    const UserList = () => {
      const [users, setUsers] = useState([]);
      const [loading, setLoading] = useState(true);
    
      useEffect(() => {
        fetch("<https://jsonplaceholder.typicode.com/users>") //users API 호출
          .then((response) => response.json())
          .then((data) => {
            setUsers(data);
            setLoading(false);
          })
          .catch((error) => {
            console.error("Error fetching users:", error);
            setLoading(false);
          });
      }, []);
    
      if (loading) return <p>Loading...</p>;
    
      return (
        <ul>
          {users.map((user) => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      );
    };
    
    const PostList = () => {
      const [posts, setPosts] = useState([]);
      const [loading, setLoading] = useState(true);
    
      useEffect(() => {
        fetch("<https://jsonplaceholder.typicode.com/posts>") //posts API 호출, 동일 로직 중복
          .then((response) => response.json())
          .then((data) => {
            setPosts(data);
            setLoading(false);
          })
          .catch((error) => {
            console.error("Error fetching posts:", error);
            setLoading(false);
          });
      }, []);
    
      if (loading) return <p>Loading...</p>;
    
      return (
        <ul>
          {posts.map((post) => (
            <li key={post.id}>{post.title}</li>
          ))}
        </ul>
      );
    };
    
    const App = () => (
      <div>
        <h2>Users</h2>
        <UserList />
        <h2>Posts</h2>
        <PostList />
      </div>
    );
    
    export default App;
    
    ```
    

위 코드의 경우 API를 호출 하는 과정에서 2회의 **코드 중복**이 발생하게 되는데, 프로젝트 볼륨이 커질 경우 코드가 **반복** 된다면 **유지 보수에 어려움**을 겪을 확률이 높다.

- 사용자 정의 훅을 사용했을 때
    
    ```jsx
    import React, { useEffect, useState } from "react";
    
    //커스텀 훅 정의
    const useFetchData = (url) => {
      const [data, setData] = useState([]);
      const [loading, setLoading] = useState(true);
    
      useEffect(() => {
        fetch(url)
          .then((response) => response.json())
          .then((data) => {
            setData(data);
            setLoading(false);
          })
          .catch((error) => {
            console.error("Error fetching data:", error);
            setLoading(false);
          });
      }, [url]);
    
      return { data, loading };
    };
    
    const UserList = () => {
      const { data: users, loading } = useFetchData("<https://jsonplaceholder.typicode.com/users>");
    
      if (loading) return <p>Loading...</p>;
    
      return (
        <ul>
          {users.map((user) => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      );
    };
    
    const PostList = () => {
      const { data: posts, loading } = useFetchData("<https://jsonplaceholder.typicode.com/posts>");
    
      if (loading) return <p>Loading...</p>;
    
      return (
        <ul>
          {posts.map((post) => (
            <li key={post.id}>{post.title}</li>
          ))}
        </ul>
      );
    };
    
    const App = () => (
      <div>
        <h2>Users</h2>
        <UserList />
        <h2>Posts</h2>
        <PostList />
      </div>
    );
    
    export default App;
    
    ```
    

사용자 훅을 사용하면 코드 재사용성 증가에 따라 유지 보수가 쉬워지고, 코드 가독성이 올라간다. 추가적으로 의존성을 **숨길 수 있다.**

![image.png](attachment:06675fd1-09a0-404c-bdc7-03ebfce7776d:image.png)

> Effect를 작성하면 린터는 Effect의 의존성 목록에 Effect가 읽는 모든 반응형 값(예를 들어 props 및 State)을 포함했는지 확인합니다. 이렇게 하면 Effect가 컴포넌트의 최신 props 및 State와 동기화 상태를 유지할 수 있습니다. 불필요한 의존성으로 인해 Effect가 너무 자주 실행되거나 무한 루프를 생성할 수도 있습니다. - react 공식 문서 -

커스텀 Hook을 추출하는 것은 데이터 흐름을 명확하게 해준다. 컴포넌트 안의 Effect(부수 효과)를 "숨김으로써" 다른 사람이 불필요한 의존성을 추가하는 것을 막을 수 있다(캡슐화).

<aside> 💡

A: 이거 useEffect를 사용해서 API를 호출 해야 할 것 같은데요?

B: 아 그건 useFetch hooks를 사용하시면 돼요!

A: 아! 그럼 별도로 useEffect를 추가하지 않고, 커스텀 훅만 사용하면 되겠군요!

</aside>

## ☑️ 고차 컴포넌트(HOC,Higher Order Component)

컴포넌트 자체의 로직을 재사용하기 위한 방법으로, **컴포넌트를 인자로 받아 새로운 컴포넌트를 반환한다**.

고차 함수의 경우 react에 국한된 기법이 아니기 때문에, **리액트가 아닌 자바스크립트 환경에서도 사용할 수 있다.**

리액트에서 최적화와 중복 로직 관리를 위한 대표적인 고차 컴포넌트가 `React.memo`다.

## ☑️ `React.memo`란?

일반적으로 부모 요소가 리렌더링 되면 자식 요소도 같이 리렌더링이 된다. 이와 같이 컴포넌트의 불필요한 리렌더링을 방지하는 고차 컴포넌트가 `React.memo`다. **props와 비교해 이전 props가 같다면 렌더링 자체를 생략하고 이전에 기억해 둔(memoization) 컴포넌트를 반환한다.**

`useMemo`와 비슷한 기능이지만 `useMemo`는 값을 반환받기 때문에 JSX 함수 방식이 아닌 { }을 활용한 할당식을 사용한다는 차이점이 있다.

## ☑️ 고차 함수 만들어보기

함수를 인자로 받거나 결과로 반환하는 함수

고차 함수의 예시들을 알아보자.

1. `Array.prototype.map`

```jsx
const list = [1, 2, 3]
const doubledList = list.map((item) => item * 2)

//함수를 인수로 받아서 사용함. forEach, reduce 등도 고차 함수
```

1. `setter`

```jsx
const setState = (function () {
	let currentIndex = index
	return function (value) {
		global.states[currentIndex] = value
	}
})()
```

1. 연산 고차 함수

```jsx
function add(a) {
	return function (b) {
		return a + b
	}
}

const result = add(1) 
const result2 = result(2)
```

## ☑️ 고차 컴포넌트 Deep Dive

고차 함수를 활용해서 react에서 고차 컴포넌트를 만들어보자.

아래의 코드는 사용자 인증 정보에 따라서 보여주는 컴포넌트를 분할하는 상황이다.

```jsx
import React from "react";
import { Navigate } from "react-router-dom";

// ✅ HOC: 인증된 사용자만 컴포넌트를 볼 수 있도록 함
const withAuth = (WrappedComponent) => {
  return function EnhancedComponent(props) {
    const isAuthenticated = localStorage.getItem("token"); // 예제: 토큰 여부로 인증 확인

    if (!isAuthenticated) {
      return <Navigate to="/login" />; // 로그인 페이지로 리디렉트
    }

    return <WrappedComponent {...props} />; // 인증된 경우 원래 컴포넌트 렌더링
  };
};

// ✅ 일반 컴포넌트
const Dashboard = () => <h1>대시보드 (보호된 페이지)</h1>;
const Profile = () => <h1>프로필 (보호된 페이지)</h1>;

// ✅ HOC 적용
const ProtectedDashboard = withAuth(Dashboard);
const ProtectedProfile = withAuth(Profile);

export default function App() {
  return (
    <div>
      <ProtectedDashboard />
      <ProtectedProfile />
    </div>
  );
}

```

고차 컴포넌트를 구현할 때 주의할 점이 있다.

1. `with`로 네이밍을 시작해야 한다. 일종의 관습
2. 부수 효과를 최소화 해야 한다.
    1. 고차 컴포넌트는 반드시 컴포넌트를 인수로 받게 되는데, props를 임의로 수정, 추가, 삭제 하는 일은 없어야 한다.
3. 여러 개의 고차 컴포넌트로 컴포넌트를 감쌀 경우 복잡성이 증가한다.
    1. 따라서 고차 컴포넌트는 최소한으로 사용하는 것이 좋다.

## ☑️ 사용자 정의 훅과 고차 컴포넌트 중 무엇을 써야 할까?

- 사용자 정의 훅을 사용하는 경우

`useEffect`, `useState`와 같이 **리액트에서 제공하는 훅으로만 공통 로직을 격리할 수 있다면** 커스텀 훅을 사용하는 것이 좋다.

- 고차 컴포넌트를 사용하는 경우

함수 컴포넌트의 반환값, 즉 **렌더링의 결과물에도 영향을 미치는 공통 로직**이라면 고차 컴포넌트를 사용하는 것이 좋다.

**가능하면 Custom Hook을 우선적으로 고려**하고, **UI에 영향을 주는 경우에만 HOC 사용!**