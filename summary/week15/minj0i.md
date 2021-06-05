# 29. 그래프의 너비 우선 탐색(Breadth First Search, BFS)

## 29.1 도입

- 30장 다익스트라의 최단거리, 31장 프림의 최소 스패닝 트리 알고리즘 등이 너비 우선 탐색을 골격으로 한다.
- **시작점에서 가까운 정점부터 순서대로 방문하는 탐색 알고리즘**

너비 우선탐색은 각 정점을 방문할 때마다 모든 인접 정점을 검사하고,  
처음 보는 정점 발견 시 이 정점을 방문 예정이라고 기록 후 별도의 위치에 저장한다.  
인접한 정점을 모두 검사 후, 지금까지 저장한 목록에서 다음 정점을 꺼내 방문한다.  
따라서 **어떤 정점을 먼저 꺼내는지에 의해 결정된다.**

먼저 넣은 정점을 항상 먼저 꺼내면 된다. => `큐`사용

```c++
// 코드 29.1 그래프의 너비 우선 탐색

// 그래프의 인접 리스트 표현
vector<vector<int> > adj;
// start에서 시작해 그래프를 너비 우선 탐색하고 각 정점의 방문 순서를 반환한다.
vector<int> bfs(int start) {
  // 각 정점의 방문 여부
  vector<bool> discovered(adj.size(), false);
  // 방문할 정점 목록을 유지하는 큐
  queue<int> q;

  // 각 정점의 방문 순서
  vector<int> order;
  discovered[start] = true;
  q.push(start);
  while(!q.emptry()) {
    int here = q.front();
    q.pop();
    // here를 방문한다.
    order.push_back(here);
    // 모든 인접한 정점을 검사한다.
    for(int i=0; i<adj[here].size(); ++i) {
      // 처음 보는 정점이면 방문 목록에 집어넣는다.
      if(!discovered[there]) {
        q.push(there);
        discovered[there] = true;
      }
    }
  }
  return order;
}
```

```js
// 오영근 작성
// JS
// 인접 리스트 DFS
var graph_array = [];
graph_array[0] = [];
graph_array[0].push(1);
graph_array[0].push(3);

graph_array[1] = [];
graph_array[1].push(0);
graph_array[1].push(2);

graph_array[2] = [];
graph_array[2].push(1);

graph_array[3] = [];
graph_array[3].push(0);
// console.log(graph_array);

function bfs(start, graph) {
    const discovered = [];
    const q = [];
    const order = [];

    // 발견 후 큐에 추가
    discovered[start] = true;
    q.push(start);

    while(q.length > 0) { // 큐가 빌 때까지
        const here = q.shift();
        // 큐에서 꺼내 순서에 추가
        order.push(here);

        // 정점에 인접한 정점이 발견되지 않았으면 발견여부 체크 후 큐에 추가
        for (let i=0; i<graph[here].length; i++) {
            const there = graph[here][i];
            if (!discovered[there]) {
                q.push(there);
                discovered[there] = true;
            }
        }        
    }
    return order;
}
console.log(bfs(0, graph_array));
```

깊이 우선 탐색 시 모든 정점이 거치는 순서

1. 아직 발견되지 않은 상태
2. 발견되었지만 아직 방문 전 (이 상태 정점들은 모두 큐에 저장)
3. 방문된 상태

너비 우선 탐색에서 새 정점을 발견하는데 사용했던 간선들만 모은 트리 = 너비 우선 탐색 스패닝 트리(BFS Spanning Tree)

### 너비 우선 탐색의 시간 복잡도

깊이 우선 탐색과 다를 바 없다. 정점 한 번씩 모두 방문, 방문 할 때마다 모든 간선 검사하기 때문에.

- **인접 리스트로 그래프를 표현하는 경우**
  - bfs() 호출 횟수는 V번
  - **O(|V| + |E|)**
- **인접 행렬로 그래프를 표현하는 경우**
  - bfs() 호출 횟수는 V번
  - **O(V<sup>2</sup>)**

### 너비 우선 탐색과 최단 거리

너비 우선 탐색은 `그래프`에서의 `최단 경로 문제`를 푸는 것에만 사용된다.  
`최단 경로 문제`는 두 정점을 연결하는 경로 중 가장 길이가 짧은 경로를 찾는 문제다.

