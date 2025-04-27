useRef란? 
- 컴포넌트 내부에 Reference 객체를 생성하는 기능
- useState와 비슷하지만 **useRef는 리렌더링하지 않는다**.
- 특정 DOM 요소에 접근해서 요소를 조작할 수 있는 장점이 있다.

DOM API란?
>https://one-step-js.hyobb.com/2e6337b2-0a55-4488-908c-1d08baa20d23


useRef는 리렌더링 된다고 해도 내부의 상태값이 유지된다는 특성이 있다. 값을 유지하거나, DOM에 접근해서 포커싱 등을 하기에 적합하다.