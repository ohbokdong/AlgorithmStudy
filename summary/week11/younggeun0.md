## 알고리즘 문제해결 전략 22 - 이진 검색 트리

---

### 검색 트리

* 검색 트리(Search Tree)는 연결 리스트나 큐처럼 자료들을 담는 컨테이너지만, 자료들을 일정한 순서에 따라 정렬한 상태로 저장
* **검색 트리 중 가장 흔하게 사용되는 것이 이진 검색 트리(Binary Search Tree)**
  * 대부분 언어의 표준 라이브러리에서 제공, 직접 구현할 일이 많지 않음
  * 가끔 라이브러리가 제공하지 않는 연산이 필요한 경우, 직접 구현해야 함


### 이진 검색 트리의 정의

* 이진 트리(Binary Tree)란 각 노드가 왼쪽 오른쪽, 최대 두 개의 자식 노드만을 가질 수 있는 트리
* 이진 트리는 자식 노드들의 배열 대신 두 개의 포인터 left, right를 담는 객체로 구현됨
* 이진 검색 트리는 이진 탐색(Binary Search)에서 아이디어를 가져와서 만든 트리
  * 이진 검색 트리의 각 노드는 하나의 원소를 갖고 있음
  * 각 노드의 왼쪽 서브트리에는 해당 노드의 원소보다 작은 원소를 가진 노드들이, 오른쪽 서브트리에는 보다 큰 원소를 가진 노드들이 들어감
  * 이진 검색트리에서 원하는 값을 찾는 과정은 배열에서의 이진 탐색과 비슷

<p align="center"><img src="https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week11/img/bst.png?raw=true"></p>

* **순회**
	* 이진 검색 트리(BST)를 중위 순회하면 크기 순서로 정렬된 원소의 목록을 얻을 수 있음
		* == 집합에 포함된 최대 원소나 최소 원소를 쉽게 얻을 수 있음
* **자료의 검색**
	* 이진 검색 트리의 특징 때문에 한 번 원소를 비교하는 것 만으로도 찾아야 할 전체 대상의 절반을 줄일 수 있음(이진 탐색과 비슷한 속도)
* **조작**
	* 집합에 원소를 추가하거나 삭제하는 조작 연산을 해야할 때 BST의 진가가 들어남
		* 배열에서 추가하기 위해선 삽입할 위치를 먼저 찾고(1) 그 이후에 있는 원소들을 뒤로 이동시켜야 함(2)
		* 이진 검색 트리에는 선형적인 구조 제약이 없기 때문에, 새 원소가 들어갈 위치를 찾고(1) 거기에 노드를 추가하기만 하면 됨(2)
	* BST에서 가장 까다로운 연산은 원소를 삭제하는 것
		* 새 원소 추가는 새 잎 노드가 생길 뿐이라 트리 전체 구조에 영향이 없지만 삭제는 트리 어디서는 삭제가 일어날 수 있음
		* BST 삭제 구현 방법은 여러가지 있지만 간편한 방법으로 '합치기' 연산을 구현하는 것이 있음
			* 노드 t를 지울 거라면, t의 두 서브트리를 합친 새로운 트리를 만든 뒤 이 트리를 t를 루트로 하는 서브트리와 바꿔치기 하는 것 (그림 22.2 참고)
* **X보다 작은 원소의 수 찾기/ k번째 원소 찾기**
	* 표준 라이브러리에서 지원하는 언어는 거의 없기 때문에 이런 연산이 필요할 경우 직접 이진 검색 트리를 구현해야 함
* **시간복잡도 분석과 균형 잡힌 이진 검색 트리**
	* 이진 검색 트리에 대한 모든 연산은 모두 루트에서부터 한 단계씩 트리를 내려가며 재귀 호출을 통해 수행됨
		* 최대 재귀 호출의 횟수는 트리의 높이 h와 같음, 때문에 모든 연산의 시간 복잡도가 트리의 높이 O(h)
	* **자료의 개수 N에 대해 변하는 트리의 높이 h**
		* 이진 검색 트리의 높이는 자료들이 어떤 순서로 추가되고 삭제되느냐에 따라 크게 달라짐
		* 숫차적으로 큰 숫자가 순서대로 N개 추가되면 트리의 높이는 N-1이 되고 트리는 **한 쪽으로 기울어진(skewed) 형태**를 띔
			* **skewed형태가 되면 연결 리스트와 같아지므로 이진 검색트리를 사용하는 의미가 없음**
		* 트리의 높이가 1 높아질 때마다 트리에 들어갈 수 있는 원소의 수가 대략 두 배 늘어나는 점을 고려하면 트리의 최소 높이는 O(logN)이 됨
			* 최악의 기울어진 트리는 입력이 특정 순서로 들어올 때만 발생하지만 무시할 수 없음
			* **이러한 단점을 개선한 이진 검색 트리의 변종들은 여러 가지 있고 이를 균형 잡힌 이진 검색 트리(Balanced Binary Search Tree)라고 함**
				* 이들은 트리의 구조에 추가적인 제약을 정하고 제약이 만족되도록 노드들을 옮겨서 트리의 높이가 항상 O(logN)이 되도록 유지함
					* 대표적인 예는 레드-블랙 트리(Red-Black Tree), 대부분의 표준 라이브러리에서 제공하는 이진 검색 트리의 구현도 내부적으로는 대개 레드-블랙 트리를 사용함

