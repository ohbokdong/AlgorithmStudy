# 우선순위 큐와 힙

## 23.1 도입

우선순위 큐는 순서대로 기다리고 있는 자료들을 저장하는 자료 구조라는 점에서 큐와 비슷  
하지만 먼저 입력된 순서가 아니라 우선순위가 높은 자료가 먼저 꺼내진다는 차이가 있음  
대표적인 구조는 힙(heap)  
힙은 가장 큰 원소를 찾는 데 최적화된 형태의 이진 트리, 새 원소를 추가하는 연산과 가장 큰 원소를 꺼내는 연산을 모두 O(lgN) 시간에 수행 가능

## 23.2 힙의 정의와 구현

실제로는 여러 힙의 종류가 있지만 이 장에서 다루는 것은 이진 힙(binary heap)  
가장 중요한 규칙은 부모 노드가 가진 원소는 항상 자식 노트가 가진 원소 이상이라는 것.  
왼쪽 자식과 오른쪽 자신이 갖는 원소의 크기는 제한하지 않는다는 데 유의.  
가장 큰 원소는 root.

**힙의 모양 규칙**

- 마지막 레벨을 제외한 모든 레벨에 노드가 꽉 차 있어야 한다.
- 마지막 레벨에 노드가 있을 때는 항상 가장 왼쪽부터 순서대로 채워져 있어야 한다.

### 배열을 이용한 힙의 구현

힙의 엄격한 모양 규칙 덕분에 트리에 포함된 노드의 개수만 알면 트리 전체의 구조를 알 수 있다.

- A[i]에 대응되는 노드의 왼쪽 자손은 A[2✖i➕1]에 대응
- A[i]에 대응되는 노드의 오른쪽 자손은 A[2✖i➕2]에 대응
- A[i]에 대응되는 노드의 부모는 A[(i➖1)/2]에 대응(나눗셈 결과는 내림)

정수를 저장하는 힙은 다음과 같이 만들 수 있다

```C++
vector<int> heap;
```

윗 레벨부터, 왼쪽에서 오른쪽의 순서대로 저장함

