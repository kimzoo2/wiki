### Context
- 컴포넌트 간의 데이터를 더 쉽게 전달해주는 또 다른 방법
- Props Drilling의 문제를 해결하기 위한 방법이다.

Context는 **데이터 보관소 역할**을 하는 **객체**이다.
특정 컨텍스트의 값을 각 컴포넌트에 공유할 수 있게 만들 수 있다.

### 선언
App 컴포넌트 외부에 Context를 생성한다.
```js
// App 컴포넌트 외부에서 컨텍스트를 생성함 (리렌더링할 때마다 재생성되니까)
const TodoContext = createContext();
console.log(TodoContext); // Provider만 이해하면 됨
```


### 컨텍스트 경계 설정
```js
return (
	<div className="App">
	<Header />
	<TodoContext.Provider
		value={{
		// context가 공급하는 값
			todos,
			onCreate,
			onUpdate,
			onDelete,
		}}
    >
		<Editor />
		<List />
	</TodoContext.Provider>
</div>
);
```
-> Editor와 List 컴포넌트에만 ToDoContext를 공유한다. Header는 공유하지 않는다. Provider를 통해 제공하여 Editor나 List에 존재하던 Props를 제거하였다.


### 컴포넌트 내 컨텍스트 사용
1. **컨텍스트 공유**
``` js
export const TodoContext = createContext();
```
-> App 외부에 있던 컨텍스트를 export해서 공유한다.

2. **useContext, context import**
``` js
import { useState, useRef, useContext } from "react";
import { TodoContext } from "../App";
```

3. **컨텍스트 사용**
```js
const { onCreate } = useContext(TodoContext);
```
-> 구조 분해 할당을 통해 컨텍스트에서 공유한 값을 꺼낸다.



### 문제 발생 : 최적화 풀림
useCallback을 활용해서 최적화했던 문제가 풀렸다. 어떻게 해결하면 좋을까?
-> Context 분리하기에서 이어짐