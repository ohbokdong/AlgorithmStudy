# 21. 트리의 구현과 순회

## 21.1 도입

계층적 구조를 갖는 자료들을 표현하기 위한 자료 구조가 바로 트리(tree)  
트리는 현실 세계의 개념을 추상화해 표현하는 자료 구조로 고안되기도 하며, 탐색형 자료 구조로도 유용하게 쓰인다.  
특정한 조건을 지키도록 구성된 트리들을 이용하면 배열이나 리스트를 사용하는 것보다 더 빠른 속도가 가능함.

### 트리의 구성요소

> 기초적인 정의와 용어

`트리`: 자료가 저장된 노드(node)들이 간선(edge)으로 서로 연결되어 있는 자료 구조  
`부모(parent)`: 두 연결된 노드 중 상위 노드  
`자식(child)`: 하위 노드  
`형제(sibling)`: 부모가 같은 두 노드  
`선조(ancestor)`: 부모 노드와 그의 부모들 모두  
`자손(descendant)`: 자식 노드와 그의 자식들 모두  
`뿌리 노드 혹은 루트(root)`: 최상위 요소  
`트리의 잎 노드 혹은 리프(leaf)`: 자식이 하나도 없는 노드들

> 트리와 노드의 속성

노드에 도달하기 위해 거쳐야 하는 간선의 수: `해당 노드의 깊이(depth)`  
가장 깊숙히 있는 노드의 깊이: `해당 트리의 높이(height)`

> 트리의 재귀적 속성

트리는 재귀적인 성질을 가지며 재귀 호출을 이용해 구현이 됨  
어떤 노드 t와 그 자손들로 구성된 트리를 't를 루트로 하는 서브트리(subtree)'라고 말함

> 트리의 표현

각 노드를 하나의 구조체/객체로 표현하고, 이들을 서로의 포인터로 연결하는 것

```C++
struct TreeNode {
  string lable; // 저장할 자료 (문자열일 필요는 없음)
  TreeNode* parent; // 부모 노드를 가리키는 포인터
  vector<TreeNode*> children; // 자손 노드들을 가리키는 포인터의 배열
}
```

모든 트리를 위와 같이 구현할 수 있는 것은 아니지만 보편적인 형태임

## 21.2 트리의 순회

모든 자료를 순회하기 위하여 트리의 재귀적 속성을 이용해야 한다.  
트리의 루트가 주어질 때 루트를 '방문'한 뒤 각 서브트리를 재귀적으로 방문하는 함수를 만들어 트리의 모든 노드를 순회할 수 있다.

```C++
// 코드 21.2 트리를 순회하며 모든 노드에 포함된 값을 출력하기

// 주어진 트리의 각 노드에 저장된 값을 모두 출력한다
void printLabels(TreeNode* root) {
  // 루트에 저장된 값을 출력한다.
  cout << root->label << endl;
  // 각 자손들을 루트로 하는 서브트리에 포함된 값들을 재귀적으로 출력한다.
  for (int i=0; i<root->children.size(); i++) {
      printLabels(root->children[i]);
  }
}
// cout: <<뒤 내용을 출력하는 것
// endl: 줄 바꿈
```

서브트리의 개념을 이요하면 트리의 높이 또한 재귀적으로 정의할 수 있다.  
루트의 각 자식들을 루트로 하는 서브트리들의 높이 중 최대치 + 1 = 트리의 높이

```C++
// code 21.3 순회를 이용해 트리의 높이를 계산하기

// root를 루트로 하는 트리의 높이를 구함
int height(TreeNode* root) {
  int h = 0;
  for (int i=0; i<root->children.size(); i++) {
      h = max(h, 1 + height(root->children[i]));
  }
  return h;
}
```

**트리 순회 소요시간**  
n개의 노드가 있다고 했을 때 O(n)의 시간이 들어야 정상.  
실제로 for문 내부가 한 번 수행될 때마다 printLabels()가 호출되는데, 이 함수는 트리의 모든 노드에 대해 한 번씩만 호출되므로 for문 내부의 전체 수행 회수는 정확히 n-1번이다.

### 21.3 문제: 트리 순회 순서 변경 (문제 ID: TRAVERSAL)

