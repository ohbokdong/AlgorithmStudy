# 8장 동적 계획법(dynamic programming)
[유튜브에서 찾은 강의](https://www.youtube.com/watch?v=-2Rta2Lnzmo)
## 8.1 도입
최적화 문제를 연구하는 수학 이론에서 유래한 이름

### 중복되는 부분 문제
큰 의미에서 분할 정복과 같은 접근 방식을 의미하며, 처음 주어진 문제를 더 작은 문제들로 나눈 뒤 각 조각의 답을 계산하고, 이 답들로부터 원래 문제에 대한 답을 계산해 내는 방식이다.
- 동적 계획법 vs 분할 정복: `문제를 나누는 방식`이 다름.
  - 동적 계획법: 어떤 문제는 두 개 이상의 문제를 푸는 데 사용될 수 있기 때문에, 이 문제의 답을 여러 번 계산하는 대신 한 번만 계산하고 계산 결과를 재활용하여 속도를 향상함. 그러기 위해 각 문제의 답을 저장해 두는 메모리의 장소를 캐시(cache)라고 부르며, 두번 이상 계산되는 부분 문제를 중복되는 부분 문제(overlapping subproblems)라고 부름.
- 동적 계획법 알고리즘의 가장 유명한 예: 이항 계수(binomial co-efficient), n개의 서로 다른 원소 중에서 r개의 원소를 순서없이 골라내는 방법의 수

```C++
// 재귀 호출을 이용한 이항 계수의 계산
int bino(int n, int r) {
  //기저사례: n=r(모든 원소를 다 고르는 경우) 혹은 r=0 (고를 원소가 없는 경우)
  if(r == 0 || n == r) return 1;
  return bino(n-1, r-1) + bino(n-1, r);
}
```

- 한 번 계산한 값을 저장해 뒀다 활용하는 최적화 기법: 메모이제이션(memoization)
  - 위의 bino보다 bino2는 훨씬 적게 반복함
```C++
// 메모이제이션을 이용한 이항 계수의 계산

// -1로 초기화해 둔다
int cache[30][30];
int bino2(int n, int r) {
  // 기저 사례
  if(r == 0 || n == r) return 1;
  // -1이 아니라면 한 번 계산했던 값이니 곧장 반환
  if(cache[n][r] != -1)
    return cache[n][r];
  // 직접 계산한 뒤 배열에 저장
  return cache[n][r] = bino2(n-1, r-1) + bino2(n-1, r);
}
```

### 메모이제이션을 적용할 수 있는 경우
- 참조적 투명성(referential transparency): 함수의 반환 값이 그 입력 값만으로 결정되는 지 여부
- 참조적 투명함수(referential transparent function): 입력이 고정되어 있을 때 그 결과가 항상 같은 함수들   
메모이제이션은 `참조적 투명 함수`의 경우에만 `적용할 수 있습니다`.(캐싱)

### 메모이제이션 구현 패턴
```C++
// a와 b는 각각 [0, 2500) 구간 안의 정수
// 반환 값은 항상 int형 안에 들어가는 음이 아닌 정수
int someObscureFunction(int a, int b);
```
`someObscureFunction()`을 계산하는데 시간이 오래 걸리는 참조적 투명 함수라고 가정
```C++
// 메모제이션의 사용 예

// 전부 -1로 초기화해둔다
int cache[2500][2500];
// a와 b는 각각 [0, 2500) 구간 안의 정수
// 반환 값은 항상 int형 안에 들어가는 음이 아닌 정수
int someObscureFunction(int a, int b) {
  // 기저 사례를 처음에 처리한다
  if(...) return ...;
  // (a, b)에 대한 답을 구한 적이 있으면 곧장 반환
  int& ret = cache[a][b];
  if(ret != -1) return ret;
  // 여기에서 답을 계산한다.
  ...
  return ret;
}
int main() {
  //memset()을 이용해 cache 배열을 초기화한다.
  memset(cache, -1, sizeof(cache));
}
```
1. 기저사례를 항상 제일 먼저 처리: 입력이 범위를 벗어난 경우 등을 기저사례로 처리하면 매우 유용함
2. 함수의 반환 값이 항상 0 이상이라는 점을 이용해 cache[]를 모두 -1로 초기화
3. ret가 cache[a][b]에 대한 참조형(reference)!. ret값을 바꾸면 cache[a][b]도 바뀌기 때문에 답을 저장할때도 귀찮게 cache[a][b]라고 안써도 됨. 다차원 배열일때 유용한 방법
4. memset()을 이용해 초기화하는 부분. 다중 for문보다 간편함

### 메모이제이션의 시간 복잡도 분석
`(존재하는 부분 문제의 수)X(한 부분 문제를 풀 때 필요한 반복문의 수행 횟수)`   
r의 최대치는 n이니 bino2(n, r)을 계산할 때 문제의 수 최대값은 O(n<sup>2</sup>)   
각 부분 문제를 계산할 때 걸리는 시간은 반복문이 없으니 O(1)임으로
O(n<sup>2</sup>)O(1) = O(n<sup>2</sup>)   
하지만 수행 시간의 상한 값일 뿐, 실제 수행 시간은 이 식보다 훨씬 작을 수 있다.

### 예제: 외발 뛰기 (문제 ID: JUMPGAME, 난이도 하)
> #### 재귀 호출에서 시작하기
jump(y,x) = (y,x)에서부터 맨 마지막 칸까지 도달할 수 있는지 여부를 반환한다.   
게임판의 (y,x) 위치에 있는 수를 jumpSize라고 하면,   
아래쪽으로 뛸 경우: jump(y+jumpSize, x)   
오른쪽으로 뛸 경우: jump(y, x+jumpSize)로 표현
`jump(y,x) = jump(y+jumpSize, x) || jump(y,x+jumpSize)`

```C++
// 외발 뛰기 문제를 해결하는 재귀 호출 알고리즘
int n, board[100][100];
bool jump(int y, int x) {
  // 기저 사례: 게임판 밖을 벗어나나 경우
  if(y >= n || x >= n) return false;
  // 기저 사례: 마지막 칸에 도착한 경우
  if (y == n-1 && x == n -1) return true;
  int jumpSize = board[y][x];
  return jump(y + jumpSize, x) || jump(y, x + jumpSize);
}
```

> #### 메모이제이션 적용하기
완전 탐색이 만드는 경로의 수는 엄청나게 많지만, jump()에 주어지는 입력의 개수는 100*100 = 10,000개 뿐이라는 것.   
비둘기집의 원리(비둘기집 원리는 n+1개의 물건을 n개의 상자에 넣을 때 적어도 어느 한 상자에는 두 개 이상의 물건이 들어 있다는 원리를 말한다.)에 의해 중복으로 해결되는 부분 문제들이 항상 존재함을 알 수 있음.   

```C++
// 외발 뛰기 문제를 해결하는 동적 계획법 알고리즘
int n, board[100][100];
int cache[100][100];
int jump2(int y, int x) {
  // 기저 사례 처리
  if (y >= n || x >= n) return 0;
  if (y == n-1 && x == n-1) return 1;
  // 메모이제이션
  int& ret = cache[y][x];
  if (ret != -1) return ret;
  int jumpSize = board[y][x];
  return ret = (jump2(y + jumpSize, x) || jump2(y, x + jumpSize));
}
```
- 위의 코드에서 boolean 값을 반환했으나, jump2()는 정수를 반환함
- jump2()가 반환하는 값은 참/거짓 둘 중의 하나 이나, boolean값으로는 true 아니면 false만 반환할 수 있고 계산되지 않은 상태인지 아닌지 알 수 있는 방법이 없음
- 1 또는 0의 정수를 반환하기로 약속하면 -1로 초기화한 정수형 배열을 캐시로 사용할 수 있음.
- 이 문제는 그래프로 모델링해보면 아주 간단한 도달 가능성 문제가 됨(7부)

### 동적 계획법 레시피
동적 계획법 알고리즘의 구현
1. 주어진 문제를 완전 탐색을 이용해 해결
2. 중복된 문제를 한 번만 계산하도록 메모이제이션을 적용

### 다른 구현 방법
재귀 호출을 이용하지 않고 동적 계획법 알고리즘을 구현 가능 = 반복적 동적 계획법(9.21절 참고)

(8.2 와일드카드-난이도 중, 8.3 풀이:와일드카드 패스)

## 8.4 전통적 최적화 문제들
동적 계획법의 가장 일반적인 사용처는 최적화 문제(여러 개의 가능한 답 중 가장 좋은 답을 찾아내는 문제)의 해결.  
메모이제이션을 적용하여 완전 탐색보다 더 효율적으로 문제를 풀 수 있는 경우가 존재함.   

### 예제: 삼각형 위의 최대 경로 (문제 ID: TRIANGLEPATH, 난이도 하)
![예제](https://blog.kakaocdn.net/dn/bVdS4g/btqGDQW4kxA/litNzt84rox0IDiTQtmvu0/img.jpg)
> #### 완전 탐색으로 시작하기
경로를 가로줄로 조각 낸 뒤, 각 조각에서 아래로 내려갈 지 오른쪽으로 내려갈지를 선택하면서 모든 경로를 만듬.   
재귀함수를 이용해 현재 위치와 지금까지 만난 숫자들의 합을 구함

```
pathSum(y, x, sum) = 현재 위치가 (y, x)이고, 지금까지 만난 수의 합이 sum일 때, 이 경로를 맨 아래줄까지 연장해서 얻을 수 있는 최대 합을 반환
```

> #### 무식하게 메모이제이션 적용하기
이 문제에서 가능한 경로의 개수는 삼각형의 가로줄이 하나 늘어날 때마다 두 배씩 늘어나기 때문에 가능한 경로의 수는 2<sup>n-1</sup>이 된다.   
n<=20인 정도는 가능하지만 늘어나면 계산하기 힘듬.

```C++
// 코드 8.8 삼각형 위의 최대 경로 문제를 푸는 메모이제이션 코드

// MAX_NUMBER: 한 칸에 들어갈 숫자의 최대치
int n, triangle[100][100];
int cache[100][100][MAX_NUMBER*100 + 1];
// (y,x)위치까지 내려오기 전에 만난 숫자들의 합이 sum일 때
// 맨 아래줄까지 내려가면서 얻을 수 있는 최대 경로를 반환
int path1(int y, int x, int sum) {
  // 기저 사례: 맨 아래 줄까지 도달했을 경우
  if(y == n-1) return sum + triangle[y][x];
  // 메모이제이션
  int& ret = cache[y][x][sum];
  if(ret != -1) return ret;
  sum += triangle[y][x];
  return ret = max(path1(y+1, x+1, sum), path1(y+1, x, sum));
}
```
- 226쪽 그림 8.6의 (b)와 같이 2<sup>i</sup>꼴의 숫자로만 구성된 삼각형이 있닥도 하면 서로 다른 경로는 합도 항상 다르기 때문에 완전 탐색과 다를 바가 없음

> #### 입력 걸러내기
재귀함수의 입력을 다음과 같이 두 부류로 나눈다
1. y와 x는 재귀 호출이 풀어야 할 부분 문제를 지정 => 앞으로 풀어야 할 조각들에 대한 정보
2. 반면 sum은 지금까지 어떤 경로로 이 부분 문제에 도달했는지를 나타냄 => 지금까지 풀었던 조각들에 대한 정보

sum을 아예 입력으로 받지 않으면 알고리즘이 훨씬 빨라지지만, 재귀 함수에서 sum을 입력 받지 않으면, 이전까지 어떤 숫자를 만났는지 알 수 없어서 최대 합을 반환할 수 없음.   
따라서 함수의 반환 값을 전체 경로의 최대치가 아니라 (y, x)에서 시작하는 부분 경로의 최대치로 바꿀 필요가 있다
```
path2(y, x) = (y, x)에서 시작해서 맨 아래줄까지 내려가는 부분 경로의 최대 합을 반환
```
다시 말해 path2()는 앞으로 남은 조각들의 결과만을 반환한다   

아래 코드는 부분 문제의 수가 O(n<sup>2</sup>)이고 각 부분 문제를 계산하는 데는 상수 시간밖에 안 걸리기 때문에 전체 시간복잡도는 O(n<sup>2</sup>)
```C++
// 코드8.9 삼각형 위의 최대 경로 문제를 푸는 동적 계획법 알고리즘(2)

int n, triangle[100][100];
int cache2[100][100];
// (y, x)위치부터 맨 아래줄까지 내려가면서 얻을 수 있는 최대 경로의 합을 반환
int path2(int y, int x){
  // 기저 사례
  if(y == n-1) return triangle[y][x];
  // 메모이제이션
  int& ret = cache2[y][x];
  if(ret != -1) return ret;
  return ret = max(path2(y+1, x), path2(y+1, x+1)) + triangle[y][x];
}
```

> #### 이론적 배경: 최적 부분 구조(optimal substructure)
코드 8.8에서 sum이라는 정보가 (y, x)에서 맨 아래줄까지 내려가는 문제를 해결하는 데 아무 상관이 없다는 사실을 파악했던 덕분.   
다시 말해, 남은 부분 문제는 항상 최적으로 풀어도 상관 없다는 뜻.
최적 부분 구조는 어떤 문제와 분할 방식에 성립하는 조건.   
각 부분 문제의 최적해만 있으면 전체 문제의 최적해를 쉽게 얻어 낼 수 있는 경우 이 조건이 성립.


### 예제: 최대 증가 부분 수열 (문제 ID:LIS, 난이도:하)
**LIS: Longest Increasing Subsequence)**   
최대 수열 S의 부분 수열이란 S에서 0개 이상의 숫자를 지우고 남은 수열을 말한다.
예를 들어 '1 2 4'는 '1 5 2 4 7'의 부분 수열. 부분 수열에 포함된 숫자들이 순 증가(strictly increasing)하면 증가 부분 수열이라고 함.

> #### 완전 탐색에서 시작하기
수열 A를 입력 받아 LIS의 길이를 반환하는 재귀 함수 lis(A)는, A의 모든 증가 부분 수열을 만든 뒤 그 중 가장 긴 것의 길이를 반환.   
lis(A)는 수열의 첫 번째 수를 고르고 나머지 부분을 재귀적으로 만들 것이다.   
```C++
// 코드 8.10 최대 증가 부분 수열 문제를 해결하는 완전 탐색 알고리즘
int lis(const vector<int>& A) {
  // 기저 사례: A가 텅 비어 있을 때
  if(A.empty()) return 0;
  int ret = 0;
  for(int i=0; i<A.size(); ++i) {
    vector<int> B;
    for(int j = i+1; j<A.size(); ++j)
      if(A[i] < A[j])
        B.push_back(A[j]);
    ret = max(ret, 1 + lis(B));
  }
}
```

> #### 입력 손보기
입력이 정수가 아니라 정수의 배열이라 메모이제이션이 까다로움.   
A는 항상 다음 두 가지 중 하나
1. 원래 주어진 수열 S
2. 원래 주어진 수열의 원소 S[i]에 대해, S[i+1...]부분 수열에서 S[i]보다 큰 수들만을 포함하는 부분 수열

2번 정의에서 이전에 선택한 수들이 정의에 포함되지 않음에 유의. 2번 정의에 포함되는 A는 S의 인덱스와 1:1대응하게 됨 = A로 주어질 수 있는 입력은 전부 N+1가지 밖에 없다   

```
lis(start) = S[start]에서 시작하는 부분 증가 수열 중 최대의 길이
```

재귀 호출시마다 S[start]보다 뒤에 있고 큰 수들 중 하나를 다음 숫자로 결정한 뒤, 여기서 시작하는 부분 증가 수열의 최대치를 구하게 됨.
```C++
// 코드 8.11 최대 증가 부분 수열 문제를 해결하는 동적 계획법 알고리즘(1)
int n;
int cache[100], S[100];
// S[start]에서 시작하는 증가 부분 수열 중 최대 길이를 반환
int lis2(int start) {
  int& ret = cache[start];
  if(ret != -1) return ret;
  // 항상 S[start]는 있기 때문에 길이는 최하 1
  ret = 1;
  for (int next = start+1; next < n; ++next)
    if(S[start] < S[next]>)
      ret = max(ret, lis2(next) + 1);
  return ret;
}

```
- 별도의 기저 사례 없이, 뒤에 더 이을 숫자가 없는 경우에도 for문을 순회
- O(n)개의 부분 문제를 갖고, 하나를 해결할 때마다 O(n) 시간의 반복문을 순회하므로 전체 O(n<sup>2</sup>) 시간 복잡도

> #### 시작 위치 고정하기
lis2()를 호출 시, 항상 증가 부분 수열의 시작 위치를 지정해 줘야 함
```C++
int maxLen = 0;
for(int begin = 0; begin < n; ++begin)
  maxLen = max(maxLen, lis2(begin));
```

s[-1] = -무한 이라면 모든 시작 위치를 자동으로 시도하게 됨
```C++
// 모드 8.12 최대 증가 부분 수열 문제를 해결하는 동적 계획법 알고리즘 (2)
int n;
int cache[101], S[100];
// S[start]에서 시작하는 증가 부분 수열 중 최대 길이를 반환
int lis3(int start) {
  int& ret = cache[start+1];
  if(ret != -1) return ret;
  // 항상 S[start]는 있기 때문에 길이는 최하 1
  ret = 1;
  for(int next = start+1; next < n; ++next)
    if(start == -1 || S[start] < S[next])
      ret = max(ret, lis3(next) + 1);
  return ret;
}
```
- start = -1 이 주어질 수 있기 때문에 cache[start + 1]을 사용
- cache[]의 크기도 1 커짐
- lis3(-1)-1을 결과로 쓰면 됨

> #### 더 빠른 해법
O(nlgn)에 LIS를 찾을 수 있는 알고리즘이 있음.   
텅 빈 수열에서 시작해 숫자를 하나씩 추가해 나가며 각 길이를 갖는 증가 수열 중 가장 마지막 수가 작은 것은 무엇인지를 추적한다.   
예) S의 첫 다섯 원소가 {5, 6, 7, 1, 2} 일 때 부분 수열의 LIS는 길이가 3인 {5, 6, 7} 밖에 없음. 길이가 2인 부분 수열은 {5, 6}, {6, 7}, {1, 2} 이며 가장 유리한 것은 {1, 2}
```
C[i] = 지금까지 만든 부분 배열이 갖는 길이 i인 증가 부분 수열 중 최소의 마지막 값
```

