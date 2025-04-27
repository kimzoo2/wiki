컴포넌트의 **사이드 이펙트**(=부수적인 효과, 파생되는 효과)를 제어하는 리액트 훅이다.
컴포넌트 동작에 따라 파생되는 여러 효과를 의미한다.

```js
// count가 변경될 때마다 전달된 콜백 메소드가 실행된다.
useEffect(() => {
console.log(`count : ${count} / input : ${input}`); 
}, [count, input]);
// 의존성 배열
// depecndency arry = deps(뎁스)


const onClickButton = (value) => {
	setCount(count + value); // 비동기로 동작함
	console.log(count); // 변경 이전 값을 출력하게 됨
};
```

만약 useEffect가 아니라 onClickButton이 실행될 때마다 console.log를 호출한다면?
-> 변경 이전 값을 출력하게 된다. 왜냐하면 useState의 set함수가 **비동기로 실행**되기 때문이다.