### 너드인가, 너드가 아닌가(NERD2)

#### 문제 

* **너드지수를 계산해 너드대회 참가자를 구하는 문제**
	* 너드지수는 온라인 채점 시스템으로 푼 문제의 수(p)와 밤 새며 먹은 라면 수 (q)가 높은 사람이 더 높음
* **입력**
	* 첫줄 : 테스트 케이스 수 C(1<=C<=0)
		* 테스트케이스 첫 줄 : 참가 신청한 사람들의 수 N(1<=N<=50000)
		* N 줄에 각 2개의 정수로 각 사람의 문제 수 pi와 라면 수 qi가 참가 신청한 순서대로 주어짐
			* 0<=pi, q<= 100000)
			* 두 사람의 문제 수나 라면 그릇 수가 같은 경우는 없다고 가정
	* 입력양이 많으므로 가능한 빠른 입력 함수를 사용하는 게 좋음

* **출력**
	* 각 사람이 참가 신청을 할 때마다 대회 참가 자격이 되는 사람의 수를 계산한 뒤, 각 테스트 케이스마다 그 합을 출력
		* == 참가 자격이 있는 사람들의 변화량의 총합

```
// 예제 입력
2 // C

4 // N
72 50 // pi, qi
57 67
74 55
64 60

5
1 5
2 4
3 3
4 2
5 1

// 예제 출력
8
15
```

#### 풀이

* 입력이 두 개라는 점을 이용, 2차원 평면에 입력을 그려보는 전략(그림 22.3)
  * 한 점 a가 다른 점 b보다 오른쪽 위에 있을 때 a가 b를 '지배한다'라고 표현
  * 지배당하지 않는 점 == 참가자
* **지배당하지 않는 점들은 왼쪽 위에서 오른쪽 아래로 가는 계단 모양을 이룸**
  * 지배당하지 않는 점들만 모아 x좌표가 증가하는 순서대로 정렬해 보면 y 좌표는 항상 감소
  * 지배당하지 않는 모든 x점들을 좌표 순서대로 정렬해 저장
    * **점 q가 주어졌을 때, 바로 오른쪽에 있는 점만 확인하면 이 점이 지배당하는지 아닌지를 알 수 있음**
    * 바로 오른쪽에 있는 점을 찾는 연산, 점의 추가와 삭제 등을 모두 빠르게 할 수 있는 자료구조가 있으면 이 문제를 빨리 풀 수 있음
    	* 이진 검색 트리는 모든 연산을 O(logn)시간에 할 수 있어 적합함
    	* STL의 균형잡힌 이진 검색 트리 구현인 map<int, int>를 이용해 각 점의 정보를 저장함
    		* map<int, int>::lower_bound(x)는 트리에 포함된 x이상의 키 중 가장 작은 값을 돌려주어 문제에 적합

```c++
// code 22.1 한 점이 다른 점에 지배당하는지 확인하는 함수
// 현재 다른 점에 지배당하지 않는 점들의 목록을 저장
// coords[x] = y

map<int, int> coords;
// 새로운 점 (x,y)가 기존의 다른 점들에 지배당하는지 확인
bool isDominated(int x, int y) {
    // x보다 오른쪽에 있는 점 중 가장 왼쪽에 있는 점을 찾음
    map<int, int>::iterator it = coords.lower_bound(x);

    // 그런 점이 없으면 (x, y)는 지배당하지 않음
    if (it == coords.end()) return false;

    // 이 점은 x보다 오른쪽에 있는 점 중 가장 위에 있는 점, (x,y)가 어느 점에 지배되려면 이 점도 지배되어야 함
    return y < it->second;
}
```

* 트리에서 새로운 점 q에 지배당하는 점들을 모두 지워야 함
	* q왼쪽에 있는 점에서부터 시작해 왼쪽으로 움직이면서 지배되는 점들을 지움
		* 지배되는 점들을 삭제하는 함수 removeDominated()
	* q에 지배당하지 않는 점이 등장하면 종료
		* 문제를 해결하는 함수 registered()