### 최적화 문제 동적 계획법 레시피
1. 모든 답을 만들어보고 그 중 최적해의 점수를 반환하는 완전 탐색 알고리즘을 설계
2. 전체 답의 점수 반환이 아닌, 앞으로 남은 선택들에 해당하는 점수만을 반환하도록 부분 문제 정의를 바꾸기
3. 재귀 호출 입력에 이전 선택에 관련된 정보가 있다면 꼭 필요한 것만 남기고 줄이기. 문제에 최적 부분 구조가 성립된다면 완전히 없애기도 가능. 목표는 가능한 한 중복되는 부분 문제를 많이 만드는 것. 입력 종류가 줄어들수록 더 많은 부분 문제가 중복되고, 메모이제이션을 최대한도로 활용할 수 있음
4. 입력이 배열이거나 문자열인 경우 가능하다면 적절한 변환을 통해 메모이제이션 할 수 있도록 하기.
5. 메모이제이션을 적용

삼각형 문제에 적용한다면
1. 모든 경로를 만들어보고 전체 합 중 최대치 반환하는 완전 탐색 알고리즘 path1()
2. path1()이 전체 경로 최대합 반환하는 것이 아니라 (y,x)이후 만난 숫자들의 최대 합만 반환하도록 바꿈
3. sum입력을 받지 않음
4. 이 항목 필요하지 않았음
5. 메모이제이션 적용

