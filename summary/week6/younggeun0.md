## 동적 계획법(Dynamic Programming) 1/2

---

### 8.1 도입

#### 동적 프로그래밍 == 동적 계획법

#### 중복되는 부분 문제

* 큰 의미에서 분할 정복과 같은 접근 방식을 의미
  * **처음 주어진 문제를 더 작은 문제로 나눈 뒤 각 조각의 답을 계산하고, 이 답들로부터 원래 문제에 대한 답을 계산**
* **동적 계획법**에서는 여러번 계산되는 **어떤 부분 문제를 한 번만 계산하고 계산 결과를 재활용함**으로써 속도 향상을 꾀할 수 있음
  * 이미 계산한 값을 저장해 두는 메모리의 장소 - `캐시(Cache)`
  * 두 번 이상 계산되는 부분 문제 - **중복되는 부분 문제(Overlapping Subproblems)**
    * 나눠진 각 문제들이 같은 부분 문제에 의존하는 경우 발생
      * 계산의 중복횟수는 분할의 깊이가 깊어질 수록 지수적으로 증가
    * 문제 분할에 따라 등장하는 중복되는 부분 문제 - p180, 그림 7.2, p208 그림 8.1 참고
        
**동적 계획법 알고리즘 예 - 이항계수(Binomial Coefficient)**
  * 이항 계수 nCr 은 n개의 서로 다른 원소 중에서 r개의 원소를 순서없이 골라내는 방법의 수
  * 이항 계수는 아래와 같은 점화식(Recurrence)이 성립
    * 점화식이란 재귀적으로 정의되는 수학적 함수

![img1](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week6/img/img1.png?raw=true)

```c++
// code 8.1 재귀 호출을 이용한 이항 계수 계산

int bino(int n, int r) {
  // base case, n=r (모든 원소를 다 고른 경우) 혹은 r=0 (고를 원소가 없는 경우)
  // bino() 내에서는 반복문이 없기 때문에 재귀 호출이 몇 번 이뤄지는지 계산하면 수행 시간을 파악가능
  if (r == 0 || n == r) return 1;
  return bino(n-1, r-1) + bino(n-1, r);
}
```

* `bino(n, n/2)`에서 n이 하나 증가할 때마다 함수 호출의 수가 거의 두 배 증가
  * 입력 n과 r이 정해져 있을 때 bino(n, r)의 반환 값이 일정하다는 사실을 이용하면 중복 계산을 제거 가능
  * **함수의 결과를 저장하는 장소를 마련해 두고, 한 번 계산한 값을 저장해 뒀다 재활용하는 최적화 기법을 `메모이제이션(Memoization)`이라고 함**

```c++
// code 8.2 메모이제이션을 이용한 이항 계수 계산

// -1로 초기화
int cache[30][30];
int bino2(int n, int r) {
  if (r == 0 || n == r) return 1;   // base case

  // -1이 아니면 한 번 계산했던 값이므로 리턴
  if (cache[n][r] != -1)
    return cache[n][r];

  // 직접 계산한 뒤 배열에 저장
  return cache[n][r] = bino2(n-1, r-1) + bino2(n-1, r);
}
```

* 메모이제이션을 사용하면 모든 부분 문제가 한 번씩만 계산된다고 보장하여 함수 호출 횟수가 엄청나게 감소함
  * **이와 같이 두 번 이상 계산되는 부분 문제들의 답을 미리 저장함으로써 속도의 향상을 꾀하는 알고리즘 설계 기법을 동적계획법이라 함**



#### 메모이제이션을 적용할 수 있는 경우

* `참조적 투명 함수(Referential Transparent Function)`의 경우에만 적용 가능
   *  참조적 투명성(Referential Transparency) - 함수의 반환 값이 그 입력 값만으로 결정되는 여부
   *  프로그래밍은 수학의 함수와 다르게 여러 입력에 의해 출력이 바뀔 수 있음(ex 전역 변수, 입력 파일, 클래스 멤버 변수 등)

#### 메모이제이션 구현 패턴

