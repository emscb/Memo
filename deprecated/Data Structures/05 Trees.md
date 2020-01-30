# 5 Trees

- 두 점을 이을 수 있는 길이 **단 하나**여야 한다. (더 있으면 graph)

## 5.1 Introduction

- 정의 : 여러 node의 set
  - 하나의 root node가 있다.
  - 나머지 노드는 분할이 된다. (겹치는 것 없이)
  - 그걸 하나씩 나누면 partition(0개 이상), disjoint한 분할이 생기는데 그것도 tree이다.
- 용어 정의
  - 노드 : 선으로 연결된 항목 하나하나. 값(정보) + branch
  - Degree of a node : 아래에 달려있는 subnode(바로 밑 레벨)의 개수 (차수)
    - Degree == 0 : Leaf node, terminal node
    - Degree != 0 : Non-terminal node, non-leaf node
  - Children : 어떤 노드의 아래에 달려있는 서브트리의 root (<-> parents)
  - Siblings : 형제, 같은 부모를 가진 노드
  - Degree of a tree : 소속된 노드의 차수들 중 제일 큰 값
  - Ancestors : 어떤 노드로부터 root node까지 갈 때 만나는 모든 노드
  - Node level : Root를 레벨 1(0이라 할 수 있음), children은 parent+1
  - Height of trees (depth) : 노드 레벨의 최대값

### 5.1.2 Representation of Trees

- List로 표현
  - 트리의 차수만큼의 포인터를 갖게 하면 포인터가 많이 낭비된다.
  - Left child-right sibling
    - 포인터 하나는 가장 왼쪽의 child, 하나는 자신의 오른쪽 sibling을 가리키게 한다.
    - 포인터 낭비를 좀 줄일 수 있다.
  - Degree-2
    - Right sibling을 기울여서 자식처럼 보이게 그린다. (차수가 최대 2밖에 못나오게 된다.)

## 5.2 Binary Trees

- 정의
  - 0개 이상의 노드로 구성된다.
  - Root는 left / right subtree를 가지게 되는데 이 또한 binary tree이다.
  - Tree와 다르게 왼쪽 / 오른쪽을 명시적으로 구별한다.

### 5.2.2 Properties of Binary Trees

- $i$ level인 노드의 최대 개수 : 2^i​-1^개
- Depth가 $k$일 때 노드의 최대 개수 : $2^k-1$개
- Binary tree의 노드가 가득 차 있을 경우 가장 아래 (leaf)쪽의 비중이 가장 높다. (전체 노드 개수 대비)
- 잠시
  - $n$ : 전체 노드 개수
  - $B$ : Branch 개수
  - $n_0$ : 차수가 0인 노드 (leaf)
  - $n_0 = n_2 + 1$
    - $n = n_0 + n_1 + n_2$
    - $n = B + 1$
    - $B = n_1 + 2*n_2$
- Full binary trees
  - 그 높이에서 가질 수 있는 최대 노드 수를 가지는 트리
  - Numbering : Root를 1번이라고 하고 레벨을 따라 왼쪽부터 채워나감
- Complete binary trees
  - 넘버링이 full binary tree와 일치하는 트리
  - Height는 $log_2(n+1)$보다 작지는 않은 최소 정수이다.

### 5.2.3 Binary Tree Representations

#### 5.2.3.1 Array Representation

- 배열을 사용하기 위해서는 넘버링을 full binary tree처럼 해야 한다. (노드가 없더라도)
- 낭비되는 메모리가 생길 수 있다.
  - 트리가 치우쳐져 있을수록(skewed) 많다. Full의 경우 낭비 X
- 포인터 연산이 필요없고 빠르다.

#### 5.2.3.2 Linked Representation

- Parent를 찾기 어렵다. = 포인터를 하나 더 넣으면 된다. (`*parent`)

## 5.3 Binary Tree Traversal

- Traversal = 순회, 다닌다, 방문한다.
- Root부터 시작해서 트리의 노드들을 방문하는 것
- 여러 연산을 포함한다.
  - e.g., 노드의 데이터 출력
  - **방문 순서를 어떻게 할 것인가**가 중요하다.
  - L, R, V : 왼쪽/오른쪽 서브트리 처리, 현재 노드 처리
- 왼쪽을 오른쪽보다 먼저 처리하는 제약을 건다.
  - LVR : Inorder traversal
  - VLR : Preorder traversal
  - LRV : Postorder traversal

### 5.3.6 Level-Order Traversal

- Queue 기반 (상술된 것들은 stack 기반이었음)
  - Root를 출력하고 큐에 그 자식들을 넣는다.
  - 그 이후 하나를 꺼내고 그 자식들을 큐에 넣는다.
  - 반복

## 5.5 Threaded Binary Trees

- $n+1$개의 포인터가 늘 남는데, 이걸 쓰레드로 활용하자!
- Inorder traversal은 스택을 썼었는데, 쓰레드를 사용하면 스택을 안써도 된다.
- 쓰레드가 inorder traversal order에 맞춰 앞, 뒤 노드를 가리키게 할 거예요.
  - 앞 노드를 predecessor, 뒤 노드를 successor라고 한다.
- 처음과 마지막 노드는 포인터가 하나씩 비는데요?
  - Root 노드 위에 헤더 노드를 두고 그 헤더를 가리키게 하자
  - 헤더 노드의 right child pointer는 무조건 자기 자신을 가리킨다.
  - Left child pointer가 root를 가리키게 한다.
  - 만약 empty-tree라면 left child pointer가 자기 자신을 가리키고 `왼쪽 쓰레드=True`로 바꾼다.
