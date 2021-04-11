# 22. 이진 검색 트리
## 22.1. 도입
- 트리는 현실 세계의 계층적 구조를 표현하는 것 외에도 다양한 용도로 사용되며 그중 대표적인 것이 `검색 트리(search tree)`
- 검색 트리는 `연결 리스트`나 `큐`처럼 자료들을 담는 컨테이너지만 자료들을 **일정한 순서에 따라 정렬한 상태로 저장**해 둠
    - ex) 32비트 정수들을 작은 것부터 큰 것까지 정렬한 상태로 저장할 수도 있고, 문자열을 가나다순으로 정렬해서 저장할 수도 있음
- 정렬한 상태로 저장한다는 특성을 이용해 원소의 `추가`와 `삭제`만이 아니라 특정 원소의 `존재 여부 확인` 등의 **다양한 연산을 빠르게 수행**함
   

## 22.2 이진 검색 트리의 정의와 조작
   
### 이진 트리의 정의
- 트리의 각 노드가 왼쪽과 오른쪽, 최대 두 개의 자식노드만을 가질 수 있는 트리
    - 각 노드의 차수가 2 이하인 순서 트리
- 따라서 이진 트리는 자식 노드들의 배열 대신 두 개의 포인터 `left`와 `right`를 담는 객체로 구현\
- 이진 검색 트리의 각 노드의 왼쪽 서브트리는 현재 노드가 가진 원소보다 작고 오른쪽 서브트리는 현재 노드가 가진 원소보다 큼
- `이진 탐색(binary search)`에서 아이디어를 가져와 만든 트리이므로 자료의 검색에 `O(lgN)`시간에 그 값을 찾을 수 있음

### 순회
- 이전 검색 트리를 `중위 순회`하면 크기 순서로 정렬된 원소의 목록을 얻을 수 있음

### 자료의 검색
- 왼쪽 서브트리는 더 작은 원소가 있고 오른쪽 서브트리는 더 큰 원소가 있으므로 실질적으로는 이진 탐색과 비슷한 속도로 자료를 찾을 수 있게 됨

### 조작
- 이진 검색 트리가 진가를 드러내는 곳은 **집합에 원소를 추가하거나 삭제하는 조작 연산**을 해야 할 때
    - 선형적인 구조의 제약이 없기 때문에, 새 원소가 들어갈 위치를 찾고 거기에 노드를 추가하기만 하면 간단히 새 원소를 추가할 수 있음
- 이진 검색 트리에서 가장 까다로운 연산은 집합에서 원소를 삭제하는 것
    - ex) 트리의 루트를 지우고 싶을 때
    - 이진 검색 트리에서 삭제를 구현하는 방법중 간편한 방법으로 `합치기`가 있음
   

## 22.3 시간 복잡도 분석과 균형 잡힌 이진 검색 트리
   
- 1부터 N까지의 숫자들을 순서대로 추가할 경우 루트에 1, 그 오른쪽에 2, 그 오른쪽에 3이 추가되어 결과적으로 트리의 높이는 `N-1`
- 이런 트리를 한쪽으로 `'기울어졌다'(skewed)`고 함
- 이러한 문제점을 개선한 이진 검색 트리의 변종들을 `균형 잡힌 이진 검색 트리(balanced binary search tree)`라고 부름
    - 트리의 구조에 추가적인 제약을 정하고 이 제약이 만족되도록 노드들을 옮겨서 트리의 높이가 항상 `O(lgN)`이 되도록 유지
    - 대표적인 예로 `레드-블랙 트리(red-black tree)`가 있음

