**연산** 자체를 Memoization 할 수 있음
불필요한 연산을 최적화 하는 리액트 훅

의존성 배열(=deps)를 기준으로 memoization된다

AS-IS
```js
const getAnalyzedData = () => {

	console.log("getAnalazedData 호출!");
	const totalCount = todos.length;
	const doneCount = todos.filter((todo) => todo.isDone).length;
	const notDoneCount = totalCount - doneCount;
	return {
		totalCount,
		doneCount,
		notDoneCount,
	};
};

const { totalCount, doneCount, notDoneCount } = getAnalyzedData();
```

-> 컴포넌트가 리렌더링 될 때마다 getAnalyzedData가 호출되어 불필요한 연산이 발생한다.

TO-BE

```js
	const { totalCount, doneCount, notDoneCount } = useMemo(() => {
	
	console.log("getAnalazedData 호출!");
	const totalCount = todos.length;
	const doneCount = todos.filter((todo) => todo.isDone).length;

	const notDoneCount = totalCount - doneCount;
	
	return {
		totalCount,
		doneCount,
		notDoneCount,
	
	};

}, [todos]); // deps를 기준으로 memoization된다
```

todos deps가 변경될 때만 콜백 함수가 실행된다. 즉, 불필요한 연산을 제거하고 최적화한다.