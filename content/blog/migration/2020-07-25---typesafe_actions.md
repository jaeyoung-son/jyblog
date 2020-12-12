---
title: '타입스크립트 리덕스 리팩토링'
date: 2020-07-26 12:21:13
category: 'typescript'
draft: false
---

이전에 typescript와 리덕스 를 함꼐 사용하는 글을 정리한 적이 있는데, 주로 사용하지 않았던 터라 시간이 지나고 잊은 부분도 있었고, typesafe-actions라는 라이브러리도 적용해서 코드도 깔끔하게 정리하기 위해 재차 시도 해보려고 합니다.

## 기본적인 ts, redux 카운터기능 구현과 todoList 기능 구현

#### modules/counter.ts

```ts
// 액션 타입
const INCREASE = 'counter/INCREASE' as const
const DECREASE = 'counter/DECREASE' as const
const INCREASE_BY = 'counter/INCREASE_BY' as const

// 액션 생성함수
export const increase = () => ({
  type: INCREASE,
})

export const decrease = () => ({
  type: DECREASE,
})

export const increaseBy = (diff: number) => ({
  type: INCREASE_BY,
  payload: diff,
})

//액션 겍체들에 대한 타입
type CounterAction =
  | ReturnType<typeof increase>
  | ReturnType<typeof decrease>
  | ReturnType<typeof increaseBy>

// 모듈에서 관리 할 상태의 타입
type CounterState = {
  count: number
}

const initialState: CounterState = {
  count: 0,
}

// 리듀서
function counter(
  state: CounterState = initialState,
  action: CounterAction
): CounterState {
  switch (action.type) {
    case INCREASE:
      return { count: state.count + 1 }
    case DECREASE:
      return { count: state.count - 1 }
    case INCREASE_BY:
      return { count: state.count + action.payload }
    default:
      return state
  }
}

export default counter
```

기본적인 액션, 액션생성함수, 리듀서를 준비하고 그에대한 타입들을 잡아준다.

#### modules/index.ts

```ts
import { combineReducers } from 'redux'
import counter from './counter'

const rootReducer = combineReducers({
  counter,
})

export default rootReducer

// 루트 리듀서의 반환 타입 잡기
export type RootState = ReturnType<typeof rootReducer>
```

일반적인 루트리듀서를 만드는것처럼 만드나, ReturnType으로 루트리듀서의 반환 타입을 잡아서 export 해준다.

#### index.tsx

스토어 생성

```tsx
...
import { Provider } from 'react-redux';
import { createStore } from 'redux';
import rootReducer from './modules';

const store = createStore(rootReducer);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

특별한 처리 없이 일반 js처럼 스토어를 생성해서 provider에 적용해준다.

#### components/Counter.tsx

```tsx
import React from 'react'

type CounterProps = {
  count: number
  onIncrease: () => void
  onDecrease: () => void
  onIncreaseBy: (diff: number) => void
}

function Counter({
  count,
  onIncrease,
  onDecrease,
  onIncreaseBy,
}: CounterProps) {
  return (
    <div>
      <h1>{count}</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
      <button onClick={() => onIncreaseBy(5)}>+5</button>
    </div>
  )
}

export default Counter
```

props를 전달받아서 그값들을 렌더링 해준다. 왜 굳이 한번에 받지 않고 전달받을까?? 라는 것에 대한 답은 여러 확장성을 생각해서 컨테이너 컴포넌트를 따로 두어서 만들어준다.

#### containers/CounterContainer.tsx

```tsx
import React from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { RootState } from '../modules'
import { increase, decrease, increaseBy } from '../modules/counter'
import Counter from '../components/Counter'

function CounterContainer() {
  const count = useSelector((state: RootState) => state.counter.count)
  const dispatch = useDispatch()

  const onIncrease = () => {
    dispatch(increase())
  }

  const onDecrease = () => {
    dispatch(decrease())
  }

  const onIncreaseBy = (diff: number) => {
    dispatch(increaseBy(diff))
  }

  return (
    <Counter
      count={count}
      onIncrease={onIncrease}
      onDecrease={onDecrease}
      onIncreaseBy={onIncreaseBy}
    />
  )
}

export default CounterContainer
```

액션생성함수들을 불러오고, 컴포넌트 내부에서 redux-hooks를 사용해 상태를 조회하고, 디스패치를 가져온다.
상태값을 조회할때는 전에 export해주었던 RootState를 지정해주고, 각각의 디스패치를 매칭시켜서 props로 전달한다.

#### modules/todos.ts

어디에서나 빠지지 않는 투두리스트

```ts
// 액션 타입
const ADD_TODO = 'todos/ADD_TODO' as const
const TOGGLE_TODO = 'todos/TOGGLE_TODO' as const
const REMOVE_TODO = 'todos/REMOVE_TODO' as const

let nextId = 1