## 8.5 문제: 합친 LIS(문제 ID: JLIS, 난이도 하)
두 개의 정수 수열 A와 B의 길이 0 이상의 증가 부분 수열을 얻은 뒤 이들을 크기 순서대로 합친 것 중 가장 긴 수열을 합친 LIS(JLIS, Joined Longest Increasing Subsequence)를 구하여 길이를 계산하는 프로그램 구하기

### 시간 및 메모리 제한
2초 안 실행, 64MB 이하의 메모리 사용
### 입력
첫 줄에는 테스트 케이스의 수 C(1이상 50이하). 각 테스트의 첫 줄에는 A와 B의 길이 n과 m(1이상 100이하)이 주어지고, 다음 줄에는 n개의 정수로 A의 원소들, 그 다음줄에는 m개의 정수로 B의 원소들이 주어집니다. 모든 원소들은 32비트 부호 있는 정수에 저장 가능

### 출력
각 테스트 케이스마다 한 줄에 JLIS의 길이를 출력

## 8.6 풀이: 합친 LIS
### 탐욕법으로 안된다.
두 수열의 LIS를 각각 찾은 뒤 이들을 합치면 되지 않음
### 비슷한 문제를 풀어본 적이 있군요
수열 S의 최대 증가 부분 수열을 찾는 재귀 함수 lis3()의 정의
```
lis3(start) = S[start]에서 시작하는 최대 증가 부분 수열의 길이
```
```
jlis(indexA, indexB) = A[indexA]와 B[indexB]에서 시작하는 합친 증가 부분 수열의 최대 길이
```

