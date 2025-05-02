React.memo란?
리액트의 내장함수로, **불필요한 리렌더링을 방지**하는 방법이다.
부모 컴포넌트가 리렌더링 되더라도 **props가 변경되지 않는 이상 리렌더링되지 않게 해주는 방법**이다. (memo로 최적화 됨)

문제는 객체 타입을 props로 받으면 memoization이 어렵다. 
-> 객체는 새로 생성될 때마다 값이 변경되기 때문이다.

해결 방법은 **HOC**. memo 호출 시 콜백함수를 통해 이전값과 현재값을 비교하여 같은 컴포넌트인지 검증하는 방법이다.

```js
import { memo } from "react";

// 고차 컴포넌트 (HOC)
export default memo(TodoItem, (prevProps, nextProps) => {

// 부모 컴포넌트가 리렌더링될 때마다 콜백을 실행해서
// 함수의 반환값을 통해 바뀌었는지 바뀌지 않았는지 확인할 수 있게 됨

// T -> 리렌더링 X
// F -> 리렌더링 O

	if (prevProps.id !== nextProps.id) return false;
	if (prevProps.isDone !== nextProps.isDone) return false;
	if (prevProps.content !== nextProps.content) return false;
	if (prevProps.date !== nextProps.date) return false;
	return true;

});
```


HOC란?
- 컴포넌트에 기능을 추가하는 패턴