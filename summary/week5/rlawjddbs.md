# 7. 분할 정복(Divide & Conquer)
- 각개 격파라고 설명되는 가장 유명한 알고리즘 디자인 패러다임
- 본 패러다임을 차용한 알고리즘들은 한 문제를 둘 이상의 부분 문제로 나눠 각 부분 문제를 재귀호출로 계산한 뒤 부분 문제의 답으로 전체 문제의 답을 계산함
## 일반적인 재귀 호출과 분할 정복의 차이
![일반 재귀호출과 분할 정복의 차이](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week5/images/recursive_divide_diffPoint.png)
- 재귀 호출은 문제를 `한 조각`과 `나머지 전체 조각`으로 나눔
- 분할 정복은 문제를 `절반`씩 나눔
   
## 분할 정복을 사용하는 알고리즘들이 대개 갖춘 구성 요소
1. `divide`: 문제를 더 작은 문제로 분할하는 과정
2. `merge`: 각 문제에 대해 구한 답을 원래 문제에 대한 답으로 병합하는 과정
3. `base case`: 더 이상 답을 분할하지 않고 곧장 풀 수 있는 매우 작은 문제
   
## 분할 정복을 적용할 수 있는 문제들의 특성
1. 문제를 둘 이상의 부분 문제로 나누는 자연스러운 방법이 있어야 함
2. 부분 문제의 답을 조합해 원래 문제의 답을 계산하는 효율적인 방법이 있어야 함

> ## 분할 정복의 장점
> - 많은 경우 같은 작업을 더 빠르게 처리해 줌

## 분할 정복 예제
### 수열의 빠른 합과 행렬의 빠른 제곱
- 6.2절의 1+2+…+n의 합을 재귀 호출을 이용해 계산하는 `recursiveSum()` 함수처럼, 분할 정복을 이용하여 계산하는 `fastSum()` 함수 작성
- `1`부터 `n`까지의 합을 `n`개의 조각으로 나눈 뒤, 이들을 반으로 뚝 잘라 `n/2`개의 조각들로 만들어진 부분 문제 두 개를 만듬 (편의상 n은 짝수로 가정)
   
![분할 정복 계산식1](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week5/images/fastSumFunction1.png)   
   
- 첫 번째 부분 문제는 fastSum(n/2)로 나타낼 수 있지만, 두 번째 부분 문제는 그렇지 않음
- 재귀적으로 풀기 위해서는 각 부분 문제를 '1부터 n까지의 합' 꼴로 표현할 수 있어야 함
- 두 번째 조각은 'a부터 b까지의 합'형태를 가지고 있기 때문에 적절하지 않음
- 따라서 두 번째 부분 문제를 fastSum(x)를 포함하는 형태로 바꿔 써야 함   
![분할 정복 계산식2](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week5/images/fastSumFunction2.png)   
   
- 공통된 항 `n/2`를 빼내면 놀랍게도 `fastSum(n/2)`이 나타나므로 다음과 같이 쓸 수 있음

![분할 정복 계산식3](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week5/images/fastSumFunction2.png)   

```C++
// 코드 7.1 - 1부터 n까지의 합을 구하는 분할 정복 알고리즘

// 필수 조건: n은 자연수
// 1 + 2 + … + n 을 반환
int fastSum(int n) {
    // 기저 사례
    if(n == 1) return 1;
    if(n % 2 == 1) return fastSum(n-1) + n;
    return 2 * fastSum(n/2) + (n/2)*(n/2);
}
```
- 위 분할 공식은 `짝수인 n`에 대해서만 동작하므로, **홀수인 입력이 주어질 때**는 `짝수인 n-1까지의 합`을 `재귀호출`로 계산하고 `n`을 더해 답을 구함

### 시간 복잡도 분석
두 함수 모두 내부에 반복문이 없기 때문에, `fastSum()`과 `recursiveSum()`이 종료하는 데 걸리는 시간은 **순전히 함수가 호출되는 횟수에 비례**함
```
recursiveSum() - n번의 함수 호출이 필요
fastSum() - 호출될 때마다 최소한 두번에 한 번 꼴로 n이 절반으로 줄어드므로 fastSum()의 호출 횟수가 훨씬 적으리란 것을 쉽게 예상 가능
```
   
   
***fastSum***(1011<sub>2</sub>) = ***fastSum***(1010<sub>2</sub>) + 11   
***fastSum***(1010<sub>2</sub>) = ***fastSum***(101<sub>2</sub>) * 2 + 25   
***fastSum***(101<sub>2</sub>) = ***fastSum***(100<sub>2</sub>) + 5   
***fastSum***(100<sub>2</sub>) = ***fastSum***(10<sub>2</sub>) * 2 + 4   
***fastSum***(10<sub>2</sub>) = ***fastSum***(1<sub>2</sub>) * 2 + 1   
***fastSum***(1<sub>2</sub>) = 1   
   