* 메모이제이션은 동적 계획법으로 자주 구현되므로 항상 한 가지 패턴으로 구현하면 작성, 디버깅이 수월함
* **메모이제이션 구현 패턴**
  * 항상 기저 사례를 제일 먼저 처리한다
  * 캐시는 반환 값과 다른 값으로 초기화 한다
    * 캐시는 참조형(Reference)라는 점에 유의한다
  * 다중 for문을 사용하는 방법이 아닌 메모이제이션용 배열 초기화하는 방법을 알아두면 좋음
* **자신이 가장 좋다고 생각하는 방식을 하나 정해 일관되게 사용하는게 좋음**

```c++
// 코드 8.3 예제
// a, b는 각각 [0, 2500) 구간 안의 정수
// 반환 값은 항상 int 형 안에 들어가는 음이 아닌 정수
// - someObscureFunction은 한 번 처리하는 데 굉장히 시간이 오래 걸리는 참조적 투명 함수라 가정
int someObscureFunction(int a, int b);


// 위 함수를 메모이제이션으로 바꿔 구현한 예
int cache[2500][2500]; // 전부 -1로 초기화
int someObscureFunction(int a, int b) {
  // 기저 사례를 처음에 처리
  if (...) return ...;

  // (a,b)에 대한 답을 구한 적이 있으면 곧장 반환
  int& ret = cache[a][b];
  if (ret != -1) return ret;

  // 여기에서 답을 계산
  ...
  return ret;
}

int main() {
  // memset()을 이용해 cache배열을 초기화
  memset(cache, -1, sizeof(cache));
}
```

#### 메모이제이션 시간 복잡도 분석

* 메모이제이션을 사용하는 알고리즘의 시간 복잡도는 주먹구구로 상한 계산 가능

```
(존재하는 부분 문제의 수) * (한 부분 문제를 풀 때 필요한 반복문의 수행 횟수)
```

#### 예제 : 외발 뛰기 p215 참고

* 목적
  * n * n 격자로 이루어진 게임판에서 왼쪽 위칸에서 시작해 게임판 맨 오른쪽 아래 칸에 도착하는 것
* 문제
  * 게임판이 주어질 때 시작점에서 끝점에 도달하는 방법이 존재하는지를 확인하는 것
* 아래 또는 오른쪽으로 이동이 가능, 다음과 같이 재귀적 표현 가능
  * jump(y,x) = jump(y + jumpSize, x) || jump(y, x + jumpSize)

```c++
// 메모이제이션 전, code 8.4 외발뛰기 문제를 해결하는 재귀 호출 알고리즘
int n, board[100][100];
bool jump(int y, int x) {
  // base case1, 게임판 밖을 벗어난 경우
  if (y >= n || x >= n) return false;

  // base case2, 마지막 칸에 도착한 경우
  if (y == n-1 && x == n-1) return true;

  int jumpSize = board[y][x];
  return jump(y + jumpSize, x) || jump(y, x + jumpSize);
}

// 완전 탐색이 포기하기 전까지 만들어야 하는 경로의 개수는 n에 대해 지수적으로 늘어남
// 비둘기 집의 원리로 중복으로 해결되는 부분문제들이 항상 존재함
// jump()는 참조적 투명 함수, 메모이제이션을 이용해 중복된 연산제거 가능

// 메모이제이션 적용 후
int n, board[100][100];
int cache[100][100]; // -1로 초기화, 계산된 상태(1), 도착할 수 없으면(0)
int jump2(int y, int X) { // 반환형이 int
  if (y >= n || x >= n) return 0;
  if (y == n-1 && x == n-1) return 1;

  // 메모이제이션
  int& ret = cache[y][x];
  if (ret != -1) return ret;

  int jumpSize = board[y][x];
  return ret = jump2(y + jumpSize, x) || jump2(y, x + jumpSize); // 1 || 0 == 1, 1이 반환되면 도달가능
}
```

#### 동적 계획법 레시피

* 대개 동적 계획법 알고리즘은 아래 두 단계로 이루어짐

1. **주어진 문제를 완전 탐색을 이용해 해결**
2. **중복된 부분 문제를 한 번만 계산하도록 메모이제이션 적용**

* 재귀 호출을 이용하지 않고도 동적 계획법 알고리즘을 구현가능 == 반복적 동적 계획법