트리의 방문 순서는 필요에 맞춰 정의해줘야 함.

- 이진 트리(binary tree)는 모든 노드의 왼쪽과 오른쪽, 최대 두 개의 자손이 있는 트리
  - 전위 순회(preorder traverse): 트리의 루트 -> 왼쪽과 옹른쪽을 순서대로
  - 중위 순회(inorder traverse): 왼쪽과 오른쪽 서브트리 사이에 트리 루트 방문
  - 후위 순회(postorder traverse): 왼쪽과 오른쪽 서브트리 모두 방문한 뒤 루트 방문

참고: https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week10/younggeun0.md/#%ED%8A%B8%EB%A6%AC%EC%9D%98-%EC%88%9C%ED%9A%8C-1

**문제**

- 어떤 이진 트리를 전위 순회했을 때 노드들의 방문 순서와 중위 순회했을 때 노드들의 방문 순서가 주어졌을 때, 후위 순회했을 때 노드들을 어떤 순서대로 방문하게 될지 계산하는 문제

- 입력
  - 테스트 케이스 수 C (C<=1<=100)
  - 트리에 포함된 노드의 수 N(1<=N<=100)
  - 트리를 전위 순회했을 때, 중위순회 했을 때의 노드 방문 순서가 N개의 정수로 주어짐
  - 각 노드는 1000 이하의 자연수로 번호가 매겨져 있으며 한 트리에 두 노드의 번호가 같은 일은 없음
- 출력
  - 각 테스트 케이스마다 한 줄에 해당 트리의 후위 순회했을 때 노드들의 방문 순서를 출력

```c++
// 예제 입력
1 // C
7 // N
27 16 9 12 54 36 72 // Pre
9 12 16 27 36 54 72 // Inorder

// 예제 출력
12 9 16 36 72 54 27
```

### 21.4 풀이: 트리 순회 순서 변경

재귀 호출을 이용하면 아주 간단하게(ㅎ..) 구현할 수 있다

```
printPostOrder(preorder[], inorder[]) = 트리의 전위 순회 순서 preorder[]와 중위 순회 순서 inorder[]가 주어질 때, 후위 순회 순서를 출력한다.
```

- preorder[0]이 루트의 번호: 전위 순회는 루트에서 시작하므로
- inorder[]에서 루트의 위치를 찾으면 서브트리의 크기 L, R을 알 수 있음
- printPostOrder()의 수행 시간을 지배하는 것은 선형 시간이 걸리는 std :: find()와 slice()함수의 호출
- 트리의 각 노드마다 printPostOrder()가 한 번씩 호출되므로 전체 시간 복잡도는 O(n<sup>2</sup>)

```c++
// code 21.4 트리 순회 순서 변경 문제를 해결하는 재귀 호출 코드

vector<int> slice(const vector<int>& v, int a, int b) {
  return vector<int>(v.begin() + a, v.begin() + b);
}

// 트리의 전위탐색 결과와 중위탐색 결과가 주어질 때 후위탐색 결과를 출력한다.
voide printPostOrder(const vector<int>& preorder, const vector<int>& indorer) {
  // 트리에 포함된 노드의 수
  const int N = preorder.size();
  // 기저 사례: 텅 빈 트리면 곧장 종료
  if (preorder.empty()) return;
  // 이 트리의 루트는 전위 탐색 결과로부터 곧장 알 수 있다.
  const int root = preorder[0];

  // 이 트리의 왼쪽 서브트리의 크기는 중위 탐색 결과에서 루트의 위치를 찾아서 알 수 있다.
  const int L = find(inorder.begin(), inorder.end(), root) - inorder.begin();

  // 오른쪽 서브트리의 크기는 N에서 왼쪽 서브트리와 루트를 빼면 알 수 있다.
  const int R = N - 1 - L;

  // 왼쪽과 오른쪽 서브트리의 순회 결과를 출력
  printPostOrder(slice(preorder, 1, L+1), slice(inorder, 0, L));
  printPostOrder(slice(preorder, L+1, N), slice(inorder, L+1, N));

  // 후위 순회이므로 루트를 가장 마지막에 출력한다.
  cout << root << ' ';
}
```
