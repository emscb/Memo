# 6 Graphs

## 6.1 The Graph Abstract Data Type

### 6.1.2 Definitions

- 두 개의 집합으로 이뤄짐
  - Vertices / Edges (정점 / 선)
  - Edge : Vertices의 쌍
    - 방향이 없으면 `()`, 있으면 `<>`로 표현 (정해진 건 아님)
- 그래프의 제한점
  - Self edge는 없다.
  - 같은 점들에 대한 중복되는 선은 없다.
- Complete graph
  - 점이 $n$개 라면 $n(n-1)/2$개가 가질 수 있는 edge의 최대값 (undirected)
  - Directed면 2배
- 점 $u$, $v$가 edge로 연결되어 있다면
  - 그 두 점은 adjacent하다고 한다.
  - 그 쌍 $(u,v)$은 incident on/to $u$ and $v$라 한다.
- Subgraph

#### Path

- $u$에서 $v$까지 가는 길
- 지나가는 점을 순서대로 나열한 집합 (각 점 사이에 edge가 존재한다.)
- Length of path : 그 사이 간선의 개수
- Simple path : 같은 edge가 안나오는 것 (책에는 같은 점이 없는걸로 나와 있음)
- Simple directed path : 똑같은데 방향을 따져야 함
- Undirected path : Directed graph에서 방향을 무시하고 따짐
- Cycle : 시작과 끝 점이 같은 것 (Circuit)

#### Connected Component

- 두 정점 사이 path가 하나 이상인 경우 **connected**라 한다.
- 그래프의 모든 정점들이 connected라면 **connected graph**라 한다.
- Connected component : Maximal connected subgraph
  - 그 그래프에 점을 하나 추가했는데 connected가 아니라면 초기 그래프가 connected component이다.
- **Strongly connected**
  - "두 개의 정점이 strongly connected다." : 두 정점 사이에 경로가 양방향으로 다 있다.
  - "그래프가 strongly connected다." : 모든 정점들이 strongly connected하면 됨

#### Tree와 Graph의 관계

- Graph 중 tree인 걸 골라낼 수 있음
  - Cycle이 없는 undirected graph를 tree라 볼 수 있음
  - Root의 개념은 없지만... 모양은!

#### Degree 차수

- 정점의 차수 : 연결된 간선의 개수
- In-degree / Out-degree : 나가는 선인지 들어오는 선인지
- 정점의 차수들을 다 더하면 전체 간선 개수\*2가 된다.
- Directed graph를 **digraph**라 부르자

### 6.1.3 Graph Representations

- Class를 `virtual`로 구현해서 상속받아 여러 방법으로 구현할 수 있게 해놨다.

#### 6.1.3.1 Adjacency Matrix

- 행렬처럼 표시하자
  - 정점의 개수에 비례하는 2차원 배열을 이용
  - 간선의 유무를 0/1로 표기
- 복잡도는 $O(n^2)$
- 간선이 많다면 괜찮지만 별로 없으면 (sparse해지면) 좀 아깝다.

#### 6.1.3.2 Adjacency Lists

- Linked list를 이용해 표현
- 엄청 그래프가 크다면 list보다 array로 하는게 메모리를 아낄 수 있다.
- 정점의 차수를 계산하고 싶다.
  - 해당 list 노드의 개수가 그 점의 차수다.
  - Digraph에서 똑같은 방법으로 훑으면 out-degree는 계산할 수 있다.
  - In-degree를 많이 쓴다면 그냥 inverse adjacency list를 만들자
    - 기준을 반대로 생각해서 애초에 들어오는 점을 기준으로 노드를 이은 것
  - 두 개의 list를 합치면?

---

- Orthogonal하게 표현하려면 두 개의 list가 필요하다. (행 따로, 열 따로)
- 노드 하나하나는 간선에 해당
- Data field 2개와 포인터(link field) 2개가 필요
- ($u$, $v$, column, row) 형태

#### 6.1.3.3 Adjacency Multilists

- 간선에 한 번만 방문해야 하거나, 방문했었던 사실을 기록해야 하는 경우
  - Undirected graph에서 flag 표기가 매우 귀찮기 때문에 다른 자료 구조로 하자
- Multilist의 node의 구조
  - `m` : Flag (방문했는지 기억)
  - `vertex1, 2` : 정점 2개
  - `link1, 2` : 포인터 2개 (간선에 해당하는 노드를 가리킴)
- `adjNode` : 간선에 해당하는 노드를 가리키는 각 정점의 배열
  - `adjNode`에서 어느 점에서부터 시작됐는지 알고 그 점에 해당하는 포인터를 따라가면 연결성을 알 수 있다.

#### 6.1.3.4 Weighted Edges

- 두 점 사이의 거리처럼 **간선에 가중치를 부여**할 수 있다.
- Graph 중에 weighted edge를 갖고 있는 것을 **네트워크**라고 부른다.

## 6.2 Elementary Graph Operations

### 6.2.1 Depth-First Search

