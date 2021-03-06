## 동적 계획법(Dynamic Programming) 2/2

---


### 지난 내용 복습

* **재귀 함수**
  * **자신이 수행할 작업을 수행하고 나머지를 자기 자신을 호출해 실행하는 함수**
  * 모든 재귀 함수는 최소한의 작업에 도달했을 때 답을 곧장 반환하는 조건문(기저사례)을 포함해야 함
    * 쪼개지지 않는 가장 작은 작업을 `기저 사례(Base Case)`라 함

```c++
int recursiveSum(int n) {
    if(n == 1) return 1; // 더 이상 쪼개지지 않을 때 -> '기저사례(Base Case)'
    return n + recursiveSum(n-1);
}
```

* **완전 탐색 알고리즘**
  * **가능한 방법을 전부 만들어 보는 알고리즘**
    * == `무식하게 풀기` == `Brute-Force 알고리즘`
  * 주로 재귀 호출을 사용해 완전 탐색을 수행
  * 완전 탐색은 존재하는 모든 답을 하나씩 검사하므로, 걸리는 시간은 가능한 답의 수에 비례
  * 완전 탐색 레시피

> 1. 가능한 모든 답의 후보를 만드는 과정을 여러 개의 선택으로 나눔. (각 선택은 답의 후보를 만드는 과정의 한 조각이 됨)
> 2. 그 중 하나의 조각을 선택해 답의 일부를 만들고, 나머지 답을 재귀 호출을 통해 완성
> 3. 조각이 하나밖에 남지 않은 경우, 혹은 하나도 남지 않은 경우에는 답을 생성했으므로 이것을 기저 사례로 선택해 처리

* **분할 정복 알고리즘(Divide & Conquer Algorithm)**
  * **해결할 수 없는 문제를 작은 문제로 분할하여 문제를 해결하는 방법이나 알고리즘**
  * 본 패러다임을 차용한 알고리즘들은 한 문제를 둘 이상의 부분 문제로 나눠 각 부분 문제를 재귀호출로 계산한 뒤 부분 문제의 답으로 전체 문제의 답을 계산함
  * 퀵 정렬, 병합 정렬이 대표적인 예

```bash
// 위키에 있던 수도코드 예제
function F(x):
  if F(x)의 문제가 간단 then:
    return F(x)을 직접 계산한 값
  else:
    x 를 y1, y2로 분할
    F(y1)과 F(y2)를 호출
    return F(y1), F(y2)로부터 F(x)를 구한 값
```

* **동적 프로그래밍(Dynamic Programming)**
  * == `동적 계획법`
  * 큰 의미에서 분할 정복과 같은 접근 방식을 의미
  * **처음 주어진 문제를 더 작은 문제들로 나눈 뒤 각 조각의 답을 계산하고, 이 답들로부터 원래 문제에 대한 답을 계산해 내는 방식이다**
  * 어떤 문제는 두 개 이상의 문제를 푸는 데 사용될 수 있기 때문에, 이 문제의 답을 여러 번 계산하는 대신 한 번만 계산하고 계산 결과를 재활용하여 속도를 향상함. 그러기 위해 각 문제의 답을 저장해 두는 메모리의 장소를 캐시(cache)라고 부르며, 두번 이상 계산되는 부분 문제를 중복되는 부분 문제(overlapping subproblems)라고 부름.
    * 중복되는 부분 문제 연산을 캐시를 이용해 비용을 줄이는 기법을 `메모이제이션(Memoization)` 이라고 함
  * 동적 프로그래밍 레시피

> 1. 주어진 문제를 완전 탐색을 이용해 해결
> 2. 중복된 문제를 한 번만 계산하도록 메모이제이션 적용

* **최적화 문제(ㅇ<-<)**
  * **여러 개의 가능한 답 중 가장 좋은 답(최적해)을 찾아내는 문제**
    * 동적 계획법은 처음에 최적화 문제의 해답을 빨리 찾기 위해 노력하는 과정에서 고안됨
    * 최적화 문제를 동적 계획법으로 푸는 것 또한 완전 탐색에서 시작하지만 최적화 문제 특정 성질이 성립할 경우 단순히 메모이제이션을 적용하기 보다 좀 더 효율적으로 동적 계획법을 구현가능
  * `최적 부분 구조(Optimal Substructure)`
    * 어떤 문제와 분할 방식에 성립하는 조건
    * 각 부분 문제의 최적해만 있으면 전체 문제의 최적해를 쉽게 얻어낼 수 있을 경우 이 조건이 성립한다고 함(...)
  * 최적화 문제 동적 계획법 레시피