A[indexA]와 B[indexB]중 작은 쪽이 앞에 온다고 하면, 이 증가 부분 수열의 다음 숫자는 A[indexA+1] 이후 혹은 B[indexB+1] 이후의 수열 중 max(A[indexA], B[indexB])보다 큰 수 중 하나가 된다. 그리고 A[nextA]를 다음 숫자로 선택했을 경우에 합친 증가 부분 수열의 최대 길이는 1+jlis(nextA, indexB)가 된다.   

```C++
// 코드 8.13 합친 LIS 문제를 해결하는 동적 계획법 알고리즘

// 입력이 32비트 부호 있는 정수의 모든 값을 가질 수 있으므로
// 인위적인 최소치는 64비트여야 한다.
const long long NEGINF = numberic_limits<long long>::min();
int n, m, A[100], B[100];
int cache[101][101];

// min(A[indexA], B[indexB]), max(AindexA), B(indexB)로 시작하는
// 합친 증가 부분 수열의 최대 길이를 반환한다.
// 단 indexA == indexB == -1 혹은 A[indexA] != B[indexB]라고 가정한다.
int jlis(int indexA, int indexB) {
  // 메모이제이션
  int& ret = cache[indexA+1][indexB+1];
  if(ret != -1) return ret;
  // A[indexA], B[indexB]가 이미 존재하므로 2개는 항상 있다.
  ret = 2;
  long long a = (indexA == -1 ? NEGINF : A[indexA]);
  long long b = (indexB == -1 ? NEGINF : B[indexB]);
  long long maxElement = max(a, b);
  // 다음 원소를 찾는다.
  for (int nextA = indexA + 1; nextA < n; ++nextA)
    if (maxElement < A[indexA])
      ret = max(ret, jlis(nextA, indexB) + 1);
  for (int nextB = indexB + 1; nextB < m; ++nextB)
    if (maxElement < B[nextB])
      ret = max(ret, jlis(indexA, nextB) + 1);
  return ret;
}
```
- 아주 작은 값을 표현하기 위해 64비트 정수인 NEGINF를 사용
- 문제에는 입력의 범위가 32비트 부호 있는 정수 전체라고 나와 있기 때문에 입력에 등장하지 않은 작은 값을 쓰려면 64비트 정수를 써야 함