// 액션 생성 함수
export const addTodo = (text: string) => ({
  type: ADD_TODO,
  payload: {
    id: nextId++,
    text,
  },
})

export const toggleTodo = (id: number) => ({
  type: TOGGLE_TODO,
  payload: id,
})

export const removeTodo = (id: number) => ({
  type: REMOVE_TODO,
  payload: id,
})

// 액션 객체들에 대한 타입
type TodosAction =
  | ReturnType<typeof addTodo>
  | ReturnType<typeof toggleTodo>
  | ReturnType<typeof removeTodo>

// 상태에서 사용 할 할 일 항목 데이터 타입 정의
export type Todo = {
  id: number
  text: string
  done: boolean
}

export type TodosState = Todo[]

const initialState: TodosState = []

// 리듀서
function todos(
  state: TodosState = initialState,
  action: TodosAction
): TodosState {
  switch (action.type) {
    case ADD_TODO:
      return state.concat({
        id: action.payload.id,
        text: action.payload.text,
        done: false,
      })
    case TOGGLE_TODO:
      return state.map(todo =>
        todo.id === action.payload ? { ...todo, done: !todo.done } : todo
      )
    case REMOVE_TODO:
      return state.filter(todo => todo.id !== action.payload)
    default:
      return state
  }
}

export default todos
```

위와같이 리덕스 셋팅을 해준 후 루트리듀서에 등록한다.

#### components/TodoInsert.tsx

새 항목추가 함수를 받아서 등록하는 컴포넌트

```tsx
import React, { ChangeEvent, FormEvent, useState } from 'react'

type TodoInsertProps = {
  onInsert: (text: string) => void
}

function TodoInsert({ onInsert }: TodoInsertProps) {
  const [value, setValue] = useState('')
  const onChange = (e: ChangeEvent<HTMLInputElement>) => {
    setValue(e.target.value)
  }
  const onSubmit = (e: FormEvent) => {
    e.preventDefault()
    onInsert(value)
    setValue('')
  }

  return (
    <form onSubmit={onSubmit}>
      <input
        placeholder="할 일을 입력하는곳."
        value={value}
        onChange={onChange}
      />
      <button type="submit">등록버튼</button>
    </form>
  )
}

export default TodoInsert
```

#### components/TodoItem.tsx

```tsx
import React, { CSSProperties } from 'react'
import { Todo } from '../modules/todos'

type TodoItemProps = {
  todo: Todo
  onToggle: (id: number) => void
  onRemove: (id: number) => void
}

function TodoItem({ todo, onToggle, onRemove }: TodoItemProps) {
  // CSSProperties 는 style 객체의 타입
  const textStyle: CSSProperties = {
    textDecoration: todo.done ? 'line-through' : 'none',
  }
  const removeStyle: CSSProperties = {
    marginLeft: 8,
    color: 'red',
  }

  const handleToggle = () => {
    onToggle(todo.id)
  }

  const handleRemove = () => {
    onRemove(todo.id)
  }

  return (
    <li>
      <span onClick={handleToggle} style={textStyle}>
        {todo.text}
      </span>
      <span onClick={handleRemove} style={removeStyle}>
        (X)
      </span>
    </li>
  )
}

export default TodoItem
```

여기서 style 객체를 만들어 줄 때 CSSProperties를 불러와서 타입을 넣어주었다. 스타일 객체의 타입을 잡아줄때 사용하면 된다.

#### components/TodoList.tsx

```tsx
...

type TodoListProps = {
  todos: Todo[];
  onToggle: (id: number) => void;
  onRemove: (id: number) => void;
};

function TodoList({ todos, onToggle, onRemove }: TodoListProps) {
  if (todos.length === 0) return <p>등록된 항목이 없네요.</p>;
  return (
    <ul>
      {todos.map(todo => (
        <TodoItem
          todo={todo}
          onToggle={onToggle}
          onRemove={onRemove}
          key={todo.id}
        />
      ))}
    </ul>
  );
}

export default TodoList;

```

리스트 배열을 순환하며 렌더링 해주는 투두리스트 컴포넌트를 만든다. 그 후 카운터에서 적용했던것처럼 스토어의 상태를 조회하고, 디스패치를 조회하는 컨테이너 컴포넌트를 만들어서 각각의 값들을 위의 TodoInsert, TodoList 컴포넌트들에 전달해 준다.

### typesafe-actions

typesafe-actions 라이브러리는 리덕스를 사용하는 프로젝트에서 액션생성함수,리듀서를 간결하게 만들 수 있게 해주는 라이브러리다.

```
npm install typesafe-actions
```

#### 카운터에 적용해보기

```ts
import { createAction, ActionType, createReducer } from 'typesafe-actions'
```

위의 세가지를 typesafe-actions에서 불러온다. 이전에는 createStandardAction을 사용했으나 최신버전으로 바뀌면서 createAction을 사용한다.