### 8.2 문제: 와일드카드 p218 참고

* `와일드카드`는 다양한 운영체제에서 파일 이름의 일부만으로 파일 이름을 지정하는 방법
  * 이 때 사용하는 문자열을 와일드카드 패턴이라고 함
  * 패턴은 특수 문자 `*`나 `?`를 포함할 수 있는 문자열
* 이 문제에서 `?`는 어떤 글자와도 대응되고, `*`은 0글자 이상의 어떤 문자열에도 대응됨
* 와일드 카드 패턴과 함께 파일명의 집합이 주어질 때, 패턴에 대응되는 파일명을 찾는 문제
  * `*`가 몇 글자에 대응되어야 하는지 모름 -> 완전 탐색

```c++
// code 8.6 와일드카드 문제를 해결하는 완전 탐색 알고리즘

// 와일드카드 패턴 w가 문자열 s에 대응되는지 여부를 반환함
bool match(const string&w, const string& s) {
  // w[pos]와 s[pos]를 맞춰 나감
  int pos = 0;

  // 대응 종료되는 경우를 찾기 위해 pos++
  // - 1. w[pos] != s[pos]
  // - 2. w가 끝에 도달 시 - *이 없는 경우
  // - 3. s가 끝에 도달 시 - 패턴은 남았지만 문자열이 끝난 경우
  // - 4. w[pos]가 *인 경우
  while (pos < s.size() && pos < w.size() &&
      (w[pos] == '?' || w[pos] == s[pos]))
    ++pos;

  // 더 이상 대응할 수 없으면 왜 while문이 끝났는지 확인

  // 2. 패턴 끝에 도달해서 끝난 경우: 문자열도 끝났어야 대응됨
  if (pos == w.size()) {
    return pos == s.size(); // 3.
  }

  // 4. *를 만나서 끝난 경우 : *에 몇 글자를 대응해야 할지 재귀 호출하면서 확인
  if (w[pos] == '*')
    for (int skip = 0; pos+skip <= s.size(); ++skip)
      if (match(w.substr(pos+1), s.substr(pos+skip)))
        return true;
  
  // 이 외의 경우에는 모두 대응되지 않음
  return false;
}
```

#### 중복되는 문제