## 8.7 문제: 원주율 외우기( 문제 ID: PI, 난이도 하)
<table>
  <tr>
    <th>경우</th>
    <th>예</th>
    <th>난이도</th>
  </tr>
  <tr>
    <td>모든 숫자가 같을 때</td>
    <td>333, 5555</td>
    <td>1</td>
  </tr>
  <tr>
    <td>숫자가 1씩 단조 증가하거나 단조 감소</td>
    <td>2345, 3210</td>
    <td>2</td>
  </tr>
  <tr>
    <td>두개의 숫자가 번갈아가며 나타날때</td>
    <td>323, 545454</td>
    <td>4</td>
  </tr>
  <tr>
    <td>숫자가 등차 수열을 이룰 때</td>
    <td>147, 8642</td>
    <td>5</td>
  </tr>
  <tr>
    <td>이 외의 모든 경우</td>
    <td>17912, 331</td>
    <td>10</td>
  </tr>
</table>

원주율의 일부가 입력으로 주어질 때, 난이도의 합을 최소화하도록 숫자들을 세자리에서 다섯자리까지 끊어 읽고 싶은데, 최소의 난이도 계산하기

### 시간 및 메모리 제한
1초 이내, 64MB 이하 메모리 사용

### 입력
입력 첫 줄 테스트 케이스의 수 C(1이상 50이하), 그 후 C줄에 하나씩 각 테스트 케이스가 주어짐. 각 테스트 케이스는 8가지 이상 10,000자리 이하 자연수, 맨 앞자리가 0일수도 잇음

