# 27 그래프의 표현과 정의

## 27.1 도입

그래프는 현실 세계의 사물이나 추상적인 개념 간의 연결 관계를 표현하고, 부모 자식 관계에 관한 제약이 없기 때문에 트리보다 다양한 구조가 가능하다.

### 그래프의 정의

그래프 G(V, E)는 어떤 자료나 개념을 표현하는 정점(vertex)들의 집합 V와 이들을 연결하는 간선(edge)들의 집합 E로 구성된 자료 구조이다.  
정점의 위치 정보나 간선의 순서는 정의에 포함되지 않는다.

### 그래프의 종류

![그래프의 종류](https://doorisopen.github.io/developers-library/images/Datastructure/datastructure-graph-category.JPG)

- 방향그래프(directed graph): 각 간선이 방향이라는 속성을 가짐
  - 예) 짝사랑, 일방 통행 방향
- 무향그래프(undirected graph): 방향이 없음
- 가중치 그래프(weighted graph): 각 간선에 가중치(weight)가 있음
  - 예) 두 도시 사이 거리, 사람 사이 호감도 등
- 다중 그래프(multigraph): 두 정점 사이에 두 개 이상의 간선이 있을 수 있는 그래프
  - 예) 도로망
- 단순그래프(simple graph): 최대 한 개의 간선만 있는 그래프
- 트리 혹은 루트 없는 트리(unrooted tree): 부모 자식 관계가 없는 무향 그래프
- 이분 그래프(bipartite graph): 정점들을 겹치지 않는 두 개의 그룹으로 나눠서 서로 다른 그룹에 속한 정점들 간에만 간선이 존재하는 것
  - 예) 남성-여성
- 사이클 없는 방향 그래프, DAG(directed acyclic graph): 방향그래프, 한 점에서 출발해 자기 자신으로 돌아오는 경로(사이클)가 존재

### 그래프의 경로(path)

경로: 끝과 끝이 서로 연결된 간선들을 순서대로 나열한 것

- 단순 경로(simple path): 한 정점을 최대 한 번만 지나는 경로
- 사이클 / 회로: 시작한 점에서 끝나는 경론

## 27.2 그래프의 사용 예

### 철도망의 안정성 분석 => 절단점 찾기 알고리즘

한 역이 폐쇄되어 열차가 지나지 못할 경우 철도망 전체가 두 개 이상으로 쪼개질 가능성이 있는지, 만약 있다면 어느 역이 그 위험성을 갖고 있는지 분석 시.  
각 역- 정점  
두 역 사이 연결 천로- 간선

### 소셜 네트워크 분석 => 너비 우선 탐색

친밀도 관계, 한 다리 건너면 알 수 있는 사람, 몇 번 건너면 알 수 있는 사이인지 등

### 인터넷 전송 속도 계산 => 최소 스패닝 트리 알고리즘

인터넷에 연결된 라우터와 컴퓨터 - 정점
두 기계 연결 케이블 - 간선

### 한 붓 그리기 => 오일러 경로(Eulerian path), 깊이 우선 탐색

### 외환 거래 => 최단 거리 알고리즘

1달러 -> 유로 -> 엔 -> 달러 교환 시 1달러보다 많은 돈을 가지고 있게 되는 것: 아비트러지(arbitrage). 아비트러지 존재 여부

## 27.3 암시적 그래프 구조들(implicit graph)

그래프 형태는 아니지만, 그래프를 통해 표현하면 쉽게 해결할 수 있는 문제

### 할 일 목록 정리 => 깊이 우선 탐색

어느 순서대로 해야 되는지 계산하는 문제. 위상 정렬(topological sorting).

### 15-퍼즐 => 최단 경로 문제

![15퍼즐](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQkrhDQE7Vi-bIefyCf7rkSu5lWi6tp0dhWvQ&usqp=CAU)

타일이 끼워진 퍼즐을 움직여 원래 있던 순서대로 정렬하기. 현재 타일의 배치를 하나의 정점. 타일 한 번 움직여 다른 배치를 얻을 때 두 정점 사이는 간선.

### 게임판 덮기 => 이분 매칭 알고리즘

### 회의실 배정 => 강 결합성 문제

