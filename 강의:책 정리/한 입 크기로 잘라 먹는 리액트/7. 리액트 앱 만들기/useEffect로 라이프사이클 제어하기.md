
### 1. 마운트
```js
// 1. 마운트 : 탄생

useEffect(() => {
console.log("mount");
}, []); // deps로 빈배열을 넣으면 된다
```

### 2. 업데이트
```js
// 2. 업데이트 : 변화, 리렌더링
const isMount = useRef(false);

useEffect(() => {
	// update 단계에서만 트리거 하는 방법
	if (!isMount.current) {
		isMount.current = true;
		return;
	}
	console.log("update");
});
```


### 3. 언마운트
```js
const Even = () => {
	useEffect(() => {
		// 클린업, 정리함수
		return () => {
		console.log("unmount");
		};
	}, []); // 마운트 될 때 실행

	return <div>짝수입니다.</div>;

};
```