- 위는 fastSum(11)을 실행할 때 재귀 호출의 입력이 어떻게 변화하는지를 이진수 표현으로 보여준 것
- n의 이진수 표현의 마지막 자리가 1이면 0으로 바뀌고, 마지막 자리가 0이면 끝자리가 없어진다는 것을 알 수 있음
- `fastSum()`의 총 호출 수: `n의 이진수 표현의 자리수` + `첫자리를 제외하고 나타나는 1의 개수`
- 두 값의 상한은 모두 log<sub>n</sub>이므로 이 알고리즘의 실행 시간은 O(log<sub>n</sub>)   
   
   
-----

## 행렬의 거듭제곱
- n x n = A 일 때 A의 거듭 제곱 A<sup>m</sup>은 A를 연속해서 m번 곱한 것
- m이 매우 클 때 A<sup>m</sup>을 구하는 것은 꽤나 시간이 오래 걸리는 작업이 됨
    - 행렬의 곱셈에는 O(n<sup>3</sup>)의 시간이 들기 때문에 곧이곧대로 m-1번의 곱셈을 통해 A<sup>m</sup>을 구하려면 모두 O(n<sup>3</sup>m)번의 연산이 필요함
- 분할 정복을 이용하면 이러한 연산의 결과를 빠르게 구할 수 있음
   
A<sup>m</sup> = A<sup>m/2</sup> x A<sup>m/2</sup>
   
```C++
// 코드 7.2 - 행렬의 거듭제곱을 구하는 분할 정복 알고리즘
// 정방행렬을 표현하는 squareMatrix 클래스가 있다고 가정
class SquareMatrix;
// n*n 크기의 항등 행렬(identity matrix)을 반환하는 함수
SquareMatrix identity(int n);
// A^m을 반환
SquareMatrix pow(const SquareMatrix& A, int m) {
    // 기저 사례: A^0 = I
    if(m == 0) return identity(A.size());
    if(m % 2 > 0) return pow(A, m-1) * A;
    SquareMatrix half = pow(A, m / 2); // 내부 함수 초기화 인가?
    return half * half;
}
```

### 나누어 떨어지지 않을 때의 분할과 시간 복잡도
- m이 홀수일 때, A<sup>m</sup> = A∙A<sup>m-1</sup>로 나누지 않고 

-----

## 병합 정렬과 퀵 정렬
- 주어진 수열을 크기 순서대로 정렬하는 알고리즘
- 두 알고리즘은 모두 **분할 정복 패러다임을 기반**으로 해서 만들어짐

> ### 병합 정렬 알고리즘
> - 수열을 가운데에서 쪼개 비슷한 크기의 수열 두 개로 만든 뒤 이들을 재귀 호출을 이용해 각각 정렬
> - 그 후 정렬된 배열을 하나로 합쳐 정렬된 전체 수열을 얻음

> ### 퀵 정렬
> - 병합 과정이 필요 없도록 한쪽의 배열에 포함된 수가 다른 쪽 배열의 수보다 항상 작도록 배열을 분할함
> - 이를 위해 퀵 정렬은 `파티션(partition)`이라고 부르는 단계를 도입함
>   - 배열에 있는 수 중 임의의 `기준수(pivot)`를 지정한 후 기준보다 작거나 같은 숫자를 왼쪽, 더 큰 숫자를 오른쪽으로 보내는 과정
   
![병합 정렬과 퀵 정렬](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week5/images/merge_sort_quick_sort.png)   
- `a`는 병합 정렬의 동작 과정을 보여줌
    - 각 수열의 크기가 1이 될 때까지 절반씩 쪼개 나간 뒤, 정렬된 부분 배열들을 합쳐 나감
    - 병합 정렬의 분할 방식은 아주 단순하고 효율적(주어진 배열을 가운데서 절반으로 그냥 나눔)
    - 이 과정을 상수 시간인 O(1)만에 수행할 수 있으나, 나눠서 정렬한 배열들을 하나의 배열로 합치는 별도의 병합 과정을 실행하는데 O(n)의 시간이 걸림
   
