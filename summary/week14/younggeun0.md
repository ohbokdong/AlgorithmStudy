## 알고리즘 문제해결 전략 28 - 그래프의 깊이 우선 탐색

---

* **그래프의 모든 정점들을 특정한 순서에 따라 방문하는 알고리즘들을 그래프의 탐색(Serach) 알고리즘이라고 함**
* 트리 순회는 정점들을 확인하는 것 외에 큰 의미는 없지만 그래프는 트리보다 구조가 훨씬 복잡할 수 있어 탐색 과정에서 얻어지는 정보가 중요함
  * 어떤 간선이 사용되었는지, 어떤 순서로 방문되었는지를 통해 그래프의 구조를 알 수 있음
* 탐색 알고리즘 중 가장 널리 사용되는 **두 가지가 `깊이 우선 탐색(DFS, Depth First Search)`, `너비 우선 탐색(BFS, Breadth First Search)`**

![깊이,너비 우선 탐색](https://media.vlpt.us/images/adorno10/post/67d1a2f5-14c5-4242-8721-d03d6984bd18/image.png)

### 깊이 우선 탐색(DFS, Depth First Search)

* 그래프의 모든 정점을 발견하는 가장 단순하고 고전적인 방법
* **현재 정점과 인접한 간선들을 하나씩 검사하다가 아직 방문하지 않은 정점으로 향하는 간선이 있다면 그 간선을 무조건 따라감**
  * **더 이상 갈 곳이 없는 막힌 정점에 도달하면 마지막에 따라왔던 간선을 따라 뒤로 돌아감**
  * 뒤로 돌아가는 작업을 구현하기 위해선 지금까지 거쳐온 정점들을 모두 저장해 둬야 함
    * **재귀 호출**을 이용하면 뒤로 돌아가는 작업을 간단히 할 수 있음 **(Stack, LIFO)**

```c++
// code 28.1 그래프의 깊이 우선 탐색
// 그래프의 인접 리스트 표현
vector<vector<int>> adj;

// 각 정점을 방문했는지 여부를 나타냄
vector<bool> visited;

// dfs()는 아직 방문하지 않은 정점으로 이어지는 간선을 만날 경우 재귀 호출을 통해 해당 정점을 방문함
// 깊이 우선 탐색을 구현
void dfs(int here) {
    cout << "DFS visits " << here << endl;
    visited[here] = true;

    // 모든 인접 정점을 순회하면서
    for (int i=0; i<adj[here].size(); i++) {
        int there = adj[here][i];
        
        // 아직 방문한 적이 없다면 방문
        if (!visited[there])
            dfs(there);
    }
    // 더 이상 방문할 정점이 없으니, 재귀 호출을 종료하고 이전 정점으로 돌아감
}

// 모든 정점을 방문한다
void dfsAll() {
    // visited를 모두 false로 초기화
    visited = vector<bool>(adj.size(), false);

    // 모든 정점을 순회하며 아직 방문한 적 없으면 방문
    for (int i=0; i<adj.size(); i++)
        if (!visited[i])
            dfs(i);
}

// C++ 싫다..
```

![dfs_1](https://raw.githubusercontent.com/ohbokdong/AlgorithmStudy/main/summary/week14/dfs_1.png)

```js
// p828, 그림 28.2 형태로 예제 그래프를 만듦
// 각 정점을 방문했는지 여부를 나타냄
var visited;

// dfs()는 아직 방문하지 않은 정점으로 이어지는 간선을 만날 경우 재귀 호출을 통해 해당 정점을 방문함
// 깊이 우선 탐색을 구현
function dfs(here, graph) {
    console.log("DFS_visits : " + here);
    visited[here] = true;

    // 모든 인접 정점을 순회하면서
    for (var i=0; i<graph[here].length; i++) {
        var there = graph[here][i];
        
        // 아직 방문한 적이 없다면 방문
        if (!visited[there])
            dfs(there, graph);
    }
    // 더 이상 방문할 정점이 없으니, 재귀 호출을 종료하고 이전 정점으로 돌아감
}

// 모든 정점을 방문한다
function dfsAll(graph) {
    // visited를 모두 false로 초기화
    visited = [];
    for (var i=0; i<graph.length; i++)
        visited.push(false);

    // 모든 정점을 순회하며 아직 방문한 적 없으면 방문
    for (var i=0; i<graph.length; i++)
        if (!visited[i])
            dfs(i, graph);
}


// 인접 리스트 DFS
var graph_array = [];

graph_array[0] = [];
graph_array[0].push(1);
graph_array[0].push(2);

graph_array[1] = [];
graph_array[1].push(0);
graph_array[1].push(2);

graph_array[2] = [];
graph_array[2].push(1);

graph_array[3] = [];
graph_array[3].push(0);

graph_array[4] = [];
graph_array[4].push(6);
graph_array[4].push(5);
graph_array[4].push(7);

graph_array[5] = [];
graph_array[5].push(4);
graph_array[5].push(6);

graph_array[6] = [];
graph_array[6].push(4);
graph_array[6].push(5);
graph_array[6].push(7);

graph_array[7] = [];
graph_array[7].push(6);
graph_array[7].push(4);

console.log(graph_array);
console.log(dfsAll(graph_array));


// 인접 행렬 DFS, 8*8
var graph_matrix = [];
                // 0, 1 ,2, 3, 4, 5, 6, 7
graph_matrix[0] = [0, 1, 0, 1, 0, 0, 0, 0];
graph_matrix[1] = [1, 0, 1, 0, 0, 0, 0, 0];
graph_matrix[2] = [0, 1, 0, 0, 0, 0, 0, 0];
graph_matrix[3] = [1, 0, 0, 0, 0, 0, 0, 0];
graph_matrix[4] = [0, 0, 0, 0, 0, 1, 1, 1];
graph_matrix[5] = [0, 0, 0, 0, 1, 0, 1, 0];
graph_matrix[6] = [0, 0, 0, 0, 1, 1, 0, 1];
graph_matrix[7] = [0, 0, 0, 0, 1, 0, 1, 0];

console.log(graph_matrix);
console.log(dfsAll(graph_matrix));
```

* **그래프에서는 모든 정점들이 간선을 통해 연결돼 있다는 보장이 없음**
  * 때문에 서로 연결되지 않은 두 개의 부분으로 나뉜 그래프는 dfs() 만으로 모든 정점을 순서대로 발견할 수 없음
  * 대개 깊이 우선 탐색은 그래프 전체 구조를 파악하기 위해 사용되므로, 그래프의 구조상 한 번에 모든 정점을 다 볼 수 없는 경우에도 모든 정점을 다 방문할 필요가 있음
  
### **깊이 우선 탐색의 시간 복잡도**

* 그래프 표현 방식에 따라 성능은 달라짐
* **인접 리스트로 그래프를 표현하는 경우**
  * dfs() 호출 횟수는 V번
    * dfs() 한 번의 수행 시간은 모든 인접 간선을 검사하는 for문에 의해 지배됨
    * 모든 정점에 대해 dfs()를 수행하고 나면 모든 간선을 정확히 한번(방향 그래프) 혹은 두 번(무향 그래프) 확인함
  * **O(V + E)**
* **인접 행렬로 그래프를 표현하는 경우**
  * dfs() 호출 횟수는 V번
    * dfs() 내부에서 다른 모든 정점을 순회하며 두 정점 사이에 간선이 있는가를 확인해야 하기 때문에 한 번의 실행에 O(V)의 시간이 듦
  * **O(V<sup>2</sup>)**


### 두 정점이 서로 연결돼 있는가 확인하기

* **깊이 우선 탐색을 이용하면 그래프 상에서 두 정점 사이를 잇는 경로가 있는지 간단히 확인 가능**
  * 어떤 정점에 대해 dfs(u)를 수행하면, u에서부터 간선들을 통해 갈 수 있는 모든 정점을 다 방문
  * **dfs(u)를 수행하고 visited[]를 참조하면 u로부터 각 정점에 갈 수 있는지를 쉽게 확인가능**

### 연결된 부분집합의 개수 세기

* **무향 그래프가 간선으로 서로 연결돼 있지 않은 몇 개의 조각으로 쪼개져 있는 경우, 각 연결된 정점들의 부분 집합을 `컴포넌트(Component)` 혹은 `요소`라고 부름**
* **dfs() 함수는 시작한 정점에서 갈 수 있는 모든 정점을 방문**
  * dfs()를 호출하고 나면 같은 컴포넌트에 속한 점은 모조리 방문됨
  * **dfsAll()에서 dfs()를 호출하는 횟수를 세면 그래프가 몇 개의 컴포넌트로 나뉘어 있는지 쉽게 확인 가능**


### 위상정렬(Topological Sort)
* **의존성이 있는 작업들이 주어질 때, 이들을 어떤 순서로 수행해야 하는지 계산해줌**
  * 작업 B를 하기 전에 반드시 작업 A를 해야 한다면, 작업 B가 작업 A에 의존한다고 말함
* 각 작업을 정점으로 표현하고, 작업 간의 의존 관계를 간선으로 표현한 방향 그래프를 `의존성 그래프(Dependency Graph)`라고 함
  * 작업 v가 u에 의존한다면, 의존성 그래프는 간선 (u, v)를 포함하게 됨
* **의존성 그래프는 사이클이 없는 방향 그래프 (DAG)가 됨**
  * 모든 의존성이 만족되려면, 모든 간선이 왼쪽에서 오른쪽으로 가야 함
  * **이렇게 DAG의 정점을 배열하는 문제를 위상 정렬(Topological Sort)라고 함**
* **위상 정렬을 구현하는 가장 직관적인 방법은 들어오는 간선이 하나도 없는 정점들을 하나씩 찾아서 정렬 결과의 뒤에 붙이고, 그래프에서 이 정점을 지우는 과정을 반복하는 것**
  * **깊이 우선 탐색을 이용하면 dfsAll()을 수행하며 dfs()가 종료할 때마다 현재 정점의 번호를 기록하여 문제를 쉽게 해결 가능**
    * dfsAll()이 종료한 뒤 기록된 순서를 뒤집으면 위상 정렬 결과를 얻을 수 있음
    * dfs()가 늦게 종료한 정점일수록 정렬 결과의 앞에 옴

![topological_sort_0](https://raw.githubusercontent.com/ohbokdong/AlgorithmStudy/main/summary/week14/topological_sort_0.png)

```js
var graph = [];
graph[0] = [];
graph[0].push(1);
graph[1] = [];
graph[1].push(2);
graph[1].push(3);
graph[2] = [];
graph[2].push(3);
graph[3] = [];

var visited;
var topological_sort = [];

function dfs(here, graph) {
    console.log("DFS_visits : " + here);
    visited[here] = true;

    for (var i=0; i<graph[here].length; i++) {
        var there = graph[here][i];
        if (!visited[there])
            dfs(there, graph);
    }
    topological_sort.push(here);
}

function dfsAll(graph) {
    visited = [];
    for (var i=0; i<graph.length; i++)
        visited.push(false);

    for (var i=0; i<graph.length; i++)
        if (!visited[i])
            dfs(i, graph);
}

dfsAll(graph);
console.log(topological_sort); // [3, 2, 1, 0]

topological_sort = topological_sort.reverse();

console.log(topological_sort); // [0, 1, 2, 3], 위상 정렬 결과
```

![topological_sort_1](https://raw.githubusercontent.com/ohbokdong/AlgorithmStudy/main/summary/week14/topological_sort_1.png)

* **귀류법으로 알고리즘의 정당성을 증명가능** (p831 참고)
    * 귀류법이란 어떤 명제가 참임을 직접 증명하는 대신, 그 부정 명제가 참이라고 가정하여 그것의 불합리성을 증명함으로써 원래의 명제가 참인 것을 보여 주는 간접 증명법
    * 간선 (u, v)가 있다고 가정
      * == dfs(u) 후에 dfs(v)가 종료됨
      * dfs(u)는 종료하기 전 모든 인접한 간선을 보기 때문에 dfs(u)는 간선 (u, v)를 검사했을 것
        * 이 때 visited[v]는?
          * visited[v]가 거짓이면 dfs(u)는 dfs(v)를 재귀호출했을 것, dfs(v)가 종료한 후에 dfs(u)가 종료해 v는 u의 왼쪽에 있을 수 없음
          * visited[v]가 참이면 dfs(v)가 이미 한 번 호출됐어야 함, 그런데 dfs(v)가 dfs(u)보다 늦게 끝났다는 말은 dfs(v)는 현재 실행중이라는 의미
            * 이러기 위해선 dfs(v)에서 재귀 호출을 거쳐 dfs(u)가 호출되어야 하는데 그래프에 v에서 u로가는 경로가 있어야 함
            * 그러나 간선 (u, v)를 덧붙이면 사이클이되므로, 원래 그래프가 DAG라는 가정에 모순
      * 두 경우 모두 가정에 모순이므로 오른쪽에서 가는 간선 (u, v)는 존재할 수 없으며, 이 결과는 항상 적절한 위상 정렬 결과임

![위상정렬](https://gmlwjd9405.github.io/images/algorithm-topological-sort/topological-sort.png)

### 고대어 사전 문제

* **사전에 포함된 단어들의 목록이 순서대로 주어질 때 이 언어에서 알파벳의 순서를 계산하는 프로그램을 작성하시오**
* 입력
  * 테스트 케이스 C(C<=50)
  * 사전에 포함된 단어 수 n(1<= n <= 200)
  * n줄에 하나씩 사전에 포함된 단어가 순서대로 주어짐
    * 알파벳 소문자로 구성, 길이는 1이상 20이하, 중복 없음
* 출력
  * 각 테스트 케이스마다 한 줄을 출력
  * 알파벳들의 순서에 모순이 있다면 "INVALID HYPOTHESIS" 출력
  * 모순  없으면 26개의 소문자로 알파벳들의 순서를 출력
  * 가능한 순서가 여러 개라면 아무 것이나 출력해도 됨
* 예제 p833 참고

### 고대어 사전 풀이

* **순서들을 표현하는 좋은 방법은 방향 그래프로 표현하는 것**
  * 각 알파벳을 정점으로 표현
  * 한 알파벳이 다른 알파벳 앞에 와야 할 때 두 정점을 방향 간선으로 연결
  * == 알파벳들의 순서는 그래프의 위상 정렬 결과가 됨
* 그래프는 각 알파벳의 소문자를 표현하는 26개의 정점을 가지며, 글자 i가 j보다 먼저 와야 할 경우 (i, j)를 가짐
  * 단어가 뒤에 오는 단어의 접두사인 경우 알 수 없어 필요 시 추가적인 예외처리가 필요

```js
// code 28.2 고대어 사전 문제의 그래프를 생성

// 알파벳의 각 글자에 대한 인접 행렬 표현
// 간선 (i, j)는 알파벳 i가 j보다 앞에 와야 함
vector<vector<int>> adj;

// 주어진 단어들로부터 알파벳 간의 선후관계 그래프를 생성
void makeGraph(const vector<string>& words) {
    adj = vector<vector<int>(26, vector<int>(26, 0)); // 소문자 26개
    for (int j=1; j<words.size(); j++) {
        int i = j - 1;

        // i번째 word와 j번째 word 중 짧은 길이(비교할 길이)
        int len = min(words[i].size(), words[j].size());

        // word[i]가 word[j] 앞에 오는 이유를 찾음
        for (int k = 0; k < len; k++) {
            if (words[i][k] != words[j][k]) {
                int a = words[i][k] - 'a'; // ascii 'a'를 빼서 26 중 알파벳의 위치를 계산
                int b = words[j][k] - 'a';

                adj[a][b] = 1; // 간선 연결
                break;
            }
        }
    }
}
```

* 인접한 단어들만 검사하기 - 증명 생략
* 위상 정렬의 구현
  * 깊이 우선 탐색을 수행하면서 dfs()가 종료하는 순서를 기록한 뒤 순서를 뒤집음
    * 사이클이 없으면 이 순서는 그래프의 위상 정렬 결과가 됨

```js
// code 28.3 깊이 우선 탐색을 이용한 위상 정렬
vector<int> seen, order;

void dfs(int there) {
    seen[here] = 1;
    for (int there = 0; there < adj.size(); there++) {
        if (adj[here][there] && !seen[there])
            dfs(there);
    }
    order.push_back(here);
}

// adj에 주어진 그래프를 위상 정렬한 결과를 반환
// 그래프가 DAG가 아니라면 빈 벡터를 반환
vector<int> topologicalSort() {
    int m = adj.size();
    seen = vecotr<int>(m, 0);

    order.clear();
    for (int i = 0; i < m; i++)
        if (!seen[i])
            dfs(i);

    reverse(order.begin(), order.end());

    // 만약 그래프가 DAG가 아니라면 정렬 결과에 역방향 간선이 있음
    for (int i=0; i < m; i++) {
        for (int j =i+1; j < m; j++) {
            if (adj[order[j]][order[i]])
                return vector<int>();
        }
    }
    // 없는 경우라면 깊이 우선 탐색에서 얻은 순서를 반환
    return order;
}
```


### 오일러 서킷

* **그래프의 모든 간선을 정확히 한 번씩 지나서 시작점으로 돌아오는 경로를 찾는 문제**
  * 그래프 이론에서 `오일러 서킷(Eulerian Circuit)`이라고 부름
  * 그래프 이론에서 첫 번째로 연구된 문제로 유명
  * == 한 붓 그리기 문제
* **오일러 서킷이 어느 경우에 존재하는지 판단하는 단서는 반대로 존재할 수 없는 경우는 무엇인가 생각해 보는 것**
  * 그래프의 간선들이 두 개 이상의 컴포넌트에 나뉘는 경우 존재할 수 없음
    * 시작점과 끝점이 다른 경우 - 끝점에 인접한 간선의 개수가 홀수
    * **시작점과 끝점이 같은 경우 - 끝점에 인접한 간선의 개수가 짝수**
  * 한 정점에 인접한 간선의 수를 해당 정점의 `차수(degree)`라고 부름
    * 차수가 짝수인 정점을 짝수점
    * 차수가 홀수인 정점을 홀수점
  * 오일러 서킷의 존재 가능성에 직접적인 영향을 미치는 것은 홀수점
    * 오일러 서킷은 모든 정점에 들어가는 횟수와 나오는 횟수가 같아야 함, 홀수점이면 불가능
    * **결국 그래프의 모든 정점이 짝수점이어야만 오일러 서킷이 존재할 수 있음**

### 오일러 서킷을 찾아내는 알고리즘

* 어떤 그래프의 모든 정점이 짝수점이고, 모든 간선이 하나의 컴포넌트에 포함되어 있다고 가정
* 임의의 정점 u에서 시작해 아직 따라가지 않은 간선 중 하나를 따라가며 임의의 경로를 만드는 함수 findRandomCircuit(u)를 만듦
  * 이 함수는 현재 정점에 인접한 간선 중 아직 따라간 적 없는 임의의 간선을 따라가기를 반복하다 더 이상 따라갈 간선이 없을 때 종료함
* findRandomCircuit()이 찾는 경로의 끝점은 그래프의 모든 정점은 짝수점이므로, 시작점 외 다른 정점에서 종료하는 것은 불가능
  * 따라서 항상 시작점에서 끝나게 되고 findRandomCircuit()이 찾아낸 경로는 항상 서킷이 됨
* 서킷이 이미 모든 간선을 지나쳤다면 오일러 서킷을 찾은 셈
  * 만약 아니라면 서킷의 각 정점들을 하나씩 돌아보며 아직 따라가지 않은 간선과 인접해 있는 정점을 찾도록 함
  * 간선들이 모두 하나의 컴포넌트에 포함되어 있기 때문에 아직 따라가지 않은 간선이 남아 있는 정점은 항상 존재할 수 밖에 없음
* 남아있는 정점 v에 인접한 간선 중 따라가지 않고 남아 있는 간선의 수는 몇 개인지?
  * v 또한 짝수점이었을 텐데 우리가 처음 찾은 경로가 v를 지나가면서 짝수 개의 간선을 사용했기 때문에 짝수 개의 간선이 남을 수 밖에 없음
  * 따라서 v에서 시작하도록 findRandomCircuit()을 다시 수행하면 새로운 서킷을 얻게 됨
  * 원하는 것은 모든 간선을 포함하는 하나의 서킷이기 때문에 이 두 서킷을 합쳐야 함
  * 처음 찾았던 서킷을 v에서 자른 뒤, 새로운 서킷을 끼워넣어 하나의 큰 서킷을 만들 수 있음
* **실선 = 새로 얻은 사이클**
  * **실선들을 합치면 전체 서킷이 됨**

![오일러서킷](https://mblogthumb-phinf.pstatic.net/20150517_35/infoefficien_1431828027471tOSG8_PNG/2015-05-17_10%3B59%3B05.PNG?type=w2)

### 깊이 우선 탐색을 이용한 오일러 서킷 탐색 알고리즘 구현

* **두 서킷을 합치기 위해서는 서킷의 길이에 비례하는 시간이 걸리기 때문에 시간도 오래걸리고 구현이 까다로움**
* `깊이 우선 탐색`을 응용한 구현을 이용하면 간단하게 구현 가능
  * findRandomCircuit()을 깊이 우선 탐색처럼 재귀 호출로 구현하는 것
  * findRandomCircuit(u)는 u와 인접한 간선들을 검사하며 아직 방문하지 않은 간선 (u, v)가 있으면 다시 findRandomCircuit(v)를 호출함
  	* 더 이상 따라갈 간선이 없으면 재귀 호출을 종료하고 반환
* **재귀 호출이 종료하는 순간, 지금까지 따라온 간선들을 모으면 하나의 서킷이 됨**
  * **재귀 호출의 반환 과정은 지금까지 지나온 정점들을 방문했던 역순으로 하나씩 지나므로, 이 과정에서 지나지 않은 간선이 있으면 새 서킷을 만들어 가운데 끼워 넣도록 함**
* 아래 구현 코드에는 정점에 따라가지 않은 간선이 남아 있을 때 새 서킷을 만들어서 지금까지 만든 서킷 가운데 끼워넣는 코드가 없음
  * 각 간선을 따라갈 때 경로에 추가하는 것이 아니라, 재귀 호출이 끝나고 반환할 때 추가하기 때문
  * 간선을 따라갈 때마다 getEulerCircuit() 함수를 호출하고 내부에서는 O(V) 반복문을 수행하기 때문에 전체 시간 복잡도는 O(VE)가 됨
  	* 인접 리스트 표현 방법을 사용 시 O(E)가 가능하나 반대쪽에서 오는 간선을 지우는 구현이 까다로움

```c++
// code 28.4 깊이 우선 탐색을 이용한 오일러 서킷 찾기

// 그래프의 인접 행렬 표현 adj[i][j] = i와 j사이의 간선 수
vector<vector<int>> adj;

// 무향 그래프의 인접행렬 adj가 주어질 때 오일러 서킷을 계산
// 결과로 얻어지는 circuit을 뒤집으면 오일러 서킷이 됨
void getEulerCircuit(int here, vector<int>& circuit) {
    for(int there = 0; there < adj[here].size(); ++there) {
        while(adj[here][there] > 0) {
            adj[here][there]--; // 양쪽 간선을 모두 지움 
            adj[there][here]--;
            getEulerCircuit(there, circuit);
        }
    }
    circuit.push_back(here); // 현재 정점에서 만들어진 서킷만 저장
}
// C++ 싫다..
```

![circuit](https://raw.githubusercontent.com/ohbokdong/AlgorithmStudy/main/summary/week14/circuit_1.png))

```js
// // 5개의 노드가 circuit을 이루는 경우
var adj = []; 
        // 0, 1, 2, 3, 4
adj[0] = [ 0, 1, 1, 0, 0];
adj[1] = [ 1, 0, 1, 1, 1];
adj[2] = [ 1, 1, 0, 0, 0];
adj[3] = [ 0, 1, 0, 0, 1];
adj[4] = [ 0, 1, 0, 1, 0];

var circuit = [];
var getEulerCircuit = function(here, circuit) {
    for(var there = 0; there < adj[here].length; ++there) {
        while(adj[here][there] > 0) {
            adj[here][there]--; // 양쪽 간선을 모두 지움 
            adj[there][here]--;
            getEulerCircuit(there, circuit);
        }
    }
    circuit.push(here); // 현재 정점에서 만들어진 서킷만 저장
};

getEulerCircuit(0, circuit);
console.log(circuit); // [0, 2, 1, 4, 3, 1, 0]
circuit.reverse();
console.log(circuit); // [0, 1, 3, 4, 1, 2, 0]
```

### 오일러 트레일

* 오일러 서킷처럼 그래프의 모든 간선을 정확히 한 번씩 지나지만 시작점과 끝점이 다른 경로를 `오일러 트레일(Eulerian Trail)`이라고 함
  * **오일러 트레일을 찾는 문제는 오일러 서킷을 찾는 문제로 변환 가능**
  * 시작점 a, 끝점 b에서 끝나는 오일러 트레일을 찾고 싶다면?
    * 시작점 a, 끝점 b인 오일러 서킷을 찾고, (b, a) 간선을 지워서 서킷을 끊으면 오일러 트레일을 얻을 수 있음
* **시작점과 끝점을 제외한 모든 점이 짝수점이고 시작점과 끝점은 홀수점**

### 문제 : 단어 제한 끝말잇기

* 단어 제한 끝말잇기는 일반적인 끝말잇기와 달리 사용할 수 있는 단어의 종류가 게임 시작 전에 미리 정해져 있으며, 한 단어를 두 번 사용할 수 없음
* **단어 제한 끝말잇기에서 사용할 수 있는 단어들의 목록이 주어질 때, 단어들을 전부 사용하고 게임이 끝날 수 있는지, 그럴 수 있다면 어떤 순서로 단어를 사용해야 하는지를 계산하는 프로그램을 작성하는 문제**
* 시간 및 메모리 제한
  * 실행 시간 1초, 64MB 메모리
* 입력
  * 테스트 케이스 수 C (1<= C <= 50)
  * 각 테스트 케이스의 첫 줄에는 게임에서 사용할 수 있는 단어의 수 n(1<=n<=100)
  * 그 후 한 줄에 하나씩 게임에서 사용할 수 있는 n개의 단어가 주어짐
    * 각 단어는 알파벳 소문자로 구성돼 있으며, 길이는 2 이상 10 이하
    * 한 테스트 케이스에 두 번 이상 출현하는 단어는 없음 
* 출력
  * 각 테스트 케이스마다 한 줄을 출력
    * 모든 단어를 사용하고 게임이 끝나는 방법이 없다면 "IMPOSSIBLE"을 출력(따옴표 제외)
    * 방법이 있다면 사용할 단어들을 빈 칸 하나씩을 사이에 두고 순서대로 출력
      * 방법이 여러개라면 그 중 아무 것이나 출력


### 풀이 : 단어 제한 끝말잇기

* **해밀토니안 경로와 오일러 트레일**
  * 문제를 그래프로 표현하는 가장 직관적인 방법은 입력에 주어진 각 단어를 정점으로 하는 방향 그래프를 만드는 것
  * 한 단어의 마지막 글자가 다른 단어의 첫 글자와 같다면 두 단어를 연속해서 사용할 수 있으므로 간선을 추가
    * p844, 그림 28.7 참고
      * 굵은 경로에 있는 단어들을 순서대로 사용하면 답
  * **그래프의 모든 정점을 정확히 한 번씩 지나는 경로를 `해밀토니안 경로(Hamiltonian Path)`라고 함**
    * 해밀토니안 개념은 직관적이지만 큰 그래프에 대한 해밀토니안 경로를 찾는 방법은 아직 고안되지 않음
    * 해밀토니안 조합을 찾는 유일한 방법은 조합 탐색
      * 모든 정점의 배열을 하나하나 시도하며 이들이 경로가 되는지 확인하는 것
        * 최악의 경우 n!
        * 단어 100개일 때 1초 안에 풀 수 없음
* **이 문제를 시간안에 푸는 방법은 입력에 주어진 각 단어를 정점이 아닌 간선을 갖는 방향 그래프를 만드는 것**
  * **그래프의 정점들은 알파벳의 각 글자를 표현하며, 각 단어는 첫 글자에서 마지막 글자로 가는 간선이 됨**
  * 이 그래프의 오일러 트레일 혹은 오일러 서킷을 찾으면 답

```c++
// code 28.5 끝말잇기 문제의 입력을 그래프로 만들기
// 그래프의 인접 행렬 표현, adj[i][j] == i와 j 사이의 간선의 수
vector<vector<int>> adj;

// graph[i][j] == i로 시작해서 j로 끝나는 단어의 목록
vector<string> graph[26][26];

// indegree[i] == i로 시작하는 단어의 수 
// outdegree[i] == i로 끝나는 단어의 수
vector<int> indegree;
vector<int> outdegree;

void makeGraph(const vector<string>& words) {
    // 전역 변수 초기화
    for (int i=0; i<26; i++)
        for (int j=0; j<26; j++)
            graph[i][j].clear();

    adj = vector<vector<int>>(26, vector<int>(26, 0));
    indegree = vector<int>(26, 0);
    outdegree = vector<int>(26, 0); 

    // 각 단어를 그래프를 추가한다.
    for (int i=0; i<words.size(); i++) {
        int first_alphabet = words[i][0] - 'a';
        int last_alphabet = words[i][words[i].size()-1] - 'a';
        graph[first_alphabet][last_alphabet].push_back(words[i]);
        adj[first_alphabet][last_alphabet]++;
        outdegree[first_alphabet]++;
        indegree[last_alphabet]++;
    }
}
```

* **방향 그래프에서의 오일러 서킷**
  * 이 문제에서 오일러 서킷을 찾을 그래프는 방향 그래프
  * 방향 그래프에서 오일러 서킷을 찾는 알고리즘은 무향 그래프와 거의 다르지 않지만 오일러 서킷의 존재 조건이 무향 그래프와 다름
    * 무향 그래프에서는 각 정점에 인접한 간선이 짝수 개여야 했음
    * **방향 그래프에서 각 간선은 둘 중 한 방향으로만 쓸 수 있기 때문에, 각 정점에 들어오는 간선의 수와 나가는 간선의 수가 같아야만 함**
      * 간선들의 방향을 무시했을 때 방향 그래프의 정점들이 서로 연결돼 있어야 한다는 의미
  * a에서 시작하고 b에서 끝나는 오일러 트레일을 찾기 위해서는 간선 (b,a)를 그래프에 추가한 뒤 오일러 서킷을 찾아야 함
    * 이 때 오일러 서킷의 존재 조건
      * a에서는 나가는 간선이 들어오는 간선보다 하나 많음
      * b는 들어오는 간선이 나가는 간선보다 하나 많음
      * 다른모든 정점에서는 나가는 간선과 들어오는 간선의 수가 같음
* **오일러 서킷 혹은 트레일**
  * 이 문제에서 답은 오일러 서킷일수도 트레일일 수도 있음
  * 오일러 서킷을 찾을지, 트레일을 찾을지 알기 좋은 방법은 각 정점의 차수를 확인하는 것 
    * 오일러 트레일의 시작점에서는 나가는 간선의 수가 들어오는 간선의 수보다 하나 많음
    * 이런 정점이 있다면 오일러 트레일
    * 아니면 오일러 서킷을 찾는 함수를 작성 가능

```c++
// code 28.6 방향 그래프에서 오일러 서킷 혹은 트레일을 찾아내기

// 유향 그래프의 인접 행렬 adj가 주어질 때 오일러 서킷 혹은 트레일을 계산
void getEulerCircuit(int here, vector<int>& circuit) {
    for (int there=0; there<adj.size(); there++) {
        while(adj[here][there] > 0) {
            adj[here][there]--; // 간선을 지움, 방향 그래프기 때문에 양쪽 지우지 않음
            getEulerCircuit(there, circuit);
        }
    }
    circuit.push_back(here);
}

// 현재 그래프의 오일러 트에리이나 서킷을 반환
vector<int> getEulerTrailOrCircuit() {
    vector<int> circuit;

    // 우선 트레일을 찾아봄, 시작점이 존재하는 경우
    for (int i=0; i<26; i++) {
        if (outdegree[i] == indegree[i]+1) {
            getEulerCircuit(i, circuit);
            return circuit;
        }
    }

    // 아니면 서킷이나, 간선에 인접한 아무 접점에서나 시작함
    for (int i=0; i<26; i++) {
        if (outdegree[i]) {
            getEulerCircuit(i, circuit);
            return circuit;
        }
    }

    // 모두 실패한 경우 빈 배열을 반환
    return circuit;
}
```


* **오일러 서킷/ 트레일의 존재 여부 확인**
  * 위처럼 그래프 생성과 오일러 서킷 혹은 트레일을 찾는 코드를 작성하고 나면 나머지는 단순(?)
    * 오일러 트레일이 존재하는지 확인하고, 존재한다면 출력 문자열을 계산해 반환하기만 하면 됨

```c++
// code 28.7 끝말잇기 문제를 오일러 트레일 문제로 바꿔 해결하는 알고리즘

// 현재 그래프의 오일러 서킷/트레일의 존재 여부를 확인한다
bool checkEuler() {
    // 예비 시작점과 끝점의 수
    int plus1 = 0;
    int minus1 = 0;

    for (int i=0; i<26; i++) {
        int delta = outdegree[i] - indegree[i];

        // 모든 정점의 차수는 -1, 1 또는 0 이어야 함
        if (delta < -1 || 1 < delta) return false;
        if (delta == 1) plus1++;
        if (delta == -1) minus1++;
    }

    // 방향 그래프에서 오일러 서킷이 있으려면 모든 정점에서 나가는 간선의 수와 들어오는 간선의 수가 같아야 함
    // 방향 그래프에서 오일러 트레일이 있으려면 나가는 간선이 하나 많은 시작점이 하나, 들어노느 간선이 하나 많은 끝점이 하나 있어야 함
    
    // 시작점과 끝점은 각 하나씩 있거나 하나도 없어야 함
    return (plus1 == 1 && minus1 == 1) || (plus1 == 0 && minus1 == 0);
}

string solve(const vector<string>& words) {
    makeGraph(words);

    // 차수가 맞지 않으면 실패
    if (!checkEuler()) return "IMPOSSIBLE";
    
    // 오일러 서킷이나 경로를 찾아냄
    vector<int> circuit = getEulerTrailOrCircuit();

    // 모든 간선을 방문하지 못했으면 실패 (그래프가 두 개 이상으로 분리돼 있는 경우)
    if (circuit.size() != words.size() + 1) return "IMPOSSIBLE";

    // 아닌 경우 방문 순서를 뒤집은 뒤 간선들을 모아 문자열로 만들어 반환
    reverse(circuit.begin(), circuit.end());
    string ret;

    for (int i=1; i<circuit.size(); i++) {
        int a = circuit[i-1];
        int b = circuit[i];

        ret += graph[a][b].back();
        graph[a][b].pop_back();
    }

    return ret;
}
```

* 이 코드의 시간 복잡도는 오일러 트레일을 찾는 함수에 의해 지배됨
  * 오일러 트레일을 찾는 함수의 수행 시간은 알파벳의 수 A와 단어의 수 n의 곱에 비례하여 **O(nA)**

