- 변경될 수 있는 값과 변경되지 않는 값을 통해 context를 분리하자.

- 리렌더링이 되던 이유는 Provider에 선언된 함수 객체가 재생성되기 때문이다.
![재생성][https://cdn.inflearn.com/public/files/posts/28955cdb-9963-4b44-a825-67ad4681aca8/42dd2654-34e7-469c-8b78-3ee3ce91b772.png]
-> value에 선언한 것도 결국 새로운 객체이기 때문에 리렌더링 되었다, 는 뜻으로 이해함

그래서 이 문제를 해결하기 위해 변경될 수 있는 값과 변경되지 않는 값을 분리하고(-> 값 변경 시 리렌더링 범위를 최소화하기 위함이다.)변경되지 않는 객체를 만들어 useMemo로 값을 저장하여 연산하지 않게 한다.

```js
const memoizedDispatch = useMemo(() => {
	return {
		onCreate,
		onUpdate,
		onDelete,
	};
}, []);

return (

<div className="App">
<Header />
	<TodoStateContext.Provider value={todos}>
		<TodoDispatchContext.Provider value={memoizedDispatch}>
			<Editor />
			<List />
		</TodoDispatchContext.Provider>
	</TodoStateContext.Provider>
</div>
);
```