[javascript로 구현한 힙](https://1ilsang.dev/2020-04-02/js/heap)

### 새 원소의 삽입

새로 들어오면 밀려내려가는 원소는 모양 규칙에 따라 어느 서브트리로 들어가야 할 지가 정해짐.  
대소 관계 규칙 대신 모양 규칙을 먼저 만족 시키면 번거로움을 피할 수 있다.

1. 모양 규칙에 의해 새 노드는 항상 heap[]의 맨 끝에 추가된다. 일단 대소 관계 무시, 이 위치에 새 노드 추가해서 모양 규칙 만족 시킴.
2. 마지막에 추가한 새 원소를 자신의 부모 노드가 가진 원소와 비교하여 부모 노드가 가진 원소가 작으면 두 원소의 위치를 바꾸면 됨.
3. 새 원소가 더 크거나 같은 원소를 가진 부모를 만나거나, 루트에 도달할 때까지 위 방법을 반복

while문의 내부가 한 번 수행될 때마다 트리에서 한 레벨 위로 올라가므로, push_heap()의 시간 복잡도는 트리의 높이인 O(lgN)

```C++
// 코드 23.1 정수 원소를 갖는 최대 힙에 새 원소 삽입하기

// 정수를 담는 최대 힙heap에 새 원소 newValue를 삽입
void push_heap(vector<int>& heap, int newValue) {
  // 힙의 맨 끝에 newValue를 삽입
  heap.push_back(newValue);
  // 현재 newValue의 위치
  int idx = heap.size() - 1;
  // 루트에 도달하거나 newValue 이상의 원소를 가진 조상을 만날 때까지
  while(idx > 0 && heap[(idx  - 1) / 2] < heap[idx]) {
    swap(heap[idx], heap[(idx - 1) / 2]);
    idx = (idx - 1) / 2;
  }
}
```

### 최대 원소 꺼내기

최대 원소 찾기 = 배열의 첫 원소를 확인  
하지만 힙에서 꺼내는 일이 힘듬 = 루트를 지우는 작업  
마지막 노드는 리프이므로 지워져도 구조에 영향을 미치지 않으며, 얘를 루트에 넣으면 다시 힙의 모양 규칙을 만족하게 됨  
이후, 두 자식 노드가 가진 원소 중 더 큰 원소를 선택해 루트가 갖고 있는 원소랑 맞바꾸는 작업을 반복하면 된다.  
while문의 내부가 한 번 실행될 때마다 트리의 아래 레벨로 내려가기 때문에, 시간 복잡도는 O(lgN)이 된다

```C++
// 코드 23.2 정수 원소를 갖는 최대 힙에서 최대 원소 삭제하기

// 정수를 담는 최대 힙 heap에서 heap[0]을 제거한다.
void pop_heap(vector<int>& heap) {
  // 힙의 맨 끝에서 값을 가져와 루트에 덮어씌운다.
  heap[0] = heap.back();
  heap.pop_back();
  int here = 0;
  while(true) {
    int left = here * 2 + 1, right = here * 2 + 2;
    // 리프에 도달한 경우
    if(left >= heap.size()) break;
    // heap[here]가 내려갈 위치를 찾는다.
    int next = here;
    if(heap[next] < heap[left])
      next = left;
    if(right < heap.size() && heap[next] < heap[right])
      next = right;
    if(next == here) break;
    swap(heap[here], heap[next]);
    here = next;
  }
}
```

### 다른 연산들

힙이 지원하는 다른 연산들

1. 힙을 만드는 연산  
   N개의 원소를 텅 빈 힙에 순서대로 삽입해야 할 때 쓸 수 있다. 길이 N인 배열에 저장한 뒤 힙을 만드는 연산을 수행하면 O(N)만에 힙으로 만들어 준다. push_heap()을 N번 호출하는 O(NlgN)에 비해 빠르지만, 어차피 N개의 원소를 다시 꺼내는데 시간이 걸리므로 의미는 없다.
2. 힙 정렬(Heapsort)  
   힙에서 원소를 꺼낼 때 항상 정렬된 순서대로 반환된다는 점을 이용한다. 병합 정렬과 달리 추가적인 메모리를 요구하지 않으면서, 최악의 경우에도 O(NlgN)시간 복잡도를 요구하기 때문에 유명함
3. 이미 힙에 들어있는 원소 중 하나를 증가시키기  
   힙을 이용해 우선순위 큐를 구현할 때 유용하게 사용된다. 새 원소를 삽입할 때처럼 변경된 원소를 위로 올려 주는 방식으로 구현할 수 있는데, 각 원소가 힙의 어디에 위치하는지를 별도의 배열에 유지해야 하는 등 구현이 번거로움

## 23.3 문제: 변화하는 중간 값 (문제 ID: RUNNINGMEDIAN)

한 수열의 중간 값(median)은 이 수열을 정렬했을 때 가운데 오는 값을 말한다.
짝수일 때는 가운데 있는 두 값 중 작은 것을 중간 값이라고 정의하도록 하자. 새로운 수가 추가될 때마다 중간 값은 변한다.

### 입력 생성

- A[0] = 1983
- A[i] = (A[i-1] x a + b) mod 20090711

## 23.4 풀이: 변화하는 중간 값

균형잡힌 이진 검색 트리를 사용하기  
26.6절에서 사용한 트립을 사용

```C++
// 코드 23.3 변화하는 중간 값 문제를 트립을 사용해 풀기

// mg가 생성하는 첫 n개의 난수를 수열에 추가하며 중간 값을 구한다.
int runningMedian(int n, RNG rng) {
  Node* node = NULL;
  int ret = 0;
  for(int cnt = 1; cnt <=n; ++cnt) {
    root = insert(root, new Node(rng.next()));
    int median = kth(root, (cnt + 1) / 2) -> key;
    ret = (ret + median) % 20090711;
  }
  return ret;
}
```

이진 검색 트리를 사용하지 않고 푸는 한 방법은 주어진 수열의 숫자들을 두 묶음으로 나누는 것. 숫자들을 정렬한 뒤 앞의 절반을 최대 힙, 뒤의 절반을 최소 합에 넣는다.

1. 최대 힙의 크기는 최소 힙의 크기와 같거나, 하나 더 크다.
2. 최대 힙의 최대 원소는 최소 힙의 최소 원소보다 작거나 같다.

C++ STL에 포함된 힙의 구현인 priority_queue를 쓰고 있는 점을 보기

```c++
// 코드 23.4 힙을 이용해 변화하는 중간 값 문제를 해결하는 함수의 구현

int runningMedian(int n, RNG rng) {
  priority_queue<int, vector<int>, less<int> > maxHeap;
  priority_queue<int, vector<int>, greater<int> > minHeap;
  int ret = 0;
  // 반복문 불변식:
  // 1. maxHeap의 크기는 minHeap의 크기와 같거나 1 더 크다.
  // 2. minHeap.top() <= minHeap.top()
  for (int cnt = 1; cnt <= n; ++cnt) {
    // 우선 1번 불변식부터 만족시킨다.
    if(maxHeap.size() == minHeap.size())
      maxHeap.push(rng.next());
    else
      minHeap.push(rng.next());
    // 2번 불변식이 깨졌을 경우 복구하자.
    if(!minHeap.empty() && !maxHeap.empty() && minHeap.top() < maxHeap.top()) {
      int a = maxHeap.top(), b = minHeap.top();
      maxHeap.pop(); minHeap.pop();
      maxHeap.push(b);
      minHeap.push(a);
    }
    ret = (ret + maxHeap.top()) % 20090711;
  }
  return ret;
}
```

runningMedian도 시간 복잡도는 O(NlgN).

### 수열 생성하기

입력의 양이 많을 때는 난수 생성기를 통해 입력  
연산 과정에서 정수 오버플로우가 나지 않도록 64비트 정수 캐스팅도 눈여겨 보기

```C++
// 코드 23.5 변화하는 중간 값 문제의 입력 생성하기
struct RNG {
  int seed, a, b;
  RNG(int _a, int _b) : a(_a), b(_b), seed(1983) {}
  int next() {
    int ret = seed;
    seed = ((seed * (long long)a) + b) % 20090711;
    return ret;
  }
}
```