* 입력으로 주어질 수 있는 w와 s의 종류는 제한되어 있음 == 재귀호출 시 항상 w, s 앞에서만 글자를 떼어내기 때문에 w와 s는 항상 입력에 주어진 패턴 W와 파일명 S의 접미사가 됨(suffix)
  * 입력으로 주어질 수 있는 w와 s는 최대 101개밖에 없음(문자열의 길이는 각각 최대 100)
    * C, C++에서 문자열의 끝을 알리기 위해 널문자를 사용
    * [참고 - C언어 문자열 끝에 널문자가 필요한 이유](https://m.blog.naver.com/PostView.nhn?blogId=jsky10503&logNo=221131412375&proxyReferer=https:%2F%2Fwww.google.com%2F)
* match가 101 * 101 = 10201번 호출됨, 비둘기집의 원리에 따라 어떤 부분 문제가 중복 계산되고 있음
* 메모이제이션 사용해 중복제거

```c++
// code 8.7
// -1 == 계산 전
// 1 == 대응됨
// 0 == 대응되지 않음
int cache[101][101];
string W, S //패턴과 문자열
// 와일드카드 패턴 W[w..]가 문자열 S[s..]에 대응되는지 여부를 반환
bool matchMemoized(int w, int s) {
  // 메모이제이션
  int& ret = cache[w][s];
  if (ret != -1) return ret;

  // W[w] 와 S[s]를 맞춰 나감
  while(s < S.size() && w < W.size() &&
    (W[w] == '?' || W[w] == S[s])) {
      ++w;
      ++s;
  }
  // 더 이상 대응할 수 없으면 왜 while문이 끝났는지 확인
  // 2. 패턴 끝에 도달해 끝난 경우, 문자열도 끝났어야 함
  if (w == W.size())
    return ret = (s == S.size());

  // 4. *를 만나서 끝난 경우, *에 몇 글자를 대응해야 할지 재귀호출하면서 확인
  if (W[w] == '*') 
    for (int skip = 0; skip+s <= S.size(); ++skip) 
      if (matchMemoized(w+1, s+skip))
        return ret = 1;
  
  // 3. 이 외의 경우에는 모두 대응되지 않음
  return ret = 0;
}
```

* 패턴과 문자열의 길이가 모두 n일 때 부분 문제의 개수는 n<sup>2</sup>, matchMemoized()는 한 번 호출될 때마다 최대 n번의 재귀 호출을 하기 때문에 시간 복잡도는 O(n<sup>3</sup>)

#### 다른 분해 방법

* 좀 더 똑똑한 분해 방식을 쓰면 O(n<sup>2</sup>)가 가능
  * O(n<sup>3</sup>)가 걸리는 이유는 내부에서 첫 *를 찾고 *에 몇 글자가 대응되어야 할지 검사하는 반복문이 존재하기 때문
    * 재귀 함수 자체에 반복문이 하나도 없도록 리펙토링하면 부분 문제 개수와 같은 시간만을 사용해 문제를 풀 수 있음
* code 8.7에서 while 문 조건을 통과했다는 의미는 두 글자가 대응된다는 의미
  * w, s를 1씩 증가시키는 대신 패턴과 문자열의 첫 한 글자씩을 떼고 서로 대응되는지 재귀호출로 확인가능

```c++
// before
// W[w] 와 S[s]를 맞춰 나감
while(s < S.size() && w < W.size() &&
  (W[w] == '?' || W[w] == S[s])) {
    ++w;
    ++s;
}

// after
if (w < W.size() && s < S.size() &&
  (W[w] == '?' || W[w] == S[s]))
  return ret = matchMemoized(w+1, s+1);
```

* `*`에 몇 글자가 대응되어야 할지를 확인하는 반복문을 재귀 호출로 변한
  * 매 단계에서 `*`에 아무 글자도 대응시키지 않을 것인지, 아니면 한 글자를 더 대응시킬 것인가를 결정하면 됨

```c++
// 4. *를 만나서 끝난 경우, *에 몇 글자를 대응해야 할지 재귀호출하면서 확인
if (W[w] == '*') 
  if (matchMemoized(w+1, s) ||
    (s < S.size() && matchMemoized(w, s+1)))
    return ret = 1;
```

* n번의 반복문을 제거해서 시간 복잡도는 O(n<sup>2</sup>)가 됨


### 8.4 전통적 최적화 문제들

* `최적화 문제`란 여러 개의 가능한 답 중 가장 좋은 답(최적해)을 찾아내는 문제
  * 동적 계획법은 처음에 최적화 문제의 해답을 빨리 찾기 위해 노력하는 과정에서 고안됨
* 최적화 문제를 동적 계획법으로 푸는 것 또한 완전 탐색에서 시작하지만 최적화 문제 특정 성질이 성립할 경우 단순히 메모이제이션을 적용하기 보다 좀 더 효율적으로 동적 계획법을 구현가능

#### 예제 : 삼각형 위의 최대 경로 p226 참고

* 맨 위를 숫자에서 시작해서 한 번에 한 칸씩 아래로 내려가 맨 아래 줄까지 닿는 경로를 만들려 함
  * 경로는 바로 아래 숫자 혹은 오른쪽 아래 숫자로 내려갈 수 있음
  * 모든 경로 중 숫자의 합을 최대화하는 경로와 경로와 포함된 숫자들의 최대 합을 구하는 문제
* 완전 탐색으로 시작, 책 점화식 참고

#### 무식하게 메모이제이션 적용하기

* 최초 점화식으로 만든 코드

```c++
// code 8.8
// MAX_NUMBER : 한 칸에 들어갈 숫자의 최대치
int n, triangle[100][100];
int cache[100][100][MAX_NUMBER*100+1];

// (y,x) 위치까지 내려오기 전에 만난 숫자들의 합이 sum 일 때
// 맨 아래줄까지 내려가면서 얻을 수 있는 최대 경로를 반환함
int path1(int y, int x, int sum) {
  // base case, 맨 아래 줄까지 도달한 경우
  if (y == n-1) return sum + trangle[y][x];

  // 메모이제이션
  int& ret = cache[y][x][sum];
  if (ret != -1)
    return ret;

  sum += triangle[y][x];
  return ret = max(path1(y+1, x+1, sum), path1(y+1, x, sum));
}
```

* 위 코드의 문제점
  * 배열의 크기가 입력으로 주어지는 숫자의 범위에 좌우되어 사용하는 메모리가 너무 큼
  * 특정 입력에 대해서는 완전 탐색처럼 동작하는 문제가 존재
    * 경로 위 모든 숫자가 다르면 중복 계산이 발생하지 않음

#### 입력 걸러내기

* 위 코드를 보면 입력 x, y만 알아도 sum을 구할 수 있음, sum을 제거
* 함수의 반환 값을 전체 경로의 최대치가 아니라 (y,x)에서 시작하는 부분 경로의 최대치로 변경
  * 수정된 점화식은 p229 참고

```c++
// code 8.9
int n, triangle[100][100];
int cache2[100][100];

// (y,x) 위치부터 맨 아래줄까지 내려가면서 얻을 수 있는 최대 경로의 합을 반환
int path2(int y, int x) {
  // base case, 맨 아래 줄까지 도달한 경우
  if (y == n-1) return trangle[y][x];

  // 메모이제이션
  int& ret = cache2[y][x];
  if (ret != -1)
    return ret;

  return ret = max(path2(y+1, x), path2(y+1, x+1)) + triangle[y][x];
}
```

#### 이론적 배경 : 최적 부분 구조(Optimal Substructure)

* 위처럼 최적화가 가능했던 이유는 sum이라는 정보가 (y,x)에서 맨 아래줄까지 내려가는 문제를 해결하는 데 아무 상관이 없다는 사실을 파악했기 때문
  * == 지금까지 어떤 경로로 이 부분 문제에 도달했건 남은 부분 문제는 항상 최적으로 풀어도 상관 없다는 뜻
* `최적 부분 구조(Optimal Substructure)`
  * 어떤 문제와 분할 방식에 성립하는 조건, 각 부분 문제의 최적해만 있으면 전체 문제의 최적해를 쉽게 얻어낼 수 있을 경우 이 조건이 성립한다고 함
  * 최적 부분 구조는 직관적으로 이해할 수 있어 따로 증명이 필요하지 않음, 직관적이지 않은 경우 귀류법 또는 대우로 증명함

#### 예제 : 최대 증가 부분 수열(LIS, Longest Increasing Subsequence)

* 주어진 수열에서 얻을 수 있는 증가 부분 수열 중 가장 긴 것을 찾는 문제 == 최대 증가 부분 수열 찾기 문제
  * 부분 수열에 포함된 숫자들이 순 증가(Strictly Increasing)하면 이 부분 수열을 `증가 부분 수열`이라고 부름
* 숫자 하나씩 조각낸 뒤 한 조각에서 숫자 하나씩을 선택하는 완전 탐색 알고리즘으로 시작

```c++
// code 8.10
int lis(const vector<int>& A) {
  // base case, A가 비었을 때
  if(A.empty()) return 0;

  int ret = 0;
  for(int i=0; i < A.size(); i++) {
    vector<int> B; // 부분 수열을 저장용
    for(int j = i+1; j < A.size(); j++) {
      if (A[i] < A[j]) // 선택된 숫자보다 큰 수만 추가
        B.push_back(A[j]);
    }
    // 재귀 호출로 완전탐색
    ret = max(ret, 1 + lis(B)); // 1을 더하는 이유는 A[i]의 숫자의 길이를 추가해주기 위함
  }
  return ret;
}
```

#### 입력 손보기

* 입력이 정수가 아니라 정수의 배열이라 메모이제이션을 적용하기 까다로움
  * A의 정의를 이용, 좀 더 간단하게 표현
    * A는 원래 주어진 수열이거나 원래 주어진 수열의 원소 S\[i\]에 대해, S\[i+1...\] 부분 수열에서 S\[i\]보다 큰 수들만 포함하는 부분 수열
      * 이전에 선택한 수들이 정의에 포함되지 않음
    * 즉, **lis(start) == S\[start\]에서 시작하는 부분 증가 수열 중 최대의 길이**

```c++
// code 8.11
int n;
int cache[100], S[100];

// S[start]에서 시작하는 증가 부분 수열 중 최대 길이를 반환
int lis2(int start) {
  int& ret = cache[start];
  if (ret != -1)
    return ret;

  // 항상 S[start]는 있기 때문에 길이는 최하 1
  ret = 1;

  // 재귀호출을 할 때마다 S[start]보다 뒤에 있고 큰 수들 중 하나를 다음 숫자로 결정한 뒤, 여기서 시작하는 부분 증가 수열의 최대치를 구함
  for (int next = start+1; next < n; ++next)
    if (S[start] < S[next])
      ret = max(ret, lis2(next) + 1);

  return ret;
}
```

#### 시작위치 고정하기

* lis2()를 호출할 때 항상 증가 부분 수열의 시작 위치를 지정해 줘야 함
  * 처음 호출할 때 각 위치를 순회하면서 최대 값을 찾는 다음과 같은 코드를 작성해야 함

```c++
int maxLen = 0;
for (int begin = 0; begin < n; ++begin)
  maxLen = max(maxLen, lis2(begin));
```

* 위 코드를 변형을 통해 없앨 수 있음
  * S\[-1\] = -Infinity 라고 가정하는 코드를 작성
  * lis2(-1)을 호출하면 항상 -Infinity부터 시작하니까 모든 시작 위치를 자동으로 시도하게 됨
    * start=-1이 주어질 수 있어 cache[]에 접근할 때 cache\[start\] 대신 cache\[start+1\]을 사용
    * lis3(-1)-1을 결과로 쓰면 됨
      * -1일 때도 +1이 되기 때문에 -1처리로 길이 보정

```c++
// code 8.12
int n;
int cache[101], S[100];

// S[start]에서 시작하는 증가 부분 수열 중 최대 길이를 반환
int lis3(int start) {
  int& ret = cache[start + 1];
  if (ret != -1)
    return ret;

  // 항상 S[start]는 있기 때문에 길이는 최하 1
  ret = 1;

  for (int next = start+1; next < n; ++next)
    if (start == -1 || S[start] < S[next])
      ret = max(ret, lis3(next) + 1);

  return ret;
}
```

#### 더 빠른 해법

* LIS를 찾는 가장 빠른 방법은 O(nlogn), 위 방법은 O(n<sup>2</sup>)
* 가장 빠른 알고리즘은 텅 빈 수열에서 시작해 숫자를 하나씩 추가해 나가며 각 길이를 갖는 증가 수열 중 가장 마지막 수가 작은 것이 무엇인지 추적함
  * `C[i]=지금까지 만든 부분 배열이 갖는 길이 i인 증가 부분 수열 중 최소의 마지막 값` 을 이용하도록 코드를 개선하면 LIS 길이 k에 대해 O(nk) 시간에 LIS를 찾을 수 있음
  * C[]가 항상 순증가한다는 사실을 깨닫고 C[]에서 이분 검색을 하면 LIS를 O(nlogk) <= O(nlogn)에 찾는 알고리즘을 만들 수 있음

#### 최적화 문제 동적 계획법 레시피

* 최적화 문제의 동적 계획법을 설계하기 위한 과정을 간략히 정리

> 1. 모든 답을 만들어 보고 그 중 최적해의 점수를 반환하는 완전 탐색 알고리즘을 설계
> 2. 전체 답의 점수를 반환하는 것이 아니라, 앞으로 남은 선택들에 해당하는 점수만을 반환하도록 부분 문제 정의를 바꿈
> 3. 재귀 호출의 입력에 이전 선택에 관련된 정보가 있다면 꼭 필요한 것만 남기고 줄임 (중복 부분 문제를 많이  만들어 메모이제이션 효과를 극대화)
> 4. 입력이 배열이거나 문자열인 경우 가능하다면 적절한 변환을 통해 메모이제이션할 수 있도록 함
> 5. 메모이제이션 적용

* 위 삼각형 최대 경로 문제 해결 과정을 적용한 예

> 1. 모든 경로를 만들어보고 전체 합 중 최대치를 반환하는 완전 탐색 알고리즘 path1()을 만듦
> 2. path1()이 전체 경로의 최대 합을 반환하는 것이 아니라 (y,x) 이후로 만난 숫자들의 최대 합만을 반환하도록 > 바꿈
> 3. 반환 값의 정의를 바꿔 이전에 한 선택에 대한 정보인 sum을 입력받을 필요가 없어짐(최적 부분 구조가 성립하도록 수정)
> 4. 이 항목은 필요하지 않음
> 5. 메모이제이션 적용

### 8.5 문제 : 합친 LIS p236 참고

* 어떤 수열에서 0개 이상의 숫자를 지운 결과를 원 수열의 `부분 수열`이라고 함
* 중복된 숫자가 없고 오름 차순으로 정렬되어 있는 부분 수열들을 `증가 부분 수열`이라고 함
* 두 개의 정수 수열 A, B에서 각각 길이 0 이상의 증가 부분 수열을 얻은 디ㅜ 이들을 크기 순서대로 합친 것을 `합친 증가 부분 수열`이라 함
  * 이 중 가장 긴 수열을 `합친 LIS(JLIS, Joined Longest Increasing Subsequence)`라고 함
* A와 B가 주어졌을 때, 합친 LIS의 길이를 계산하는 프로그램을 작성하는 문제

### 8.6 풀이 : 합친 LIS

#### 탐욕법으로는 안됨

* [탐욕 알고리즘](https://ko.wikipedia.org/wiki/%ED%83%90%EC%9A%95_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
  * 최적해를 구하는 데에 사용되는 근사적인 방법으로, 여러 경우 중 하나를 결정해야 할 때마다 그 순간에 최적이라고 생각되는 것을 선택해 나가는 방식으로 진행하여 최종적인 해답에 도달하는 알고리즘
* 두 수열의 LIS를 각각 찾은 뒤 이들을 합치는 방법은 사용하지 못함
  * 예제 3번째 입력에서 JLIS를 찾기 위해선 A에서 1 2를 선택해야하지만 A의 LIS는 10 20 30, B의 LIS는 10 20 30이므로 두 개를 합치는 걸로는 못 풂
* 이렇게 비슷한 문제들은 비슷한 형태의 부분 문제 정의를 써서 풀 수 있는 경우가 많음
  * 위 수열 S의 최대 증가 부분 수열을 찾는 재귀 함수 lis3()의 정의는 `lis3(start) == S[start]에서 시작하는 최대 증가 부분 수열의 길이` 였음
* 입력 수열이 늘었으니 재귀 함수도 두 개의 입력을 받아야 함
  * jlis(indexA, indexB) = A\[indexA\]와 B\[indexB\] 에서 시작하는 합친 증가 부분 수열의 최대 길이
  * 두 수의 순서는 지정하지 않았으므로 A\[indexA\]와 B\[indexB\] 중 작은 쪽이 앞에 옴
    * 이 증가 부분 수열의 다음 숫자는 A\[indexA+1\] 이후 혹은 B\[indexB+1\] 이후의 수열 중 max(A\[indexA\], B\[indexB\]) 보다 큰 수 중 하나가 됨
    * A\[nextA\]를 다음 자로 선택했을 경우 합친 증가 부분 수열의 최대 길이는 1+jlis(nextA, indexB)가 됨
* 점화식은 p238 참고

```c++
// code 8.13
// 입력이 32비트 부호 있는 정수의 모든 값을 가질 수 있으므로 인위적인 최소치는 64비트여야 함
const long long NEGINF = nemeric_limits<long long>::min();
int n, m, A[100], B[100];
int cache[101][101];

// min(A[indexA], B[indexB]), max(A[indexA], B[indexB])로 시작하는 합친 부분 증가 수열의 최대 길이를 반환
// 단, indexA == indexB == -1 혹은 A[indexA] != B[indexB]라고 가정
int jlis(int indexA, int indexB) {
  // 메모이제이션
  int& ret = cache[indexA+1][indexB+1];
  if (ret != -1)
    return ret;

  // A[indexA], B[indexB]가 이미 존재하므로 2개는 항상 있음
  ret = 2;

  long long a = (indexA == -1 ? NEGINF : A[indexA]);
  long long b = (indexB == -1 ? NEGINF : B[indexB]);
  long long maxElement = max(a, b);

  // 다음 원소를 찾음
  for (int nextA = indexA + 1; nextA < n; ++nextA)
    if (maxElement < A[indexA])
      ret = max(ret, jlis(nextA, indexB) + 1); 
  for (int nextB = indexB + 1; nextB < n; ++nextB)
    if (maxElement < A[indexB])
      ret = max(ret, jlis(indexA, nextB) + 1);

  return ret;
}
```

### 8.7 문제 : 원주율 외우기 p239 참고

* 원주율의 일부가 입력으로 주어질 때 숫자들을 세 자리에서 다섯 자리까지 끊어 최소의 난이도를 계산하는 문제
  * 난이도는 p240 표 참고
### 8.8 풀이 : 원주율 외우기

* 입력의 크기를 보면 완전 탐색은 불가능
  * 적절한 완전 탐색 알고리즘을 만들면 메모이제이션으로 해결가능
* 수열을 쪼개는 모든 방법을 하나씩 만들어 보고 그 중 난이도의 합이 가장 작은 조합을 찾아내는 완전 탐색 알고리즘으로 풀 수 있음
  * 각 재귀 함수는 한 번 불릴 때마다 첫 조각의 길이를 하나하나 시도하며 남은 수열을 재귀적으로 쪼갬
  * 3,4,5 세 개의 부분 문제에 대한 최적해를 각각 구했다고 하면, 전체 문제의 최적해는 다음 세 경우 중 가장 작은 값이 됨

> 길이 3인 조각의 난이도 +3글자 빼고 나머지 수열에 대한 최적해
> 
> 길이 4인 조각의 난이도 +4글자 빼고 나머지 수열에 대한 최적해
> 
> 길이 5인 조각의 난이도 +5글자 빼고 나머지 수열에 대한 최적해

* 나머지 수열의 최적해를 구할 때 앞의 부분은 어떤 식으로 쪼갰는지 중요하지 않음(최적 부분 구조가 성립)
* p242 점화식 참고

#### 구현

* 숫자 한 조각이 주어졌을 때 난이도를 판정하는 classify()와 실제 메모이제이션을 구현하는 memorize()로 나뉨

```c++
// code 8.14
const int INF = 987654321;
string N;

// N[a..b] 구간의 난이도를 반환하는 classify
int classify(int a, int b) {
  // 숫자 조각을 가져옴
  string M = N.substr(a, b-a+1);

  // 첫 글자만으로 이루어진 문자열과 같으면 난이도는 1
  if (M == string(M.size(), M[0])) return 1;

  // 등차 수열인지 검사
  bool progressive =true;
  for (int i=0; i<M.size(); i++)
    if (M[i+1] - M[i] != M[1] - M[0])
      progressive = false;
  
  // 등차 수열이고 공차가 1혹은 -1이면 난이도는 2
  if (progressive && abs(M[1] - M[0]) == 1)
    return 2;

  // 두 수가 번갈아 등장하는지 확인
  bool alternating = true;
  for (int i=0; i<M.size(); i++)
    if (M[i] != M[i%2])
      alternating = false;
  
  if (alternating) return 4; // 두 수가 번갈아 등장하면 난이도 4
  if (progressive) return 5; // 공차가 1이 아닌 등차 수열 난이도 5
  return 10; // 이 외는 모두 난이도 10
}

int cache[10002];
// 수열 N[begin..]를 외우는 방법 중 난이도의 최소 합을 출력
int memorize(int begin) {
  // base case, 수열의 끝에 도달한 경우
  if (begin == N.size())
    return 0;

  // 메모이제이션
  int& ret = cache[begin];
  if (ret != -1)
    return ret;

  ret = INF;

  for (int L = 3; L <= 5; L++)
    if (begin + L <= N.size())
      ret = min(ret, memorize(begin + L)
                      + classify(begin, begin + L - 1));
  
  return ret;
}
```