N개의 팀이 하나의 회의실을 두고 적어 낸 두 개의 시간 중 하나씩 배정해서 모든 팀이 회의할 수 있도록 하기. 만족성 문제(satisfiability problem). 두 선택지 중 하나 선택하는 문제를 2-SAT이라고 하기도 함

## 27.4 그래프의 표현 방법

그래프는 트리에 비해 추가하고 삭제하는 일이 자주 일어나지 않는 경우에 많이 사용된다. 대부분 구조의 변경이 어렵더라도 간단하고 메모리를 적게 차지하는 방법을 구현한다.  
그래프는 간선의 정보를 어떤 식으로 저장하느냐에 따라 두 가지로 나뉜다.

### 인접 리스트(adjacency list) 표현

각 정점마다 해당 정점에서 나가는 간선의 목록을 저장해서 그래프를 표현

```C++
vector<list<int> > adjacent;
```

adjacent[i]는 정점 i와 간선을 통해 연결된 정점들의 번호를 저장하는 연결 리스트  
가중치가 있다면 다음과 같은 형태

```C++
struct Edge {
  int vertex; // 간선의 반대쪽 끝 점의 번호
  int weight; // 간선의 가중치
}
```

### 인접 행렬(adjacency matrix) 표현

인접 리스트의 단점: 두 정점이 주어졌을 때 연결되어 있는 지 확이하려면 일일이 다 뒤져야 한다는 것  
인접 행렬은 |V| X |V| 크기의 행렬, 즉 2차원 배열을 이용하며, 가장 간단한 형태의 인접 행렬 표현은 2차원 불린 값 배열이 된다.

```C++
vector<vector<bool> > adjacent;
```

adjacent[i, j]는 정점 i에서 j로 가는 간선이 있는 지 나타내는 불린 값 변수로 존재한다면 어떤 정수나 실수, 없다면 -1 혹은 아주 큰 값 등 존재할 수 없는 값으로 지정

### 인접 행렬 표현과 인접 리스트 표현의 비교

![비교](https://lh3.googleusercontent.com/proxy/6N5HfzV5UeZNxsW5XTAfkTInY8Bd9Zk_XCoJp2Ia5cIja_D-hOa9cv3HqWawboHrXjR7cLWAlbrnshumJe7hDZ6349YW8Yw0D_Pcyd0SURDxiZFejBI6RlFmVyyKebyfsTm7JtOZqfEnTUc3TUd6rJB1Gw26uIQv7sZAjsSECYuacWfKEMy816yk8d2oIV46XLz1Gu7nzEIQfRbjWNKps4f3qQIzLeelwQU1fk0neXmsY8wpiFvPPMX41AIdY2W0vpIddJroPejbax67b8L4qtn6YMI3PXPCb3EN)

> 인접 행렬 표현

**장점**  
두 정점을 있는 간선이 존재하는 지 여부를 한 번의 배열 접근으로 확인 가능  
**단점**  
실제 간선 개수와 상관없이 O(|V|<sup>2</sup>) 크기의 공간을 사용

간선의 수가 |V|<sup>2</sup>에 비해 훨씬 적은 그래프를 `희소 그래프(sparse group - 인접 리스트가 유리)`이라고 하고 비례하는 경우를 `밀집 그래프(dense graph - 인접 행렬이 유리)`라고 한다.

### 암시적 그래프 표현

`미로에서 최단 경로를 찾는 문제`의 경우 서로 인접한 칸들을 서로 연결한 그래프를 만들어 `너비 우선 탐색`을 사용하면 문제를 쉽게 풀 수 있다.  
빈 칸의 위치를 (y, x)로 표현하여 현재 칸 중심으로 상하좌우를 검색하여 비어있는지 확인하면 된다.  
15-퍼즐도 좋은 예.  
암시적 그래프 표현은 그래프 사용하는 알고리즘과 변환 과정이 합쳐져 좀 더 코드가 복잡해질 가능성도 있다.  
**복잡한 연산이나 알고리즘을 수행할 거라면 번거롭더라도 입력을 미리 그래프 표현으로 바꿔두는 것이 전체 코드를 간결하게 할 수 있다**