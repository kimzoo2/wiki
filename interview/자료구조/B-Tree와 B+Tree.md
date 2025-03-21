## B-Tree (Balanced Tree)
- **스스로 균형을 잡을 수 있는 트리**를 의미한다. 두개 이하의 자식만 가질 수 있는 Binary Tree와는 달리 N개 이상의 자식을 가질 수 있다.
- 리프 노드는 **linked list로 구현**되어 있어, 노드 간의 탐색이 가능하다. 하지만 **single linked list**이기 때문에 앞으로 탐색은 불가하다.

## B+Tree
- B-Tree에서 **double linked list**로 구현되어 있는 트리를 의미한다. 그렇기 때문에 **리프 노드 간 앞에서 뒤로 탐색**이 가능하다.
- 실제 데이터는 리프 노드에만 존재한다. 브랜치 노드들은 키만 갖고 있으며 **데이터를 탐색하기 위해선 리프 노드를 탐색**해야 한다.

### B-Tree vs B+Tree
- **리프 노드의 자료 구조**와 **데이터 저장 위치 차이**

### 왜 DB는 B-Tree 자료 구조로?
- **대수확장성** 때문이다.

>**대수확장성**이란?
>트리의 깊이가 리프의 노드 수에 비해 느리게 확장되는 성질을 의미한다.