> 1. 모든 답을 만들어 보고 그 중 최적해의 점수를 반환하는 완전 탐색 알고리즘을 설계
> 2. 전체 답의 점수를 반환하는 것이 아니라, 앞으로 남은 선택들에 해당하는 점수만을 반환하도록 부분 문제 정의를 바꿈
> 3. 재귀 호출의 입력에 이전 선택에 관련된 정보가 있다면 꼭 필요한 것만 남기고 줄임 (중복 부분 문제를 많이 만들어 메모이제이션 효과를 극대화)
> 4. 입력이 배열이거나 문자열인 경우 가능하다면 적절한 변환을 통해 메모이제이션할 수 있도록 함
> 5. 메모이제이션 적용


---


### 8.9 문제 : Quantization p244 참고

* Quantization(양자화)는 더 넓은 범위를 갖는 값들을 작은 범위를 갖는 값들로 근사해 표현함으로써 자료를 손실 압축하는 것
  * 양자화 예시
    * 16비트 JPG 파일을 4컬러 GIF 파일로 변환하는 작업
    * 키가 161, 164, 178, 184인 사람들을 160대 둘, 170 하나, 180대 하나로 축약표현하는 것
* 1000 이하의 자연수들로 구성된 수열을 s가지의 자연수만을 사용하도록 양자화할 때 숫자별 오차 제곱의 합의 최소화치를 구하는 문제
  * 예시 - 수열 1 2 3 4 5
    * 2 2 3 3 3으로 양자화 했을 때
      * 각 숫자별 오차는 -1, 0, 0, 1, 2이고, 오차 제곱의 합은 1+0+0+1+4=6이 됨
    * 2 2 2 4 4로 양자화 했을 때
      * 각 숫자별 오차는 -1, 0, -1, 0, 1이고, 오차 제곱의 합은 1+0+1+0+1=3이 됨
  * 입력
    * 테스트 케이스 수 C(1<=C<=50)
    * 수열의 길이 n(1<=n<=100)
    * 사용할 숫자의 수 s(1<=s<=10)
    * n개의 정수 수열의 숫자들(수열의 모든 수는 1000이하의 자연수)
  * 출력
    * 각 테스트 케이스마다 한 줄에 주어진 수열을 s개의 수로 양자화할 때 오차 제곱의 합의 최소치를 출력

### 8.10 풀이 : Quantization

#### 하던 대로 안됨

* 앞에 풀었던 문제들처럼 재귀적인 해법으로는 풀 수 없음
  * == 양자화된 결과 수열을 재귀적으로 구해 최소 오차 제곱을 구해 반환하는 방법

> quantize(A) = A에 속한 수를 양자화해서 얻을 수 있는 최소 오차 제곱의 합

* 이 문제에서는 사용할 수 있는 숫자의 가짓수에 제한이 존재(s)
  * 남은 문제를 재귀적으로 해결할 때 지금까지 사용한 숫자들을 무시할 수 없음, 이미 s가지 숫자를 다 쓴 상태면 이 중 하나의 숫자를 선택해야 하기 때문
  * == 최적 부분 조건이 성립하지 않음
* quantize()는 남은 숫자들만이 아니라 이전 숫자들은 어떤 숫자로 양자화했는지 또한 알아야 함
  * 때문에 지금까지 사용한 숫자들의 집합 또는 입력으로 받아야 함

> quantize(A, U) = U가 지금까지 한 번 이상 사용한 숫자들의 집합일 때 A에 속한 수를 양자화해서 얻을 수 있는 최소 오차 제곱의 합

* 이렇게 변경한 quantize함수는 A의 첫 번째 숫자를 어떻게 표현할지 결정하고 나머지를 재귀호출해서 해결하게 됨
  * 그러나 이런 완전 탐색 코드는 엄청나게 많은 수의 답을 만들게 됨(n이 1000이고 U가 10이라 가정하면 앞으로 990개가 추가로 n번의 재귀호출로 완전 탐색을 하게됨 ...(안끝남))

#### 답의 형태 제한하기

