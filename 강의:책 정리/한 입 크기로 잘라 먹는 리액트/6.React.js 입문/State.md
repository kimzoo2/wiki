State를 갖는 React 컴포넌트 -> State 값에 따라 UI를 렌더링한다. => 리 렌더, 리 렌더링이라고 함

사용방법
```
import { useState } from "react";

// 초기값을 받아서 두개의 인덱스를 가진 배열을 반환한다
// 변수, 리렌더링을 촉발하는 set 함수를 반환
// 구조 분해 할당을 통해 배열을 할당
const [count, setState] = useState(0);
const [light, setLight] = useState("OFF");
```

=> https://ko.react.dev/reference/react/useState


#### var, let 등을 통해 상태 변화를 하면 되지 않을까?
State를 사용하지 않으면 **리렌더링 되지 않는다.**