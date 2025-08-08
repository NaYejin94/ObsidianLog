# Effect에서 이벤트 분리하기

### 이벤트 핸들러 vs Effect

|이벤트 핸들러|Effect|
|---|---|
|사용자의 **특정 상호작용**에 대한 응답|컴포넌트의 **동기화**|
|즉각적이고 사용자의 직접 액션에 반응|렌더링 사이클에 따라 자동으로 실행됨|
|내부로직 : 반응형이 아님|내부 로직 : 반응형|

- **이벤트 핸들러**
    
    : 사용자의 특정 상호작용에 대한 응답으로 실행되는 함수
    
    : 명확한 트리거가 있음
    
    : 즉각적인 반응을 처리
    
    : 단일 작업을 수행
    
- **Effect**
    
    : 컴포넌트가 외부 시스템과 동기화될 때 사용
    
    : 렌더링 후 실행되어야 하는 작업에 적합
    
    : 의존성 배열에 따라 실행 시점이 결정
    

### 반응형 값과 반응형 로직

- 반응형 값
    
    : 컴포넌트 본문 내부에 선언된 props, state, 변수
    
    : Effect가 읽는 모든 반응형 값은 의존성으로 선언되어야 한다.
    
    ⇒ Effect 이벤트 `useEffectEvent`
    

---

# Effect 의존성 제거하기

### 의존성 배열의 역할

```jsx
const serverUrl = '<https://localhost:1234>';

function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, []); // <-- Fix the mistake here!
  return <h1>Welcome to the {roomId} room!</h1>;
}
```

1. 의존성 배열이 비어있으므로 roomId가 변경되어도 Effect가 실행되지 않음
2. 의존성 배열에 반응형 값(roomId)을 사용하지 않음

### 불필요한 의존성 제거하기

⇒ 의존성 배열에 무조건 모든 변수를 넣는 게 아니라 Effect의 목적에 따라 실행 시점을 의도적으로 제어 해야한다.

1. 다른 조건에서 Effect의 _다른 부분_을 다시 실행하고 싶을 수도 있습니다.
    
    ⇒ Effect 내부의 로직을 모두 동일한 의존성에 묶는 대신 특정 조건에 따라 다른 부분만 실행하고 싶을 때가 있다(**useEffect 분리**)
    
2. 일부 의존성의 변경에 “반응”하지 않고 “최신 값”만 읽고 싶을 수도 있습니다.
    
    ⇒ Effect가 특정 변수의 변경에 반응(재실행)하지 않고, 그 변수의 최신 값만 참조하고 싶을 때가 있다
    
3. 의존성은 객체나 함수이기 때문에 _의도치 않게_ 너무 자주 변경될 수 있습니다.
    
    ⇒ 의존성 배열에 객체, 배열, 함수 같은 참조형 데이터를 넣으면, 렌더링마다 새로운 참조가 생성되어 Effect가 불필요하게 자주 실행될 수 있다
    
    1 ) 컴포넌트 외부로 이동
    
    2 ) Effect 내로 동적 객체(함수) 이동
    
    3 ) 객체에서 원시 값 읽기 / 함수에서 원시 값 계산 ( 순수 함수 일때)
    

---

# 커스텀 Hook으로 로직 재사용하기

- 커스텀 Hook은 React Hook을 활용해 재사용 가능한 로직을 캡슐화
- 중복 코드를 줄이고, 컴포넌트 간 로직 공유를 쉽게 만듦

<aside>

**중요한 건, 두 컴포넌트 내부 코드가 _어떻게 그것을 하는지_ (브라우저 이벤트 구독하기) 보다 _그들이 무엇을 하려는지_ (온라인 state 사용하기)에 대해 설명하고 있다는 점입니다.**

</aside>

⇒ 코드를 읽는 사람은 컴포넌트의 목적(의도)을 바로 이해할 수 있고 세부 구현은 Hook 내부에서 처리하기때문에 신경 안써도 됨

### **커스텀 Hook은 state 그 자체를 공유하는게 아닌 state 저장 로직을 공유하도록 합니다.**

- 각각의 Hook은 완전히 독립되어 있다. state를 공유하는게 아님

### **Hook 사이에 상호작용하는 값 전달하기**

- 커스텀 Hook이 순수해야 하는 이유?
    
    : 컴포넌트가 재렌더링될 때마다 Hook이 호출되므로, Hook이 상태를 직접 바꾸거나 외부에 영향을 주면 안됨
    

### 언제 커스텀 Hook을 사용해야 하는지

1. 너무 과도한 커스텀 Hook은 지양
    
    : 간단한 중복은 괜찮다
    
2. Hook을 만들 때 Effect를 포함할지 고민
    
    : Effect는 React 바깥 일을 처리하는 가능성이 높기 때문에 Hook을 감싸면 목적이 명확해짐
    
3. 구체적인 문제를 해결하는데 초점을 맞춰서 커스텀 Hook을 작성해야 함
    

- [`use()](<https://ko.react.dev/reference/react/use>)`
    
    : Promis나 Context 값을 컴포넌트 렌더링 중에 **직접적으로 읽을 수 있게** 해준다.
    
    : 데이터 페칭이나 비동기 작업을 처리할 때 사용되며 Suspense와 통합되어 로딩 상태를 자동으로 관리한다.
    
    : 기존 방식과의 차이
    
    ```jsx
    function UserProfile() {
      const [user, setUser] = useState(null);
      const [loading, setLoading] = useState(true);
    
      useEffect(() => {
        async function fetchUser() {
          const response = await fetch('/api/user');
          const data = await response.json();
          setUser(data);
          setLoading(false);
        }
        fetchUser();
      }, []);
    
      if (loading) return <p>Loading...</p>;
      return <div>{user.name}</div>;
    }
    ```
    
    ```jsx
    // use() 활용
    function UserProfile() {
      const user = use(fetch('/api/user').then(res => res.json()));
      return <div>{user.name}</div>;
    }
    ```