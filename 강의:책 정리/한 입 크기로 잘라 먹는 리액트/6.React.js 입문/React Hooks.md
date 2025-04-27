### React Hooks란?
클래스 컴포넌트의 기능을 함수 컴포넌트에서도 사용할 수 있도록 하는 기능이다.
-> 2017년도 이전에는 **Class 컴포넌트**를 사용했음. 하지만 **Class 문법이 복잡하다는 단점**이 있었다. 그래서 함수 컴포넌트에서도 클래스 컴포넌트 기능을 사용할 수 있도록 낚아채는 기능을 추가했다.

여태 쓰던 useState, useRef도 React Hooks였다. use~라는 prefix가 붙는 특정이 있다.

- 조건문, 반복문에서는 호출 불가
- 나만의 hook도 가능

#### 1. 함수 컴포넌트에서만 가능

```js
// 바깥에서 호출하면 오류난다.
const state = useState(); <! 오류 발생
const HookExam = () => {
... 
}
```

### 2. 조건문, 반복문에서 호출 불가
``` js
/*
if(true) {
const state = useState(); <! 오류 발생
}

*/
```
- 실행 시키는 순서가 복잡해지기 때문이라고 함

### 3. 나만의 훅
함수에 use prefix를 붙이면 리액트 내부에서 리액트 훅이라고 인식한다.
대체로 src 아래에 hooks 폴더를 만듦

```js
import { useState } from "react";

// function -> custon Hook으로 바꾸기
// 함수명이 use~로 시작하면 리액트 내부에서 리액트 훅으로 인식한다.
function useInput() {
	const [input, setInput] = useState("");
	const onChange = (e) => {
		setInput(e.target.value);
		};
	return [input, onChange];
}
```