* 이와 같이 **부분 문제의 개수가 너무 많을 땐 답이 항상 어떤 구조를 가질 것이라고 예측하고 그것을 강제하는 방법을 사용해볼 수 있음**
  * 이 문제에서는 주어진 수열을 정렬하면, 같은 숫자로 양자화되는 숫자들은 항상 인접해 있음을 알 수 있음
* 우선 입력에서 주어진 수를 정렬, 인접한 숫자끼리 묶음으로 적절히 분할, 각 묶음을 한 숫자로 표현 => 오류를 최소화

```c++
{1, 4, 6, 744, 755, 777,890, 897, 902}

{1, 4, 6}, {744, 755, 777, 890, 897, 902}

{4, 4, 4}, {759, 759, 759}, {896, 896, 896}
```

* 이렇게 되면 이 문제는 주어진 수열을 s개의 묶음으로 나누는 문제가 됨
  * 재귀호출에서 첫 묶음의 크기가 얼마인지를 결정하면 됨
  * 첫 묶음의 크기가 x라고 하면 나머지 n-x개의 숫자를 s-1개의 묶음으로 나누면 됨
  * 이 때 나머지 s-1 묶음의 오류 합이 최소여야 전체도 최소 오류이므로 최적 부분 구조 또한 성립

> quantize(from, parts) = from번째 이후의 숫자들을 parts개의 묶음으로 묶을 때 최소 오류 제곱 합을 반환하는 함수

> minError(a,b)는 a번째 숫자부터 b번째 숫자까지 하나의 수로 표현했을 때의 최소 오류를 반환

* 첫 번째 묶음의 크기가 size일 때 최소 오류는 minError(from, from + size -1) + quantize(from + size, parts - 1)이 됨
  * 점화식은 p247 참고

#### 한 개의 구간에 대한 답 찾기, 구현 생략(p248~251)

---

### 8.11 경우의 수와 확률

* **동적 계획법은 최적화 문제를 풀기 위해 고안되었으나 경우의 수를 세거나 확률을 계산하는 문제에도 흔하게 사용됨**
  * 경우의 수를 계산하는 문제는 많은 경우 재귀적인 특징을 갖기 때문(ex. 이항 계수 문제)

#### 오버플로에 유의하기

