1.**단계: state를 설정하는 것에서 action을 dispatch 함수로 전달하는 것으로 바꾸기**

```jsx
const [tasks, setTasks] = useState(initialTasks); //수정 전 
const [tasks, dispatch] = useReducer(tasksReducer, initialTasks); // 수정 후

// 수정전
 setTasks([...tasks, { 
 setTasks(tasks.map(t => {
 setTasks(
 
//수정 후 
dispatch({ 
 <<-- 여기 안에 들어오는 것들은 action임 
```

```jsx
import { useState } from 'react';
import AddTask from './AddTask.js';
import TaskList from './TaskList.js';

export default function TaskApp() {
    const [tasks, setTasks] = useState(initialTasks);

  function handleAddTask(text) {
    setTasks([...tasks, { 
      id: nextId++,
      text: text,
      done: false 
    }]);
  }

  function handleChangeTask(task) {
    setTasks(tasks.map(t => {
      if (t.id === task.id) {
        return task;
      } else {
        return t;
      }
    }));
  }

  function handleDeleteTask(taskId) {
    setTasks(
      tasks.filter(t => t.id !== taskId)
    );
  }

  return (
    <>
      <h1>Prague itinerary</h1>
      <AddTask
        onAddTask={handleAddTask}
      />
      <TaskList
        tasks={tasks}
        onChangeTask={handleChangeTask}
        onDeleteTask={handleDeleteTask}
      />
    </>
  );
}

let nextId = 3;
const initialTasks = [
  { id: 0, text: 'Visit Kafka Museum', done: true },
  { id: 1, text: 'Watch a puppet show', done: false },
  { id: 2, text: 'Lennon Wall pic', done: false },
];
```

2.**단계: reducer 함수 작성하기**

1. 첫 번째 인자에 현재 state (`tasks`) 선언하기.
2. 두 번째 인자에 `action` 객체 선언하기.

```jsx
function yourReducer(state, action) {
  // React가 설정하게될 다음 state 값을 반환합니다.
  // 인자값은 2개를 받습니다
} 
```

3.**단계: 컴포넌트에서 reducer 사용하기** reducer에서 _다음_ state 반환하기 (React가 state에 설정하게 될 값).

```jsx
function tasksReducer(tasks, action) {
  if (action.type === 'added') {
    return [...tasks, {
      id: action.id,
      text: action.text,
      done: false
    }];
  } else if (action.type === 'changed') {
    return tasks.map(t => {
      if (t.id === action.task.id) {
        return action.task;
      } else {
        return t;
      }
    });
  } else if (action.type === 'deleted') {
    return tasks.filter(t => t.id !== action.id);
  } else {
    throw Error('Unknown action: ' + action.type);
  }
}
 // switch로 바꿔주면 더 좋음!
```

reducer 함수는 state(`tasks`)를 인자로 받고 있기 때문에, 이를 **컴포넌트 외부에서 선언**할 수 있습니다. 이렇게 하면 들여쓰기 수준이 줄어들고 코드를 더 쉽게 읽을 수 있습니다.

⇒ 컴포넌트 외부에서 선언할수 있다는게 무슨말일까??

reducer는 순수함수 이기 때문

state는 함수 아닙니다 값 혹은 데이터 입니다~

- state vs reduce
    
    |항목|순수 함수?|외부 선언 가능?|이유|
    |---|---|---|---|
    |`useState`|❌ 아님|❌ 안 됨|React 상태 훅, 부작용 포함|
    |`setCount`|❌ 아님|❌ 안 됨|React 내부 상태 바꾸고 리렌더링|
    |`useReducer`|❌ 아님|❌ 안 됨|상태 훅, 부작용 있음|
    |`dispatch`|❌ 아님|❌ 안 됨|상태 변경 및 리렌더 트리거|
    |`reducer 함수`|✅ 맞음|✅ 가능|순수 함수, 상태 계산만 수행|
    
- reducer 함수 예시
    
    ## ✅ 순수 함수인 `reducer` 예시
    
    ```jsx
    
    function reducer(state, action) {
      switch (action.type) {
        case 'increment':
          return { count: state.count + 1 }; // 🔁 새 상태 리턴
        case 'decrement':
          return { count: state.count - 1 };
        default:
          throw new Error('Unknown action');
      }
    }
    
    ```
    
    ## ⛔ 순수 함수가 아닌 reducer 예시 (❌ 이런 건 안 됨)
    
    ```jsx
    
    function badReducer(state, action) {
      switch (action.type) {
        case 'increment':
          state.count += 1; // ❌ 기존 상태를 직접 수정함 (불변성 깨짐)
          return state;
        case 'log':
          console.log('Logged!'); // ❌ 부작용 있음
          return state;
        default:
          return state;
      }
    }
    
    ```
    
    이건 다음 이유들 때문에 **순수 함수가 아님**:
    
    1. `state.count += 1` → 기존 객체 직접 수정 (불변성 위반)
    2. `console.log` → 부작용 있음
    
    - 번외) 고작 console 찍는게 무슨 부작용 이라는걸까??
        
        ## 부작용(side-effect)이란?
        
        > "함수가 실행될 때, 외부 세계에 어떤 '영향'을 주는 것"
        
        즉, 다음 중 하나라도 하면 **부작용이다**:
        
        - DOM 조작 (`document.querySelector`, etc.)
        - API 요청 (`fetch`, `axios`)
        - 로컬 스토리지 읽고 쓰기
        - 콘솔 출력 (`console.log`)
        - 타이머 설정 (`setTimeout`, `setInterval`)
        - 랜덤값 생성 (`Math.random`) ← 얘도 재실행 시 결과 달라짐
        - 상태 변경 (`setState`, `dispatch`)
        - 에러 던지기
        
        ```jsx
        function reducer(state, action) {
        console.log('Hello world'); // 👉 콘솔이라는 외부 세계에 출력함
        return state;
        }
        ```
        
        이건 **함수 내부에서 외부 세계(콘솔)**에 영향을 줬으므로,
        
        React 입장에선 부작용임
        
        👀 물론, 성능이나 기능에 큰 문제가 생기진 않지만 "순수함수인지 아닌지"를 판단할 때는 철저하게 기준 적용함
        
        - 디버깅할 땐 `console.log()` 많이 써도 괜찮아
            
        - 하지만 `reducer`, `pure function`, `useMemo`, `useCallback` 안에서는
            
            **"진짜 순수하게 계산만 한다"** 는 규칙을 지키는 게 좋아
            
        - 왜냐면 React가 **성능 최적화**할 때 이런 순수성을 믿고 동작함
            
        - thre new Error는 그럼 부작용 아님?
            
            부작용 맞는데 허용함…
            
            |코드|부작용?|순수 함수 안에서 허용?|비고|
            |---|---|---|---|
            |`console.log()`|✅|❌|디버깅용, 하지만 출력은 외부 영향|
            |`throw new Error()`|✅|✅ (보통 허용)|명시적 에러 처리를 위한 예외적 부작용|
            |`fetch()`|✅|❌|외부 네트워크 요청이므로 완전한 부작용|
            |`Math.random()`|✅|❌|실행할 때마다 값 바뀜 (비결정적)|
            

```jsx
import { useReducer } from 'react'; //추가

//수정 전
const [tasks, setTasks] = useState(initialTasks);
//수정 후
const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

```

//챌린지는 1,4 번 정도만 추천드립니다