```c++
// code 22.2 지배되는 점들을 삭제하는 함수

// 새로운 점(x,y)에 지배당하는 점들을 트리에서 지움
void removeDominated(int x, int y) {
    map<int, int>::iterator it = coords.lower_bound(x);

    if (it == coords.begin()) return; // (x,y)보다 왼쪽에 있는 점이 없을 때

    --it;

    // 반복문 불변식 : it는 (x,y)의 바로 왼쪽에 있는 점
    while(true) {
        // (x,y) 바로 왼쪽에 오는 점을 찾음
        // it가 표시하는 점 (x,y)에 지배되지 않는다면 곧장 종료
        if (it->second > y) break;

        if (it == coords.begin()) { // 이전 점이 없으면 it만 지우고 종료
            coords.erase(it);
            break;
        } else {
            // 이전 점으로 이터레이터를 하나 옮겨놓고 it를 지움
            map<int, int>::iterator jt = it;
            --jt;
            coords.erase(it);
            it = jt;
        }
    }
}

// 새 점(x,y)가 추가되었을 때 coords를 갱신하고,
// 다른 점에 지배당하지 않는 점들의 개수를 반환
int registered(int x, int y) {
    // (x,y)가 이미 지배당하는 경우에는 그냥 (x,y)를 버림
    if (isDominated(x, y)) return coords.size();

    // 기존에 있던 점 중 (x, y)에 지배당하는 점들을 지움
    removeDiminated(x, y);
    coords[x] = y;
    return coords.size();
}
```


* registered가 N번 호출 시 시간복잡도
	* isDominated가 이진 검색 트리 탐색이므로 O(logn)
	* removeDominated는 몇 개의 점이 없어지느냐에 따라 수행시간이 달라짐
		* 그러나 한 점은 최대 한 번만 지워질 수 있기 때문에 지워지는 점의 개수는 도합 N-1
	* 따라서 전체 시간 복잡도는 O(Nlogn)


### 균형 잡힌 이진 검색 트리 직접 구현하기 : 트립

* X보다 작은 원소의 수 찾기/ k번째 원소 찾기 연산은 표준 라이브러리에서 지원하는 언어는 거의 없기 때문에 이런 연산이 필요할 경우 직접 구현해야 함
	* 특정 형태의 입력 시 연결 리스트가 되어 버리는 문제가 있어 균형 잡힌 이진 트리를 구현해야 함
	* 교과서에 실리는 대부분의 균형 잡힌 이진 검색 트리 구현은 매우 까다로움(AVL 트리, 레드 블랙 트리 등)
	* **프로그래밍 대회에서는 이보다 구현이 간단한 이진 검색 트리 '트립(Treap)'을 사용**
* **트립의 정의**
	* 입력이 특정 순서로 주어질 때 그 성능이 떨어진다는 이진 검색 트리의 단점을 해결하기 위해 고안된 일종의 랜덤화된 이진 검색 트리
	* 이진 검색 트리와 같은 성질을 갖지만 트리의 형태가 원소들의 추가 순서에 따라 결정되지 않고 난수에 의해 임의대로 결정됨
		* 때문에 원소들이 어느 순서대로 추가, 삭제되더라도 트리 높이의 기대치는 항상 일정
		* 이를 위해 트립은 새 노드가 추가될 때마다 해당 노드에 우선순위(Priority)를 부여함
			* 우선순위는 원소의 대소 관계나 입력되는 순서와 아무 상관 없이 난수를 통해 생성
	* 트립은 항상 부모의 우선 순위가 자식의 우선순위보다 높은 이진 검색 트리를 만듦
		* == 우선순위가 높은 것부터 순서대로 추가한 이진 검색 트리
* **트립의 조건 두 가지**
	* 이진 검색 트리의 조건 : 모든 노드에 대해 왼쪽 서브트리에 있는 노드들의 원소는 해당 노드의 원소보다 작고, 오른쪽 서브트리에 있는 노드들의 원소는 해당 노드의 원소보다 큼
	* 힙의 조건 : 모든 노드의 우선 순위는 각자의 자식 노드보다 크거가 같음


* 트립의 높이
  * 트리의 높이 기대치는 O(logN), 증명은 p711 참고



<p align="center"><img src="https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week11/img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202021-04-17%20122131.png?raw=true"></p>


#### 트립의 구현

* 원소 key 외에도 우선순위 priority를 가짐
	* 노드를 생성할 때 rand()함수의 반환값으로 설정
