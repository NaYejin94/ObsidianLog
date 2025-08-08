1. **Context 생성**

```jsx

//App.js
const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

//TasksContext.js <<-- 두 개의 별개의 context생성
export const TasksContext = createContext(null);
export const TasksDispatchContext = createContext(null);
```

2.  **State과 dispatch 함수를 context에 넣기**

```jsx

App.js
import { TasksContext, TasksDispatchContext } from './TasksContext.js';

export default function TaskApp() {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
  // 추가된 사항..
  return (
    <TasksContext.Provider value={tasks}>//추가된 사항
      <TasksDispatchContext.Provider value={dispatch}>//추가된 사항
       <AddTask
          onAddTask={handleAddTask}
        />
        <TaskList
          tasks={tasks}
          onChangeTask={handleChangeTask}
          onDeleteTask={handleDeleteTask}
        /> // 삭제 될 사항
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}
```

3. **트리 안에서 context 사용하기**

```jsx

//app.js
<TasksContext.Provider value={tasks}>
  <TasksDispatchContext.Provider value={dispatch}>
    <h1>Day off in Kyoto</h1>
    <AddTask />
    <TaskList />
  </TasksDispatchContext.Provider>
</TasksContext.Provider>

//TaskList.js
export default function TaskList() {
  const tasks = useContext(TasksContext); <<-- 데이터 받음
  
//AddTask.js
export default function AddTask({ onAddTask }) {
  const [text, setText] = useState('');
  const dispatch = useContext(TasksDispatchContext); <<-- 데이터 받음 
  // ...
  return (
    // ...
    <button onClick={() => {
      setText('');
      dispatch({
        type: 'added',
        id: nextId++,
        text: text,
      });
    }}>Add</button>
    // ...
```

1. **하나의 파일로 합치기**

```jsx
//TaskContext.js 
import { createContext, useReducer } from 'react';

export const TasksContext = createContext(null);
export const TasksDispatchContext = createContext(null);
//---원래는 여기까지 밖에 없는 js 파일이였음 

export function TasksProvider({ children }) {
  const [tasks, dispatch] = useReducer(
    tasksReducer,
    initialTasks
  );

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        {children}
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [...tasks, {
        id: action.id,
        text: action.text,
        done: false
      }];
    }
    case 'changed': {
      return tasks.map(t => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter(t => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}

const initialTasks = [
  { id: 0, text: 'Philosopher’s Path', done: true },
  { id: 1, text: 'Visit the temple', done: false },
  { id: 2, text: 'Drink matcha', done: false }
];
//그냥 App.js의 기능을 TaskProvider로 옮겨준것 뿐임 하지만 이는 관심사 분리임 
//TasksProvider는 상태관리만 담당
//App.js는 UI만 담당
```