- Child를 나타내는 포인터와 쓰레드를 어떻게 구별하죠?
  - 구분자에 해당하는 변수를 추가하자
  - 필드가 2개 늘어난다. (왼쪽/오른쪽 포인터가 쓰레드인가? T/F)
- 어떻게 이 트리로 재귀도 아니고 스택도 안쓰면서 inorder traversal order을 만들지?
  - 트리의 가장 왼쪽으로 내려가고 본인 출력, 이후 오른쪽 포인터를 따라가며 출력한다.
  - 근데 그 포인터가 쓰레드가 아니면 우선 그 오른쪽 자식 노드로 가고 거기서 왼쪽으로 쭉 내려간다.
  - 이후 제일 끝까지 내려갔다면(`왼쪽 쓰레드=True`), 거기부터 오른쪽으로 따라가며 출력하면 된다.
- 뭐 비는 포인터를 활용하긴 하지만 필드 더 추가했으면 이득인가?
  - 그래도 포인터 비우는 거보단 낫다.
  - 스택을 안쓰는게 더 이득

## 5.6 Heaps

### 5.6.1 Priority Queues

- 큐와 유사하지만 각 item들이 우선 순위를 가지고 있다. (숫자)
- 들어갈 땐 그냥 뒤로 들어가지만 나올 때는 내가 정한 기준대로 나온다. (높거나 낮거나)
- Priority queue는 가장 우선 순위가 높은게 출력됨
- OS가 프로세스를 CPU에 할당할 때 사용한다.
- 연산 시간 복잡도
  - `Top()` : 힙을 쓸 때가 더 빠르다.
  - `Push()` : 배열이나 리스트가 더 빠르다. (큰 차이가 없다.)

### 5.6.2 Definition of a Max Heap

- Max(min) tree : 부모 노드의 값은 자식 노드보다 작지 않다. (그 반대)
- Max heap : Max tree 중에 complete binary tree인 것
- 배열을 쓰는게 메모리를 좀 덜 먹고 빠르겠다.
- 이걸 갖고 priority queue를 만들어보자!
  - Key값이 그 노드의 priority라고 생각하자
  - 가장 큰 값은 root에 있다. (`Top()`의 시간 절약)

### 5.6.3 Insertion into a Max Heap

1. 힙에서 가장 뒷 자리에 넣는다.
2. 들어간 후 tree가 max heap을 만족하는지 확인하고 만족한다면 stop
3. 아니라면 그 부모와 비교해서 자리를 바꾼다.
4. 만족할 때까지 계속

---

- 자기 자리를 찾아 올라가는 과정을 **bubble-up**이라고 부른다.
- 부모가 나보다 크거나 내가 root까지 갈 때까지 올라간다.
- 그래봤자 height만큼 올라가니까 시간 복잡도는 $O(log\ n)$

### 5.6.4 Deletion from a Max Heap

1. 우선순위가 가장 높은 노드를 버려야 함 = Root를 버린다.
2. 맨 뒤에 있는 노드를 root로 올린다.
3. 자식 노드 2개를 비교해서 더 높은 걸 올리고 내가 내려간다.
4. 만족할 때까지 계속

---

- 이것도 자기 자리를 찾아서 내려가야 하는데, 해봤자 tree의 키 만큼이니 $O(log\ n)$
- **Trickle down**이라고 부른다.

## 5.7 Binary Search Trees

- Dictionary 정의에 사용
- `virtual` : 정의가 안되어있을 수 있음 (유도받아 정의해서 사용)
- `pair<K, E>` : K와 E를 묶어서 가지고 있는 클래스
- 특징
  - 키값은 모두 유니크하다.
  - 왼쪽 서브트리의 키는 root보다 작다.
  - 오른쪽 서브트리의 키는 root보다 크다.
- 실상 정렬이 되어 있다.
  - 정렬된 순서의 searching이 가능하다. (e.g., 몇 번째로 큰 원소는?)
  - Rank : Inorder traversal을 했을 때의 순서와 같다.
  - `leftSize` : 내 왼쪽 서브트리의 노드의 개수 +1
  - 오른쪽으로 갈 땐 찾고자 하는 rank에서 `leftSize`를 빼고 내려가면 된다.

### 5.7.3 Insertion into a Binary Search Tree

- 트리를 만드는 것도 가능해진다.
- 탐색을 죽- 하다가 삽입하려는 값이 존재하면 insert 안하면 된다. (X)
  - `second`를 바꾸는 것 (value를 바꿔줘야 한다.)
  - 없다면 search가 실패한 그 자리에 삽입하면 된다.

### 5.7.4 Deletion from a Binary Search Tree

- 자식이 없는지, 하나 있는지, 두 개 있는지 확인해야 한다.
  - 자식이 없다면 그냥 지우고 부모의 포인터를 NULL로
  - 하나라면 지우고 하나 뿐인 child를 올리면 된다.
  - 두 개라면 왼쪽에서 가장 큰 걸 올리거나 오른쪽에서 가장 작은 걸 올린다.
    - 왼쪽에서 가장 큰 노드는 자식이 어차피 하나 뿐이라 삭제가 쉽다.
- 이런 것들이 모두 트리의 height에 영향을 받는데, height는 어떻게 정해질까?
  - Insertion 순서가 중요하다.
  - 가장 좋을 땐 $log_2n$이 될 듯
  - 보통은 $O(h)$

