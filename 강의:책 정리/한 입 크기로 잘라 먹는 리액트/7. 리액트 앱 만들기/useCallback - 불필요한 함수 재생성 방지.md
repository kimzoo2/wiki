React.memo는 얕은 비교를 하기 때문에 모든 변수를 하나하나 변경 검증을 해야만 했다.

객체 함수의 불필요한 재생성을 막아, 리렌더링을 방지하는 방법이 useCallback이다.


```js
const onUpdate = useCallback((targetId) => {
	dispatch({
		type: "UPDATE",
		targetId: targetId,
	});
}, []); // 마운트 하는 시점에만 함수를 생성 (재생성 방지)

TodoItem은 아래와 같이 변경 (React.memo의 얕은 비교에 따른 검증 로직 제거)
export default memo(TodoItem);
```

언제 어떤 것을 최적화하면 좋을까?
- (언제) 완성한 상태에서
- (어떤 것) 유저의 행동에 따라 개수가 많아지거나, 함수를 많이 가지고 있는 컴포넌트는 최적화를 수행하는 걸 권장