## 22.4 ~ 22.5 문제: 너드인가, 너드가 아닌가? 2
- 알고스팟 온라인 채점 시스템에서 푼 문제의 수 p
- 밤 새면서 지금까지 끓여먹은 라면 그릇 수 q
- 참가 신청자 a의 문제 수 p<sub>a</sub>와 그릇 수 q<sub>a</sub>를 다른 참가 신청자 b의 문제 수 p<sub>b</sub>와 그릇 수 q<sub>b</sub>에 각각 비교 했을 때
p<sub>b</sub> \< p<sub>a</sub>, q<sub>b</sub> \< p<sub>a</sub> 라면 참가 신청자 a의 참가 자격을 박탈시키는... 문제
```C++
// 코드 22.1 ~ 22.2
// 22.1 한 점이 다른 점에 지배당하는지 확인하는 함수
// 현재 다른 점에 지배당하지 않는 점들의 목록을 저장
// coords[x] = y
map<int, int> coords;
// 새로운 점 (x, y)가 기존의 다른 점들에 지배당하는지 확인
bool isDominated(int x, int y) {
    // x보다 오른쪽에 있는 점 중 가장 왼쪽에 있는 점을 찾는다.
    map<int, int>::iterator it = coords.lower_bound(x);
    // 그런 점이 없으면 (x, y)는 지배당하지 않음
    if(it == coords.end()) return false;
    // 이 점은 x보다 오른쪽에 있는 점중 가장 위에 있는 점이므로,
    // (x, y)가 어느 점에 지배되려면 이 점도 지배되어야 한다.
    return y < it -> second;
}

// 22.2 지배되는 점들을 삭제하는 함수
// 새로운 점 (x, y)에 지배당하는 점들을 트리에서 지움
void removeDominated(int x, int y) {
    map<int, int>::iterator it = coords.lower_bound(x);
    // (x, y)보다 왼쪽에 있는 점이 없다면
    if(it == coords.begin()) return;
    --it;
    // 반복문 불변식: it는 (x, y)의 바로 왼쪽에 있는 점
    while(true) {
        // (x, y) 바로 왼쪽에 오는 점을 찾음
        // it가 표시하는 점이 (x, y)에 지배되지 않는다면 곧장 종료
        if(it -> second > y) break;
        // 이전 점이 더 없으므로 it만 지우고 종료한다.
        if (it == coords.begin()) {
            coords.erase(it);
            break;
        } 
        // 이전 점으로 이터레이터를 하나 옮겨 놓고 it를 지운다.
        else {
            map<int, int>::iterator jt = it;
            --jt;
            coords.erase(it);
            it = jt;
        }
    }

}

// 새 점 (x, y)가 추가되었을 때 coords를 갱신하고
// 다른 점에 지배당하지 않는 점들의 개수를 반환한다.
int registered(int x, int y) {
    // (x, y)가 이미 지배당하는 경우에는 그냥 (x, y)를 버린다.
    if(ifDominated(x, y)) return coords.size();
    // 기존에 있던 점중 (x, y)에 지배당하는 점들을 지운다.
    removeDominated(x, y);
    coords[x] = y;
    return coords.size();
}
```
### 시간 복잡도
- `isDominated()` : 이진 검색 트리에서의 탐색이 지배하기 때문에 `O(lgN)`
- `removeDominated()` : 한 점은 최대 한 번만 지워질 수 있기 때문에 `N-1`
- 따라서 전체 시간 복잡도는 `O(NlgN)`

## 22.6 균형 잡힌 이진 검색 트리 직접 구현하기: 트립
### 트립의 정의
- 입력이 특정 순서로 주어질 때 그 성능이 떨어진다는 이진 검색 트리의 단점을 해결하기 위해 고안된 일종의 랜덤화된 이진 검색 트리
- 트리의 형태가 원소들의 추가 순서에 따라 결정되지 않고 난수에 의해 임의대로 결정됨

### 트립의 구현
```C++
// 코드 22.3 트립의 노드를 표현하는 객체의 구현

typedef int KeyType;
// 트립의 한 노드를 저장
struct Node {
    // 노드에 저장된 원소
    KeyType key;
    // 이 노드의 우선순위(priority)
    // 이 노드를 루트로 하는 서브트리의 크기 (size)
    int priority, size;
    // 두 자식 노드의 포인터
    Node *left, *right;
    // 생성자에서 난수 우선순위를 생성하고, size와 left/right를 초기화한다.
    Node(const keyType& _key) : key(_key), priority(rand()), size(1), left(NULL), right(NULL) {
    }
    void setLeft(Node* newLeft) { left = newLeft; calcSize(); }
    void setRight(Node* newRight) { right = newRight; calcSize(); }
    // size 멤버를 갱신한다.
    void calcSize() {
        size = 1;
        if(left) size += left -> size;
        if(right) size += right -> size;
    }
}
```