### 출력
각 테스트 케이스마다 한 줄에 최소의 난이도를 출력

## 8.8 풀이: 원주율 외우기
### 메모이제이션의 적용
적절한 완전 탐색 알고리즘을 만들면 메모이제이션으로 이 문제를 해결할 수 있음.   주어진 수열을 쪼개는 모든 방법을 하나씩 만들어 보며 그 중 난이도 합이 가장 작은 조합을 찾아 낸다. 각 경우마다 하나씩의 부분 문제를 풀어야 함. 세 개의 부분 문제에 대한 최적해를 각각 구했다고 하면, 전체 문제의 최적해는 다음 세 경우 중 가장 작은 값이 될 것
- 길이 3인 조각의 난이도 + 3글자 빼고 나머지 수열에 대한 최적해
- 길이 4인, + 4글자 빼고 최적해
- 길이 5인, + 5글자 빼고 최적해
나머지 수열의 최적해를 구할 때 앞의 부분 쪼갠 방법은 중요하지 않음(=최적 부분 구조가 성립). 따라서 시작 위치 begin이 주어질 때 최소 난이도를 반환하는 함수 memorize()를 정의하여 사용할 수 있음

- classify(): 난이도를 판정
- memorize(): 메모이제이션을 구현
- 시간 복잡도는 O(n<sup>2</sup>)
```C++
// 코드8.14 원주율 외우기 문제를 해결하는 동적 계획법 알고리즘

const int INF=987654321;
string N;
//N[a, b] 구간의 난이도를 반환한다.
int classify(int a, int b) {
  // 숫자 조각을 가져온다.
  string M = N.substr(a, b-a+1);
  // 첫 글자만으로 이루어진 문자열과 같으면 난이도는 1
  if (M == string(M.size(), M[0])) return 1;
  // 등차수열인지 검사한다.
  bool progressive = true;
  for (int i=0; i<M.size; ++i)
    if(M[i+1] - M[i] != M[1] - M[0])
      progressive = false;
  // 등차수열이고 공차가 1 혹은 -1이면 난이도는 2
  if (progressive && abs(M[1] - M[0]) == 1) return 2;
  // 두 수가 번갈아 등장인지 확인
  bool alternating = true;
  for(int i=0; i< M.size(); ++i)
    if(M[i] != M[i%2])
      alternating = false;
  if(alternating) return 4;
  if(!alternating) return 5;
  return 10; // 이외
}

int cache[10002];
// 수열 N[begin]를 외우는 방법 중 난이도의 최소 합을 출력한다.
int memorize(int begin) {
  // 기저 사례: 수열의 끝에 도달했을 경우
  if (begin == N.size()) return 0;
  // 메모이제이션
  int& ret = cache[begin];
  if(ret != -1) return ret;
  ret = INF;
  for (int L = 3; L<=5; ++L)
    if(begin + L <= L.size())
      ret = min(ret, memorize(begin + L) + classify(begin, begin + L -1));
  return ret;
}
```
