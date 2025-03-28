> **그래프 탐색이란**
> **그래프의 정점**부터 시작해 **모든 노드를 한 번 씩 방문**하는 것을 의미한다.

## DFS(Depth-first Search) 깊이 우선 탐색
- 그래프를 탐색하는 방법 중 하나로, 특정 노드부터 시작하여 가능한 깊이 탐색하는 방식이다.
**구현 방법**
- **스택**이나 **재귀 호출**
장점
- 스택을 이용하기 때문에 메모리 사용량이 적다. 현재 경로에 있는 데이터만 저장하기 때문이다.

사용 사례
- 특정 경로를 찾거나 모든 경로를 탐색할 때 유리하다.

### BFS(Breadth-First Search) 너비 우선 탐색
- 그래프를 탐색하는 방법 중 하나로, 특정 노드부터 시작하여 같은 레벨에 존재하는 이웃 노드를 탐색하는 방식이다.
**구현 방법**
- **큐**

단점
- 큐를 이용하기 때문에 메모리 사용량이 많다. 현재 탐색 중인 모든 노드를 저장해야하기 때문에 메모리 사용률이 높다.

사용 사례
-  최단 경로를 찾는데 유리하다.