* 자신을 루트로하는 서브트리에 포함된 노드를 저장하는 size 멤버
	* left, right가 바뀔 때마다 자동으로 갱신됨
	* k번째 원소를 찾는 연산이나 X보다 작은 원소를 세는 연산 등을 쉽게 구현 가능

```c++
// code 22.3 트립 노드를 표현하는 객체 구현
typeof int KeyType;

// 트립의 한 노드를 저장
struct Node {
  // 노드에 저장된 원소
  KeyType key;

  // 이 노드의 우선순위(Priority)
  // 이 노드를 루트로 하는 서브 트리의 크기(size)
  int priority, size;

  // 두 자식 노드의 포인터
  Node *left, *right;

  Node(const KeyType& _key) : key(_key), priority(rand()), size(1), left(NULL), right(NULL) {
  }

  void setLeft(Node* newLeft) { left = newLeft; calcSize(); }
  void setRight(Node* newRight) { right = newRight; calcSize(); }

  // size 멤버를 갱신
  void calcSize() {
    size = 1;
    if (left) size += left->size;
    if (right) size += right->size;
  }
};
```


#### 노드의 추가와 '쪼개기' 연산

* root를 루트로 하는 트립에 새 노드 node를 삽입 시, 먼저 root와 node의 우선순위 확인
	* root의 우선순위가 더 높은 경우 - node는 root 아래로 들어가야 함
		* 왼쪽 또는 오른쪽 서브트리로 가야할 지는 원소를 비교해서 정함
		* 재귀 호출을 통해 서브트리에 node를 삽입
	* node의 우선순위가 더 높은 경우 - node가 기존에 있던 루트 root를 밀어내고 트리의 루트가 되어야 함
		* 기존 트리에 포함되어 있던 모든 노드들은 모두 node의 자손이 되어야 함
		* **기존의 트리를 node가 가진 원소를 기준으로 '쪼개기' 연산을 해야 함**
			* node보다 작은 원소만 갖는 서브트리, 큰 원소만 갖는 서브트리를 만들어 node 양쪽 서브트리로 삽입
* **쪼개기 연산 구현**
	* root의 원소가 key 보다 작은 경우와 큰 경우로 나눠 생각할 수 있음
		* root의 원소가 더 작은 경우에 대한 설명
			* (a) - root의 원소가 더 작은 경우 트리의 원소들과 key의 대소관계를 보여줌
			* (b) - key를 기준으로 재귀적으로 서브트리를 쪼갬 
				* key보다 큰 원소가 있을 수 있는 곳은 점섬으로 표현된 오른쪽 서브트리
			* (c) - key보다 작은 원소를 갖는 트리를 root의 오른쪽 자손으로 연결하면 key 보다 작은 원소를 갖는 트리와 key 보다 큰 원소를 갖는 트리로 원래 트리가 쪼개짐


<p align="center"><img src="https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week11/img/image3.png?raw=true"></p>

```c++
// code 22.4 트립에서의 노드 추가와 트립 쪼개기 연산의 구현
typedef pair<Node*, Node*> NodePair;
// root를 루트로 하는 트립을 key 미만의 값과 이상의 값을 갖는 두 개의 트립으로 분리
NodePair split(Node* root, KeyType key) {
  if (root == NULL) return NodePair(NULL, NULL);

  // 루트가 key 미만이면 오른쪽 서브트리를 쪼갬
  if (root->key < key) {
    NodePair rs = split(root->right, key);
    root->setRight(rs.first);
    return NodePair(root, rs.second);
  }

  // 루트가 key 이상이면 왼쪽 서브트리를 쪼갬
  NodePair ls = split(root->left, key);
  root->setLeft(ls.second);
  return NodePair(ls.first, root);
}

// root를 루트로 하는 트립에 새 노드 node를 삽입한 뒤 결과 트립의 루트를 반환
Node* insert(Node* root, Node* node) {
  if (root == NULL) return node;

  // node가 루트를 대체해야 함, 해당 서브트리를 반으로 잘라 각각 자손으로 함
  if (root->priority < node->priority) {
    NodePair splitted = split(root, node->key);
    node->setLeft(splitted.first);
    node->setRight(splitted.second);
    return node;
  } else if (node->key < root->key) {
    root->setLeft(insert(root->left, node));
  } else {
    root->setRight(insert(root->right, node));
  }
  return root;
}
```

* 트립의 루트를 가리키는 포인터 root가 있을 때, 새 값 value를 다음과 같이 추가가능

```c++
root = insert(root, new Node(value);
```


#### 노드의 삭제와 '합치기' 연산

