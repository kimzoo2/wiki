State를 Props로 전달하기

```js
const Counter = () => {
	const [count, setState] = useState(0);
	return (
		<div>
			<h1>{count}</h1>
			<button
			onClick={() => {
				setState(count + 1);
			}}
            >				
			+
			</button>
		</div>
		);
};

function App() {
	return (
	<>
		<div>
			<Counter />
		</div>
	</>
	);
}
```

### 주의사항
props가 리렌더링 되는 상황을 3가지다
1. 자신이 관리하는 state 값이 변경될 때
2. props가 변경될 때
3. 부모 컴포넌트가 리렌더링될 때

=> 해결하기 위해서 **props를 컴포넌트로 분리**한다.