- `b`에 보여진 퀵 정렬은 각 부분 수열의 맨 처음에 있는 수를 기준으로 삼고 이들보다 작은 수를 왼쪽으로, 큰 것을 오른쪽으로 가게끔 문제를 분해함
- `동그라미`로 표시된 수들은 이전 수를 분할하는 데 사용된 기준을 표현
- 이 분할은 O(n)의 시간이 걸리는 복잡한 작업인데다, 우리가 어떤 기준을 선택하느냐에 따라서 (27, 9, 3, 10)이 (9, 3, 10)과 (27)로 나누어지는 것처럼 비효율적인 분할을 불러올 여지가 있음
    - 하지만 그 덕분에 각 부분 배열이 이미 정렬한 상태가 되어 별도의 병합 작업이 필요없다는 장점이 있음
   
   
위 두 알고리즘은 같은 아이디어로 정렬을 수행하지만 시간이 많이 걸리는 작업을 분할 단계에서 하느냐, 병합 단계에서 하느냐가 다름.   
이렇게 같은 문제를 해결하는 알고리즘이라도 어떤식으로 분할 하느냐에 따라 다른 알고리즘이 될 수 있음. 또한 이들 모두 분할 정복 패러다임을 사용한 알고리즘이라고 할 수 있음.   
   
### 시간 복잡도 분석
![병합 정렬 단계](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week5/images/lvl_of_merge_sort.png)   
> #### 병합 정렬
> - 단계의 수에 n을 곱하면 병합 정렬에 필요한 전체 시간을 얻을 수 있음
> - 한 단계 내에서 모든 병합에 필요한 총 시간: O(n) - 항상 일정
> - 필요한 단계의 수: O(lgn) - 문제의 크기는 항상 거의 절반으로 나누어 지기 때문
> - 병합 정렬의 시간 복잡도: O(nlgn)

> #### 퀵 정렬
> - 퀵 정렬의 경우 대부분의 시간을 차지하는 것은 주어진 문제를 두 개의 부분 문제로 나누는 파티션 과정
> - 파티션에는 주어진 수열의 길이에 비례하는 시간이 걸리므로, 사실 병합 정렬에서의 병합 과정과 다를 것이 없음
> - 퀵 정렬의 시간 복잡도를 분석하기 까다로운 것은 결과적으로 분할된 두 부분 문제가 비슷한 크기로 나눠진다는 보장을 할 수 없기 때문
>   - 기준으로 택한 원소가 최소 원소나 최대 원소인 경우 부분 문제의 크기가 하나씩만 줄어들 수도 있기 때문
>   - 최악의 경우 퀵 정렬의 시간 복잡도는 O(n<sup>2</sup>)
>       - 평균적으로 부분 문제가 절반에 가깝게 나눠질 때 퀵 정렬의 시간 복잡도는 병합 정렬과 같은 O(nlgn)
   
-----
   
### 예제: 카라츠바의 빠른 곱셈 알고리즘
- 수백 자리 ~ 수만 자리 이상의 큰 숫자들을 다룰 때 주로 사용
- 이렇게 큰 숫자들은 배열을 이용해 저장해야 함
- 두 자연수의 십진수 표기가 배열에 주어진다고 할 때, 이 둘을 곱한 결과를 계산하는 가장 기본적인 방법은 초등학교 산수 시간에 배운 방법을 그대로 사용하는 것   
   
![두 정수의 곱을 구하는 초등학교 알고리즘](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week5/images/multiply.png)   
   