- 깊이 우선 탐색, DFS
  - 그래프에서의 탐색은 tree에서의 traversal과 유사
  - 정점들을 방문하는 것
  - 기본적으로 노드(정점)의 내용을 출력
  - 어떤 순서대로 방문할거냐?
- Tree에서는 root가 있어서 거기부터 시작하면 되지만 graph는 그런 게 없기 때문에 사용자가 지정해줘야 한다.
- 계-속 아무거나 방문하다가 더 이상 갈 데가 없다면 다시 되돌아가서 방문하지 않은 이웃을 방문한다.
  - 보통 그래프를 구현한 형태를 그대로 사용한다. (아무거나가 아닐 수 있음)
- 시간 복잡도
  - List : 노드의 개수만큼 (edge만큼)
  - Matrix : O(정점의 개수^2^)

### 6.2.2 Breadth-First Search

- 너비 우선 탐색, BFS
- 시작점에서 이웃들을 모두 방문하고 다음 단계로
- 큐를 사용
  - 큐에서 하나 꺼내고 그것의 이웃을 방문했는지 확인
  - 방문할 때마다 큐에 집어넣고, 큐가 빌 떄까지 계속 반복함

### 6.2.3 Connected Components

- 그래프 속에 connected component가 몇 개인지 찾는 프로그램
  - 하나하나 출력!
- DFS나 BFS를 돌리고 끝났다면 connected component가 하나 나온다.
  - 이후 방문할 점이 없다면 끝이고, 아니라면 또 돌리면 또 component하나가 나오겠지

### 6.2.4 Spanning Trees

- Connected 그래프의 spanning tree?
  - 그 그래프의 subgraph여야 함
  - 그래프의 모든 정점을 다 갖고 있어야 함
  - Connected여야 함
  - Cycle이 없어야 함
  - **간선이 $n-1$개 있어야 함**
- DFS, BFS를 사용하면
  - 정점을 방문하는 순서대로 그냥 이어주면 된다.

### 6.2.5 Biconnected components

#### Articulation Point

- 그 정점을 삭제했을 때, 그래프가 2개 이상의 connected component로 쪼개지는 점

#### Biconnected Graph

- Articulation point가 없는 그래프

#### Biconnected component

- 어떤 그래프에서 maximal한 biconnected subgraph
- 거기다가 정점을 조금 더 붙이면 그렇지 않아지는 것

## 6.4 Shortest Paths and Transitive Closure

- Path : 정점들을 연결하는 간선들의 집합 (길)
- 경로의 길이를 구하기 위해 간선에 가중치(weight)를 부여하게 된다.
- 최단 경로의 길이 / 경로 자체 구하기
  - 사실 같은 문제다.

### 6.4.1 Single Source / All Destinations : Nonnegative Edge Costs

- 보통 directed graph를 갖고 한다.
- Source : 첫 시작점
- Destination : 끝점, 도착점
- 점 하나를 사용자가 정해주면 나머지 점들로 가는 최단 경로를 구하는 것
- 이어진 길의 weight보다 작은 weight의 길이 있으면 다른 경로가 최단 경로일 수 있다.
- $S$에는 최단 경로가 정해진 destination만 넣고, 거기서부터 또 가장 거리가 짧은 애들을 $S$에 넣는다.
  - 실상 길이의 순서대로 들어가게 된다.
  - 최단 경로를 지정하면서 $S$에 들어간다.
  - $S$에 점이 추가되다보면 **최단 경로가 달라질 수 있다.** (갱신 가능)
- 시간 복잡도는 $O(n^2)$
- 가중치가 음수가 되면 안되는 한계점이 있다.

### 6.4.2 Single Source / All Destinations : General Weights

- 가중치가 음수면 곤란한데 어떡하지
- Negative cycle의 문제점은, 지나갈 때마다 경로의 길이가 작아진다는 것이다.
  - 이게 있으면 아예 시작하지 말자
- 정점이 $n$개라면 최단 경로가 포함할 수 있는 간선의 개수는 많아봐야 $n-1$개다.
- $l$ : 최단 경로가 가지는 간선의 최대 개수
- 간선을 $k$개 쓸 수 있는데
  - $k-1$개로도 충분한 경우
  - 정말 $k$개를 다 써야되는 경우
- **재귀 정의 확인!**
- Bellman and Ford 알고리즘은 최단 길이만 출력하면 됨

### 6.4.3 All-Pairs Shortest Paths

- 정점의 모든 쌍에 대해서 최단 경로를 구하자
- 경로 $n-1$개를 보여주면 된다.
- 모든 쌍에서는 $n(n-1)$개를 보여주면 됨
  - 행렬 또는 table로
  - 그 행렬이 $A$
  - $A^2$ : 최단 경로인데 중간 정점으로 $0$부터 $2$까지 가진다는 것
  - $A^-$$^1$ : 직접 연결된 것
- **재귀 정의 확인!**
- 시간 복잡도 : $O(n^3)$