- 경로의 길이를 경로에 포함된 간선의 갯수라고 정의
- 모든 정점에 대해 시작점으로부터의 거리 distance[]를 계산하도록 할 수 있음
  - 간선(u, v)를 통해 정점 v를 처음 발견해 큐에 넣는다고 했을 때, 시작점으로부터 v까지의 최단 거리 distance[v]는 시작점으로부터 u까지의 최단거리 distance[u] + 1 과 같기 때문이다
- 너비 우선 탐색 스패닝 트리를 보면, 각 정점으로부터 트리의 루트인 시작점으로 가는 경로가 실제 그래프 상에서의 최단 경로이다

[코드 29.2 설명]  
각 정점까지의 최단 거리와 너비 우선 탐색 스패닝 트리를 계산하는 bfs2(),  
너비 우선 탐색 스패닝 트리를 입력받아 실제 최단 경로를 계산하는 shartestPath()  
각 정점의 부모 번호만으로 표현함에 주목

```c++
// 코드 29.2 최단 경로를 계산하는 너비 우선 탐색

// start에서 시작해 그래프를 너비 우선 탐색하고 시작점부터 각 정점까지의
// 최단 거리와 너비 우선 탐색 스패닝 트리를 계산한다.
// distance[i] = start 부터 i 까지의 최단 거리
// parent[i] = 너비 우선 탐색 스패닝 트리에서 i의 부모의 번호, 루트인 경우 자신의 번호
void bfs2(int start, vector<int>& distance, vector<int>& parent) {
  distance = vector<int>(adj.size(), -1);
  parent = vector<int>(adj.size(), -1);
  // 방문할 정점 목록을 유지하는 큐
  queue<int> q;
  distance[start] = 0;
  parent[start] = start;
  q.push(start);
  while(!q.empty()) {
    int here = q.front();
    q.pop();
    // here의 모든 인접한 정점을 검사한다.
    for(int i = 0; i < adj[here].size(); ++i) {
      int there = adj[here][i];
      // 처음 보는 정점이면 방문 목록에 집어넣는다.
      if(distance[there] == -1) {
        q.push(there);
        distance[there] = distance[here] + 1;
        parent[there] = here;
      }
    }
  }
}

// v로부터 시작점까지의 최단 경로 계산
vector<int> shortestPath(int v, const vector<int>& parent) {
  vector<int> path(1, v);
  while(parent[v] != v) {
    v = parent[v];
    path.push_back(v);
  }
  reverse(path.begin(), path.end());
  return path;
}
```
```js
// 오영근 작성
// JS
// 인접 리스트 DFS
var graph_array = [];
graph_array[0] = [];
graph_array[0].push(1);
graph_array[0].push(3);


graph_array[1] = [];
graph_array[1].push(0);
graph_array[1].push(2);


graph_array[2] = [];
graph_array[2].push(1);


graph_array[3] = [];
graph_array[3].push(0);
// console.log(graph_array);

function bfs2(start, graph, distance, parent) {
    const q = [];
    distance[start] = 0;
    parent[start] = start;

    q.push(start);

    while (q.length > 0) {
        const here = q.shift();
        for (let i=0; i<graph[here].length; i++) {
            const there = graph[here][i];

            if (distance[there] == -1) { // distance[there]의 값이 0일 경우 false로 인식..
                q.push(there);
                distance[there] = distance[here] + 1;
                parent[there] = here;
            }
        }
    }    
}

// 길이, 부모값을 -1로 초기화
let distance = []; // 정점으로부터의 길이를 저장할 배열
let parent = []; // BST 의 부모값을 저장할 배열

for (let i=0; i<graph_array.length; i++) {
    distance[i] = -1;
    parent[i] = -1;
}

bfs2(0, graph_array, distance, parent);

console.log(distance); // [0, 1, 2, 1]
console.log(parent); // [0, 0, 1, 0]
```

### 모든 점의 발견

대개 시작점으로부터 다른 정점들까지의 거리를 구하기 위해 사용되기 때문에 모든 정점을 검사하면서 탐색을 수행하는 작업은 잘 하지 않음