```C++
// 코드 7.3 - 두 큰 수를 곱하는 O(n2)시간 알고리즘
// num[]의 자릿수 올림을 처리
void normalize(vector<int>& num) {
    num.push_back(0);
    // 자릿수 올림을 처리한다.
    for(int i = 0; i+1 < num.size(); ++i) { // 배열 자리수보다 
        if(num[i] < 0) { // 음수
            int borrow = (abs(num[i]) + 9) / 10;
            num[i+1] -= borrow;
            num[i] += borrow * 10;
        } else { // 양수
            num[i+1] += num[i] / 10; // 다음 원소에 두 자리수 부터 더하기
            num[i] %= 10; // 현재 원소는 한 자리수만 남겨둠
        }
    }
    while(num.size() > 1 && num.back() == 0) num.pop_back(); // 왜 팝하는거지?
}
// 두 긴 자연수의 곱을 반환
// 각 배열에는 각 수의 자릿수가 1의 자리에서부터 시작해 저장되어 있다.
// 예: multiply({3, 2, 1}, {6, 5, 4}) = 123 * 456 = 56088 = {8, 8, 0, 6, 5}
vector<int> multiply(const vector<int>& a, const vector<int>& b) {
    vector<int> c(a.size() + b.size() + 1, 0); // 0값으로 초기화된 a.size() + b.size() + 1개의 원소를 가짐
    for(int i = 0; i < a.size(); ++i)
        for(int j = 0; j < b.size(); ++j)
            c[i+j] += a[i] * b[j];
    normalize(c);
    return c;
}
```
- 이 알고리즘의 시간복잡도는 두 정수의 길이가 모두 n이라고 할 때 O(n<sup>2</sup>)
    - n번 실행되는 for문이 두번 겹쳐 있기 때문
- 본 알고리즘을 카라츠바 빠른 곱셈 알고리즘으로 더욱 빠른 연산이 가능함

### 카라츠바 알고리즘으로 풀기
- 카라츠바의 빠른 곱셈 알고리즘은 두 수를 각각 절반으로 쪼갬
- a와 b가 각각 256자리 수라면 a<sub>1</sub>과 b<sub>1</sub>은 첫 128자리, a<sub>0</sub>과 b<sub>0</sub>은 그 다음 128자리를 저장하도록 하는 것
   
   
a = a<sub>1</sub> x 10<sup>128</sup> + a<sub>0</sub>   
b = b<sub>1</sub> x 10<sup>128</sup> + b<sub>0</sub>   
   
- 카라츠바는 이때 a X b를 네 개의 조각을 이용해 표현하는 방법을 살펴보았음
   
   
a X b = (a<sub>1</sub> x 10<sup>128</sup> + a<sub>0</sub>) x (b<sub>1</sub> x 10<sup>128</sup> + b<sub>0</sub>)   
      = a<sub>1</sub> x b<sub>1</sub> x 10<sup>256</sup> + (a<sub>1</sub> x b<sub>0</sub> + a<sub>0</sub> x b<sub>1</sub>) x 10<sup>128</sup> + a<sub>0</sub> x b<sub>0</sub>   
   
- 이 방법은 큰 정수 두 개를 한 번 곱하는 대신, 절반 크기로 나눈 작은 조각을 네 번 곱함
    - 10의 거듭제곱과 곱하는 것은 그냥 뒤에 0을 붙이는 시프트 연산으로 구현하면 되니 곱셈으로 치지 않음
- 이대로도 각각을 재귀 호출해서 해결하면 분할 정복 알고리즘이라고 할 수 있지만 이 방법의 전체 수행 시간이 O(n<sup>2</sup>)이므로 개선 필요
   
카라츠바는 다음과 같이 a x b를 표현했을 때 네 번 대신 세 번의 곱셈으로만 이 값을 계산할 수 있다는 점을 발견함   
   
![카라츠바1](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week5/images/karatsuba1.png)   
   
조각들의 곱을 각각 위와 같이 z<sub>2</sub>, z<sub>1</sub>, z<sub>0</sub>라고 씀. 우선 z<sub>0</sub>와 z<sub>2</sub>를 각각 한 번의 곱셈으로 구하고 다음 식을 이용함   
   