* 두 서브트리를 합칠 때 어느 쪽이 루트가 되어야 하는지를 우선순위를 통해 판단한다는 점을 제외하면 이진 검색 트리에서 노드 삭제와 다를 것이 없음
* 루트 노드를 지우면서 새로운 루트 노드를 반환하는 것을 유의

```c++
// code 22.5 트립에서 노드의 삭제와 합치기 연산의 구현
// a와 b가 두 개의 트립이고, max(a) < min(b)일 때 이 둘을 합침
Node* merge(Node* a, Node* b) {
  if (a == NULL) return b;
  if (b == NULL) return a;

  if (a->priority < b->priority) {
    b->setLeft(merge(a, b->left));
    return b;
  }
  a->setRight(merge(a->right, b));
  return a;
}

// root를 루트로 하는 트립에서 key를 지우고 결과 트립의 루트를 반환
Node* erase(Node* root, KeyType key) {
  if (root == NULL) return root;
  // root를 지우고 양 서브트리를 합친 뒤 반환
  if (root->key == key) {
    Node* ret = merge(root->left, root->right);
    delete root;
    return ret;
  }
  if (key < root->key) {
    root->setLeft(erase(root->left, key));
  } else {
    root->setRight(erase(root->right, key));
  }
  return root;
}
```

#### k번째 원소 찾기

* 이진 검색 트리를 직접 작성하는 것은 표준 라이브러리의 구현에서 제공하지 않는 기능이 필요할 때
* 위에 작성한 트립 Node 클래스는 서브 트리의 크기 size를 계싼해 저장해 두기 때문에 이것을 구현하기 쉬움
* 왼쪽 서브트리의 크기가 l일 때 
  * k <= l : k번째 노드는 왼쪽 서브트리에 속함
  * k = l + 1 : 루트가 k번째 노드
  * k > l + 1 : k번째 노드는 오른쪽 서브트리에서 k-l-1번째 노드가 됨
* kth 함수에서는 이 트리가 트립이라는 점에 대해 전혀 신경 쓰지 않음
  * 각 서브트리의 크기만 알 수 있으면 다른 종류의 이진 검색 트리 구현에서도 사용가능
  * kth 함수의 시간 복잡도는 트리 높이에 비례하기 때문에 트립 등의 균형 잡힌 이진 검색 트리에 적용해야만 O(logN) 시간에 수행가능

```c++
// code 22.6 트립에서 k 번째 원소를 찾는 알고리즘 구현
Node* kth(Node* root, int k) {
  // 왼쪽 서브트리의 크기를 우선 계산
  int leftSize = 0;
  if (root->left != NULL) leftSize = root->left->size;
  if (k <= leftSize) return kth(root->left, k);
  if (k == leftSize + 1) return root;
  return kth(root->right, k-leftSize-1);
}
```


#### X보다 작은 원소 세기

* 또 다른 유용한 연산으로 특정 범위 [a, b)가 주어질 때 범위 안에 들어가는 원소들의 숫자를 계산하는 연산이 있음
  * 이 연산은 주어진 원소 X보다 작은 원소의 수를 반환하는 countLessThan(X)가 있으면 간단하게 구현할 수 있음
  * [a, b) 범위 안에 들어가는 원소들의 수는 countLessThan(b) - countLessThan(a)로 표현가능
* countLessThan함수는 탐색 함수를 변형해 간단히 만들 수 있음
  * root의 원소가 X와 같거나 큰 경우
    * root와 그 오른쪽 서브트리에 있는 원소들은 모두 X 이상이므로 왼쪽 서브트리에 있는 원소들만을 재귀적으로 세서 반환
  * root의 원소가 X보다 작은 경우
    * 루트 왼쪽 서브트리에 있는 원소들은 모두 X보다 작으므로 재귀적으로 세지 않아도 전부 답에 들어감
    * 루트 오른쪽 서브트리에서 X보다 작은 원소들의 수를 재귀적으로 찾고, 이것에 왼쪽 서브트리에 포함된 노드 및 루트의 개수를 더해주면 됨
* 이 함수 또한 트립이 아니라 이진 검색 트리면 잘 동작함, 수행 시간 또한 트리의 높이에 비례함

```c++
// code 22.7 트립에서 X보다 작은 원소의 수를 찾는 알고리즘 구현
// key보다 작은 키 값의 수를 반환
int countLessThan(Node* root, KeyType key) {
  if (root == NULL) return 0;
  if (root->key >= key) {
    return countLessThan(root->left, key);
  }
  int ls = (root->left ? root->left->size : 0);
  return ls + 1 + countLessThan(root->right, key);
}
```