* 대개 경우의 수를 세는 문제에서 답은 입력의 크기에 대해 지수적으로 증가
  * 때문에 일반적으로 32비트 정수형의 한계를 초과하기 십상
  * 때문에 대부분의 문제에서는 답을 어떤 수 M으로 나눈 나머지를 출력하기를 요구하는 식으로 이런 현상을 해결함
    * 모듈라 연산(나머지 연산)에 관련된 식을 미리 알아두는게 좋다고 함(14.8절)
      * [나머지 - wiki](https://ko.wikipedia.org/wiki/%EB%82%98%EB%A8%B8%EC%A7%80)

#### 예제 : 타일링 방법의 수 세기 p252 참고

* 2\*n 크기의 사각형을 2*1 크기의 타일로 채우는 방법의 수를 계산하는 문제
  * 타일들은 서로 겹쳐서는 안 되고, 90도로 회전해서 쓸 수 있음(p252, n=5 일 때 그림 8.7 참고)

* **풀이 접근법**
1. 완전 탐색을 이용 모든 답을 만들면서 개수를 세어보는 함수를 작성
2. 메모이제이션을 이용해 동적 계획법 알고리즘으로 변경

* 재귀호출을 이용해 모든 타일링 방법을 만들려면 각 타일링 방법을 여러 조각으로 쪼개고 재귀 호출될 때마다 한 조각씩 만들어 나가면 됨
  * 2*n 사각형을 채우는 모든 방법들은 맨 왼쪽 세로줄이 어떻게 채워져 있느냐로 나눌 수 있음
    * 세로줄 한 개 또는 두 개의 가로 타일
      * **이 두 가지 분류는 타일링하는 방법을 모두 포함**
      * **두 가지 분류에 모두 포함되는 타일링 방법은 없음**
      * 위 두 가지 속성은 경우의 수를 셀 때 항상 확인해야 하는 조건
* **점화식**
  * 이전 부분들을 어떻게 덮었는지에 관한 정보는 들어가 있지 않음
  * 입력은 [0, n] 구간의 정수이므로 메모이제이션으로 최적화 가능

> tiling(n) = 2*n 크기의 사각형을 타일로 덮는 방법을 반환

> tiling(n) = tiling(n-1) + tiling(n-2)

```c++
// code 8.16 타일링의 수를 세는 동적계획법 알고리즘
const int MOD = 1000000007;          // MOD는 아마 문제에서 주어진 값
int cache[101];

// 2*width 크기의 사각형을 채우는 방법의 수를 MOD로 나눈 나머지를 반환
int tiling(int width) {
  // base case, width가 1 이하일 때
  if (width <= 1) return 1;

  int& ret = cache[width];
  if (ret != -1) return ret;
  return ret = (tiling(width-2) + tiling(width-1)) % MOD;
}
```

* 이 알고리즘에서 부분 문제의 수는 O(n), 각각의 값을 계산하는 데는 O(1), 전체 시간 복잡도는 O(n)

### 예제 : 삼각형 위의 최대 경로 개수 세기 p243 참고

* 8.4절에서 다뤘던 삼각형 위의 최대 경로 찾기 문제에서 최대 경로의 합을 구했었지만 경로 자체는 구하지 않았음
  * 경로는 유일하지 않기 때문
  * 최대 경로의 수를 구하는 문제라면?
* 이 문제를 해결하기 위해선 두 개의 다른 동적 계획법 문제를 해결해야 함

1. 바탕이 되는 최적화 문제를 풂
2. 각 부분 문제마다 최적해의 개수를 계산하는 동적 계획법 알고리즘을 만듦

* p254 그림 8.8 참고
* 점화식, p255 참고
  * 아래 값이 더 큰 경우
  * 오른쪽 값이 더 큰 경우
  * 아래, 오른쪽 이동한 값이 같은 경우

> count(y,x) = (y,x)에서 시작해 맨 아래줄까지 내려가는 최대 경로의 수

```c++
// code 8.17, 삼각형 위의 최대 경로의 수를 찾는 동적 계획법 알고리즘

int countCache[100][100];
// (y,x)에서 시작해서 맨 아래줄까지 내려가는 경로 중 최대 경로의 개수를 반환
int count(int y, int x) {
  // base case, 맨 아래줄에 도달한 경우
  if (y == n-1) return 1;

  // 메모이제이션
  int& ret = countCache[y][x];
  if (ret != -1) return ret;
  ret = 0;

  if (path2(y+1, x+1) >= path(y+1, x)) ret += count(y+1, x+1);
  if (path2(y+1, x+1) <= path(y+1, x)) ret += count(y+1, x);
  return ret;
}
```

### 예제 : 우물에서 기어오르는 달팽이

* 많은 경우 확률을 계산하는 문제에도 동적 계획법을 사용가능
* 깊이가 n미터인 우물의 맨 밑바닥에 우물 맨 위까지 기어올라가고 싶어하는 달팽이가 있음, 달팽이의 움직임은 그 날의 날씨에 좌우됨
  * 날이 맑으면 하루 2미터 기어올라올 수 있음
  * 비가 내리면 하루 1미터 기어올라올 수 있음
  * 앞으로 m일 간 각 날짜에 비가 올 확률이 정확히 50%라 가정할 때, m일 안에 달팽이가 우물 끝까지 올라갈 확률은?

#### 경우의 수로 확률 계산하기

* 각 날마다 비가 오거나 오지 않거나 둘 중 하나, 가능한 m일 간 날씨의 조합은 2<sup>m</sup>가지
  * 각 조합이 출현할 확률은 모두 같음
  * 날씨 조합 중 합이 n 이상인 조합의 수를 센 뒤, 전체 조합의 수인 2<sup>m</sup>으로 나누면 간단하게 확률을 계산할 수 있음
  * [참고 - 조합](https://mathbang.net/547)

#### 완전 탐색 알고리즘

> climb(C') = 지금까지 만든 날씨 조합 C'를 완성해서 원소의 합이 n이상이 되도록 하는 방법의 수

> climb(C') = climb(C' + [1]) + climb(C' + [2])

* C' + [x]는 배열 C'의 맨 뒤에 x를 덧붙인 결과라 가정
  * C'의 종류가 너무 많아 메모이제이션을 적용할 수 없음
  * **과거에 한 선택에 대한 정보는 최소한으로 줄이는게 좋음**
    * C'에서 쓰는 정보는 지금까지 C'의 길이와 C'의 원소들의 합 뿐으로 다음과 같이 부분 문제 정의를 바꿀 수 있음

> climb(days, climbed) = 지금까지 만든 날씨 조합 C'의 크기가 days이고 그 원소들의 합이 climbed일 때, C'를 완성해서 원소의 합이 n 이상이 되게하는 방법의 수

* == 달팽이가 days일 동안 climbed미터를 기어올라 왔을 때 m전까지 n미터 이상 기어오를 수 있는 경우의 수
* 이 문제는 n*m 개의 부분 문제만 가져 메모이제이션 적용가능

```c++
// code 8.18, 우물을 기어오르는 달팽이 문제를 해결하는 동적 계획법 알고리즘
int n, m;
int cache[MAX_N][2*MAX_N+1];

// 달팽이가 days일 동안 climbed미터를 기어올라 왔다고할 때 m일 전까지 n미터를 기어올라갈 수 있는 경우의 수
int climb(int days, int climbed) {
  // base case, m일이 모두 지난 경우
  if (days == m) return climbed >= n ? 1 : 0;

  // 메모이제이션
  int& ret = cache[days][climbed];
  if (ret != -1) return ret;

  return ret = climb(days + 1, climbed + 1) + climb(days + 1, climbed + 2);
}
```

### 예제 : 장마가 찾아왔다 p258 참고

* 매일 비가 올 확률이 50% -> 75%로 올라가면 날씨의 조합마다 출현할 확률이 달라질 수 있음
  * 이럴 땐 경우의 수를 계산하지 않고 직접 확률을 계산함

> climb2(days, climbed) = 달팽이가 지금까지 days 동안 climbed미터를 기어올라 왔을 때 m일 전까지 n미터 이상 기어올라갈 수 있을 확률

> climb2(days, climbed) = 0.75*climb2(days+1, climbed+1) + 0.25\*climb2(days+1, climbed+2)

#### 경우의 수 계산하기 레시피

1. 모든 답을 직접 만들어 세어보는 완전 탐색 알고리즘을 설계. 이 때 경우의 수를 제대로 세기 위해서는 재귀 호출의 각 단계에서 고르는 각 선택지에 다음과 같은 속성이 성립해야 함. 

> a) 모든 경우는 이 선택지들에 포함됨
> 
> b) 어떤 경우도 두 개 이상의 선택지에 포함되지 않음

2. 최적화 문제를 해결할 때처럼 이전 조각에서 결정한 요소들에 대한 입력을 없애거나 변형해서 줄임. 재귀 함수는 앞으로 남아있는 조각들을 고르는 경우의 수만을 반환해야 함
3. 메모이제이션을 적용

### 8.12 문제 : 비대칭 타일링 p259 참고

* 2\*n 크기의 직사각형을 2*1 크기의 타일로 채우려고 함
  * 타일들은 서로 겹쳐서는 안 되고, 90도로 회전해서 쓸 수 있음
  * **단 이 타일링 방법은 좌우 대칭이어서는 안됨**
  * n이 주어질 때 가능한 비대칭 타일링의 방법의 수를 계산하는 문제
    * 방법의 수는 매우 클 수 있어 1,000,000,007로 나눈 나머지를 출력
  * p259 그림 참고
* 입력
  * 테스트 케이스 수 C(1<=C<=50)
  * 사각형의 너비 n(1<=n<=100)
* 출력
  * 각 테스트 케이스마다 한 줄에 비대칭 타일링 방법의 수를 1,000,000,007로 나눈 나머지를 출력

### 8.13 풀이 : 비대칭 타일링

#### 완전 탐색의 함정

* 지금까지 모든 문제를 '완전 탐색 알고리즘을 만들고(1), 메모이제이션을 적용한다(2)'는 과정으로 풂
  * **기존에 풀어본 변형 문제를 만나면 직접적으로 풀기보다 좀 더 단순화된 문제의 해법을 이용해서 더 쉽게 풀 수 있는 경우가 있음**
* 입력이 배열이거나 문자열인 경우 가능하다면 적절한 변환을 통해 메모이제이션할 수 있도록 함
  * code 8.16에서 모든 타일링 방법을 계산함
  * 비대칭 타일링 방법의 수 = 전체 타일링 방법 수 - 대칭 타일링 수
* 대칭 타일링의 수를 세는 방법
  * n이 짝수인 경우와 홀수인 경우를 나눔(p261 그림 8.9)
    * n이 홀수인 경우 항상 정가운데 세로줄은 세로 타일 하나로 덮여야 함
    * n이 짝수인 경우 대칭 타일링에는 두 종류가 있음
      * 정가운데 세로줄 둘을 가로 타일로 덮고 나머지 절반이 서로 대칭인 경우
      * 절반으로 나뉜 부분들이 서로 대칭인 경우

```c++
// code 8.16 타일링의 수를 세는 동적계획법 알고리즘
const int MOD = 1000000007;
int cache[101];

// 2*width 크기의 사각형을 채우는 방법의 수를 MOD로 나눈 나머지를 반환
int tiling(int width) {
  // base case, width가 1 이하일 때
  if (width <= 1) return 1;

  int& ret = cache[width];
  if (ret != -1) return ret;

  return ret = (tiling(width-2) + tiling(width-1)) % MOD;
}

// code 8.19, 비대칭 타일링 문제를 해결하는 동적 계획법 알고리즘
// 2*width 크기의 사각형을 채우는 비대칭 방법의 수를 반환
int asymmetric(int width) {
  if (width % 2 == 1) // 비대칭인 경우
    return (tiling(width) - tiling(width/2) + MOD) % MOD;
  int ret = tiling(width);
  ret = (ret - tiling(width/2) + MOD) % MOD;
  ret = (ret - tiling(width/2 - 1) + MOD) % MOD;
  return ret;
}
```

* MOD를 더하는 이유는 음수 방지용
  * tiling()의 반환값은 경우의 수, 음수는 반환되지 않지만 뺀 값은 음수일 수 있음
  * tiling()은 MOD로 나눈 나머지를 반환, tiling(width) - tiling(width/2) 값은 음수일 수 있음

#### 직접 비대칭 타일링의 수 세기

* 직접 비대칭 타일링의 수를 세는 간단한 방법은 모든 타일링 방법을 만들어보고 이 중 좌우 대칭이 아닌 것만을 걸러내는 것
  * 메모이제이션을 사용 불가, 타일링 방법이 대칭인지 판단하기 위해서는 과거에 선택한 조각들에 대한 정보를 전부 재귀 호출에 전달해야 하기 때문
  * 메모이제이션에 과거 조각의 정보를 전부 전달하지 않고서도 문제를 해결할 수 있는 방법은 맨 앞에서부터 타일링 방법을 만들어 가는 것이 아니라 양쪽 끝에서부터 동시에 만들어 나가는 것
  * p263 그림 8.10 참고
    * 2*n 사각형을 비대칭 형식으로 덮는 타일링 방법은 모두 이 네가지 패턴 중 하나에 속함
    * a, b - 가운데 남은 회색 부분을 덮는 방법을 재귀 호출로 찾음
      * 양끝이 대칭이기 때문에 내부적으로는 비대칭이어야 함
    * c, d - 가운데 남은 회색 부분을 덮는 방법을 찾음
      * 양끝이 대칭아 아니기 때문에 내부적으로 대칭이어도 상관 없음

```c++
// code 8.20, 직접 비대칭 타일링의 수를 세는 동적 계획법 알고리즘
int cache2[101];

// 2*width 크기의 사각형을 채우는 비대칭 방법의 수를 반환
int asymmetric2(int width) {
  // base case, 너비가 2이하인 경우
  if (width <= 2) return 0;

  // 메모이제이션
  int& ret = cache2[width];
  if (ret != -1) return ret;
  // a, b는 비대칭 방법만 세어야 하므로 asymmetric2로 찾음
  ret = asymmetric2(width-2) % MOD; // a
  ret = (ret + asymmetric(width-4)) % MOD // b

  // c, d는 모든 방법을 찾으면 되므로 tiling 사용
  ret = (ret + tiling(width-3)) % MOD; // c
  ret = (ret + tiling(width-3)) % MOD; // d
  return ret;
}
```

#### 스캐폴딩으로 테스트하기

* [스캐폴딩(Scaffolding)](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week2/younggeun0.md)
* 이 문제는 입력을 생성하기 쉽고, 느리지만 정답임을 확실히 알 수 있는 알고리즘이 존재하기 때문에 스캐폴딩을 통해 테스트하기 적절할 문제
  * 1~20 정도의 값을 입력해서 결과를 확인