![카라츠바1](https://github.com/ohbokdong/AlgorithmStudy/blob/main/summary/week5/images/karatsuba2.png)   
   
따라서 위 식의 결과에서 z<sub>0</sub>과 z<sub>2</sub>를 빼서 z<sub>1</sub>을 구할 수 있음.
```
z2 = a1 * b1;
z0 = a0 * b0;
z1 = (a0 + a1) * (b0 + b1) - z0 - z2;
```
- 이 과정은 곱셈을 세 번밖에 쓰지 않음

```C++
// 코드 7.4 - 카라츠바의 빠른 정수 곱셈 알고리즘
// a += b * (10^k); 를 구현
void addTo(vector<int>& a, const vector<int>& b, int k);
// a -= b; 를 구현한다. a >= b를 가정한다.
void subFrom(vector<int>& a, const vector<int>& b);
// 두 긴 정수의 곱을 반환
vector<int> karatsuba(const vector<int>& a, const vector<int>& b) {
    int an = a.size(), bn = b.size();
    // a가 b보다 짧을 경우 둘을 바꾼다.
    if(an < bn) return karatsuba(b, a);
    // 기저 사례: a나 b가 비어 있는 경우
    if(an == 0 || bn == 0) return vector<int>();
    // 기저 사례: a가 비교적 짧은 경우 O(n^2) 곱셈으로 변경한다.
    if(an <= 50) return multiply(a, b);
    int half = an / 2;
    // a와 b를 밑에서 half 자리와 나머지로 분리한다.
    vector<int> a0(a.begin(), a.begin() + half);
    vector<int> a1(a.begin() + half, a.end());
    vector<int> b0(b.begin(), b.begin() + min<int>(b.size(), half));
    vector<int> b1(b.begin() + min<int>(b.size(), half), b.end());
    // z2 = a1 * b1
    vector<int> z2 = karatsuba(a1, b1);
    // z0 = a0 * b0
    vector<int> z0 = karatsuba(a0, b0);
    // a0 = a0 + a1; b0 = b0 + b1
    addTo(a0, a1, 0); addTo(b0, b1, 0);
    // z1 = (a0 * b0) - z0 - z2;
    vector<int> z1 = karatsuba(a0, b0);
    subFrom(z1, z0);
    subFrom(z1, z2);
    // ret = z0 + z1 * 10^half + z2 * 10^(half*2)
    vector<int> ret;
    addTo(ret, z0, 0);
    addTo(ret, z1, half);
    addTo(ret, z2, half + half);
    return ret;
}
```
   
### 시간 복잡도 분석
- 병합 단계의 수행 시간: addTo()와 subFrom()의 수행 시간에 지배됨
- 기저 사례의 처리 시간: multiply()의 수행 시간에 지배됨
- 최종 시간 복잡도: O(n<sup>lg3</sup>)

-----

## 7.2 문제: 쿼드 트리 뒤집기 (p.189)
대량의 좌표 데이터를 메모리 안에 압축해 저장하는 문제

## 7.3 풀이: 쿼드 트리 뒤집기
- 이 문제를 풀 수 있는 가장 무식한 방법은 주어진 그림의 쿼드 트리 압축을 풀어서 실제 이미지를 얻고 상하 반전한 뒤 다시 쿼드 트리 압축하는 것
- 문제에 주어진 원본 그림의 크기 제한을 보면 이 방법을 곧이곧대로 구현 할 수 없다는 것을 알 수 있음

### 쿼드 트리 압축 풀기
```C++
// 코드 7.5 - 쿼드 트리 압축을 해제하는 재귀 호출 알고리즘
char decompressed [MAX_SIZE][MAX_SIZE];
void decompress(string::iterator& it, int y, int x, int size) {
    // 한 글자를 검사할 때마다 반복자를 한 칸 앞으로 옮긴다.
    char head = *(it++);
    // 기저 사례: 첫 글자가 b 또는 w인 경우
    if(head == 'b' || head == 'w') {
        for(int dy = 0; dy < size; ++dy)
            for(int dx = 0; dx < size; ++dx)
                decompressed[y+dy][x+dx] = head;
    } else {
        // 네 부분을 각각 순서대로 압축 해제한다.
        int half = size/2;
        decompress(it, y, x, half);
        decompress(it, y, x+half, half);
        decompress(it, y+half, x, half);
        decompress(it, y+half, x+half, half);
    }
}
```
   
### 압축 다 풀지 않고 뒤집기
```C++
// 코드 7.6 - 쿼드 트리 뒤집기 문제를 해결하는 분할 정복 알고리즘
string reverse(string::iterator& it) {
    char head = *it;
    ++it;
    if(head == 'b' || head == 'w')
        return string(1, head);
    string upperLeft = reverse(it);
    string upperRight = reverse(it);
    stringlowerLeft = reverse(it);
    string lowerRight = reverse(it);
    // 각각 위와 아래 조각들의 위치를 바꿈
    return string("x") + lowerLeft + lowerRight + upperLeft + upperRight;
}
```
   
### 시간 복잡도 분석
- reverse() 함수는 한 번 호출될 때마다 주어진 문자열의 한 글자씩을 사용
- 따라서 함수가 호출되는 횟수는 문자열의 길이에 비례하므로 O(n)이 됨
    - 각 문자열을 합치는 데 O(n)의 시간이 든다 해도 시간 안에 충분히 수행 가능