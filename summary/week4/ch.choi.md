# **6 무식하게 풀기**

## **6.1 도입**

가장 많은 실수는 쉬운 문제를 어렵게 푸는 것임. 

실수를 피하기 위해  먼저 스스로 무식하게 풀수 있는지 물어보셈 

### **무식하게 푼다 (brut-force)**

- 전산학에서 컴퓨터의 빠른 계산 능력을 이용해 가능한 경우의 수를 일일이 나열하면서 답을 찾는 방법을 의미
- 가능한 방법을 전부 만들어 보는 알고리즘들을 가리켜 완전 탐색 이라고 부름

### **완전 탐색**
컴퓨터의 장점을 가장 잘 이용하는 방법

왜냐? 노가다는 컴퓨터가 짱임

예제-열명 줄세우기는 대략 360만 가지가 있음. 사람이 확인 하긴 힘드나 컴퓨터는 1초 안에 계산이 가능함

무식하지만 간편하게 문제 해결 가능

--- 

## **6.2 재귀 호출과 완전 탐색**

### **재귀 호출**

자신이 수행할 작업을 수행하고 나머지를 자기 자신을 호출해 실행하는 함수

쓸데없어 보이지만 재귀 호출은 다양한 알고리즘을 구현하는데 매우 유용하게 사용 할 수 있는 도구

for문과 같이 같은 코드를 반복하는 경우 재귀 호출로 유용하게 사용 가능

재귀 호출 예시(p.147)

```c++
int recursiveSum(int n)
{
    int(n == 1) return 1;
    return n + recursiveSum(n -1); //recursiveSum 자기 자신을 호출함 = 재귀 호출
}
```

모든 재귀 함수는 최소한의 작업에 도달했을 때 답을 곧장 반환하는 조건문을 포함해야 함.

쪼개지지 않는 가장 작은 작업을 기저 사례라고 함

기저 사례를 선택 할 때는 모든 입력이 항상 기저 사례의 답을 이용해 계산 될 수 있도록 신경 써야함.

위 코드 recursiveSum(int n)의 기저 사례가 n==2일 때 입력 1이 오게 될 경우 문제가 발생. 기저 사례에 구멍이 있음

**예시 - 잘못된 기저 사례**
```c++
int recursiveSum(int n = 1)
{
    int(n == 2) return 1; // 기저 사례를 패스
    int temp = n - 1; // temp 0;
    return n + recursiveSum(temp); //recursiveSum 자기 자신을 호출함 = 재귀 호출
}
```

--- 

## **중첩 반복문 대체하기 (p.148)**

n개의 원소중 네 개를 고르는 모든 경우를 출력하는 코드 
```c++
void pick4byfor()
{
    for(int i = 0; i < n; ++i){
        for(int j = i+1; j < n; ++j){
            for(int k = j+1; k < n; ++k){
                for(int l = k+1; l < n; ++l){
                    cout <<i<< j << k << l << endl;
                }   
            }   
        }
    }
}
```
만약 5개를 골라야 한다면?? 

for문 하나 더 추가하면 됨. 

골라야 하는 수가 늘어 날 경우 코드가 길고 복잡해 짐. 좀 빡치죠??

이때 재귀 호출을 할 경우 간결하고 유연한 코드를 작성 할 수 있음.

어떻게?

1. 작업 나누기 - 네 개의 조각으로 나눔
2. 각 조각에서 하나의 원소를 고름 
3. 남은 원소를 고르는 작업을 자기 자신을 호출해 떠넘김

**n개의 원소중 m개를 고르는 모든 경우를 출력하는 코드 (p.149)**
```c++
void pick(int n, vector<int>& picked, int topick)
{
    if(topick == 0){ printpicked(picked); return;} // 기저 사례 원소가 없을때 출력

    int smallest == picked.empty() ? 0 : picked.back() + 1;
    //고른게 비어 있다면 0, 기존에 고른 원소가 있다면 + 1 시켜서 다른 원소 고름
    
    for(int next = smallest; next <n; ++next>)
    {
        picked.push_back(next);
        pick(n, picked, topick - 1);
        picked.pop_back();
    }
}
```

재귀 호출을 사용 하면 완전 탐색을 구현할 때 아주 유용한 도구 

--- 

## **보글 게임 (p.150)**

5*5 크기의 알파벳 격자를 가지고 하는 게임 게임의 목적은 상하좌우/대각선으로 인접한 칸들의 글자들을 이어서 다너를 찾아내는 것

이 문제를 풀 때 가장 까다로운 점은 다음 글자가 될 수 있는 칸이 여러 개 있을 때 이 중 어느 글자를 선택해야 할지 미리 알 수 없다는 것
간단한 방법은 완전 탐색을 이용해 단어를 찾아낼 때까지 모든 인접한 칸을 하나씩 시도해 보는 것

### **문제의 분할**

각 글자를 하나의 조각으로 만듦. 함수 호출시에 단어의 시작 위치를 정해 주기 때문에 첫 번째 글자에 해당하는 조각을 간단하게 해결.

시작 위치(y,x)와 인접한 여덟칸에 대한 실행을 모두 시도하여 답을 찾음

### **기저 사례의 선택**

탐색 없이 답을 낼수 있는 경우
1. 위치(y,x)에 있는 글자가 원하는 단어의 첫 글자가 아닌 경우 항상 실패
2. (1번 경우에 해당되지 않을 경우)원하는 단어가 한 글자인 경우 항상 성공
3. 입력 범위 밖이면 실패

입력이 잘못되거나 범위에서 벗어난 경우도 기저 사례로 택해서 맨 처음에 처리
재귀 함수는 항상 한군데 이상에서 호출되기 때문에 반복적인 코드를 제거하는데 큰 도움이 됨.

### **구현**

```c++
const int dx[8] = {-1,-1,-1, 1,1,1, 0,0}
const int dy[8] = {-1,-0,1, -1,0,1, -1,1}

// 처음 호출 시 word = "Test" 였다면 
bool hasWord(int y, int x, const string& word)
{
    if(!inRange(y,x)) return false; //기저 사례 3
    if(board[y][x] != word[0]) return false; // 기저 사례1
    if(word.size() == 1) return true; //기저 사례 2

    for(int direction = 0; direction < 8; ++diection>)
    {
        int nexty = y + dy[direction], nextx = x + dx[direction]; // 방향 설정
        // word.substr(1)을 통해서 word 는 "est"가 넘어감
        // 그 다음은 "st"가 됨. 한글자씩 클리어
        if(hasWord(nexty, nextx, word.substr(1)))
        {
            return true;
        }
    }
    return false;
}
```
---
## **시간 복잡도 분석**

완전 탐색은 가능한 답 후보들을 모두 만들어 보기 때문에 시간 복잡도를 계산하기 위해서는 가능한 후보의 수를 전부 세어 보기만 하면 됨

후보의 수를 계산하는건 매우 단순 하지만 답을 하나라도 찾으면 바로 종료 하기 때문에 분석이 까다로움

최악의 경우는 답이 존재하지 않는 경우가 많음. 존재하지 않다면 모든 경우를 다 해봐야함.

알고리즘 시간복잡도가 증가하여 완전탐색으로 해결하기 어려운 경우 동적 계획법을 통해 해결 할 수 있음.

## **완전 탐색 레시피**

어떤 식으로 문제를 접근해야할지에 대한 대략적인 지침 

1. 완전 탐색은 존재하는 모든 답을 하나씩 검사하므로, 걸리는 시간은 가능한 수에 비례함.
    - 최대 크기의 입력을 보고 답의 개수를 계산하고 제한시간 안에 생성 가능한지 가늠, 안될 경우 다른 장에서 소개하는 설계 패러다임을 적용

2. 가능한 모든 답의 후보를 만드는 과정을 여러 개의 선택으로 나눔. 답의 후보를 만드는 과정의 한 조각이 됨
    - 보글 게임에서 시작 글자 주변 8글자를 확인한다 가 여러 개의 선택

3. 하나의 조각을 선택해 답의 일부를 만들고 나머지 답을 을 재귀 호출을 통해 완성
    -  보글 게임에서 한 글자씩 확인하고 다음 글자를 확인

4. 조각이 하나밖에 남지않은 경우 또는 남지않은 경우 답을 생성 했음으로 기저 사례로 선택해 처리

---    

## **이론적 배경 : 재귀 호출과 부분문제** 

재귀 호출의 중요한 개념중 하나

문제와 부분문제의 정의 - 동적 계획법이나 분할 정복과 같은 디자인 패러다임을 설명하는데 사용

- 문제1: 주어진 자연수 수열을 정렬하라
- 문제2: {16,7,9,1,31}을 정렬하라

위 문제1, 문제2는 비슷한 문제 같지만 두 정의 사이에는 큰 차이가 있음

**문제1은 특정한 입력을 지정 x , 문제2는 특정한 입력을 지정**

재귀 호출을 논의할 때 문제란 항상 수행해야 할 작업과 그 작업을 적용할 자료의 조합을 의미

예를 들어 {1,2,3}을 정렬시키는 문제와 {3,2,1}을 정렬 시키는 문제는 서로 다른 문제 따라서 문제2가 문제의 정의라고 할 수 있음
- 문제1의 정의로 보면 같으나 각각 오름차, 내림차 정렬이 되어 있어 서로 자료의 조합이 틀리기 때문에 서로 다름

재귀 호출의 과정은 원래 문제에서 한 조각을 때어내 다른 문제를 푼 결과임으로 문제의 일부라고 할수 있으며 문제들을 원래 문제의 부분 문제라고 함

---

## **6.3 문제: 소풍 (p.155)**

서로 친구인 아이들 끼리 짝 지어 주는 문제

## **6.4 풀이: 소풍 (p.157)**

### **완전 탐색**

가능한 조합의 수를 계산하는 문제를 푸는 가장 간단한 방법은 완전 탐색을 이용하여 조합을 모두 만들어 보는것

### **중복으로 세는 문제 (p. 157)**

```c++
int n;
bool areFriends[10][10];

int countPairings(bool taken[10])
{
    bool finished = true;

    for(int i = 0; i< n; i++>)
    {
        if(!taken[i]) finished = false;
    }
    if(finished) return 1;
    int ret = 0;
    for(int i = 0; i<n; ++i)
    {
        for(int j = 0; j<n; ++j)
        {
            if(!taken[i] && !taken[j] && areFriends[i][j])
            {
                taken[i] = taken[j] = true;
                ret += countPairings(taken);
                taken[i] = taken[j] = false;
            }
        }
    }
    return ret;
}
```

위 코드는 두가지 문제점이 있음
1. 같은 쌍을 두번 짝지어 줌
2. 다른 순서로 학생들을 짝지어 주는 것을 서로 다른 경우로 세고 있

실질적으로 같은 답을 중복으로 세는 상황은 경우의 수를 다룰 때 광장히 흔하게 마주침

해결 하기위해 선택할 수 있는 좋은 방법은가장 먼저 오는 답을 하나만을 세는 것이 있음.

학생들 중 가장 번호가 빠른 학생 부터 짝을 찾아 주면 두가지 문제가 해결됨

### **중복으로 세는 문제 해결 버전 (p. 158)**
```c++
int n;
bool areFriends[10][10];

int countPairings(bool taken[10])
{
    int firstfree = -1;

    //번호가 빠른 순서를 찾음
    for(int i = 0; i< n; i++>)
    {
        if(!taken[i]) 
            firstfree = i;
            break;
    }

    if(firstfree == -1) return 1;
    int ret = 0;
    // 번호가 빠른 친구부터 찾아줌
    for(int pairwith = firstfree + 1; pairwith < n; ++pairwith)
    {
        if(!taken[pairwith] && areFriends[firstfree][pairwith])
        {   
            taken[firstfree] = taken[pairwith] = true;
            ret += countPairings(taken);
            taken[firstfree] = taken[pairwith] = false;
        }
    }
    return ret;
}
```

### **답의 수의 상한**
모든 답을 생성해 가며 답의 수를 세는 재귀 호출 알고리즘은 답의 수에 정비례 하는 시간이 걸림.

프로그램을 짜기 전에 답의 수가 얼마나 될지 예측 하고 얼마나 오래 걸릴지 확인 해야함


---
## **6.5 문제: 게임판 덮기 (p.159)**

H*W 크기의 게임판이 있고 L 자 모양의 블록을 회전하며 덮을 수 있는지, 몇개로 덮을 수 있는 지 계산하는 문제

## **6.5 풀이: 게임판 덮기 (p.161)**

경우의 수를 세는 것이니 만큼 완전 탐색을 이용해 해결 할 수 있음.

흰칸 수가 3의 배수가 아닐 경우 무조건 답이 없음.
흰칸 수 나누기 3을 통해 N조각을 구하고 재귀 호출을 통해 해결

### **중복을 세는 문제**
블록을 놓는 순서는 중요하지 않음. 블록을 놓는 순서에 따라 여러번 세는 문제가 발생, 특정 순서대로 답을 생성 하도록 강제하여 중복으로 세는 수를 해결

순서를 고정하여 블록을 채우는 방법을 4가지로 한정 할 수 있음.

### **답의 수의 상한**

블록을 채우는 방법은 4가지가 있고 최대 16개의 블록이 들어감으로 최대 상한은 4의16승
시간내에 모두 생성이 불가능 할거 같지만 선택 할 수 있는 블록 배치가 제한적임을 알 수 있음 

### **구현**

하 너무 긴데

---

## **6.7 최적화 문제 (p.165)**

문제의 답이 하나가 아니라 여러 개이고 어떤 기준에 따라 가장 좋은 답을 찾아 내는 문제들을 통칭해 최적화 문제라고 부름

컴퓨터가 하는 많은 작업들이 최적화 문제로 표현됨.


## **6.10 많이 등장하는 완전 탐색 유형 (p.172)**

### **모든 순열 만들기**
서로 다른 n개의 원소를 일렬로 줄 세운것을 순열

[순열](https://minusi.tistory.com/entry/%EC%88%9C%EC%97%B4-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Permutation-Algorithm)

### **모든 조합 만들기**
서로 다른 n개의 원소중 R개를 순서 없이 골라내는 것을 조합 이라고 함

피자에 올리는 토핑을 생각하면 됨, 재귀 호출로 구현하기 좋은 예

### **모든 2의n승 가지 경우의 수 만들기**
n개의 질문에 대한 답이 예/아니오 중 하나 일때 모든 조합의 수는 2의n승임 각 조합을 하나의 n비트 정수로 표현 한다고 하면 재귀 호출을 사용할 것 없이 1차원 for문으로 조합들을 간단하게 시도 할 수 있음.


## **완전 탐색의 종류**

- 브루트 포스 : for문을 이용하여 처음부터 끝까지 탐색하는 방법
- 비트 마스크 : 이진수 표현을 자료구조로 쓰는 기법 (AND, OR, XOR, SHIFT, NOT)
- 백트래킹 : 분할정복을 이용한 기법, 재귀함수를 이용, 해를 찾아가는 도중에 해가 될 것 같지 않은 경로가 있다면 더 이상 가지 않고 되돌아간다.
- 재귀함수 : 함수 내에서 함수 자기 자신을 계속해서 호출하는 것
- 순열 : 서로 다른 n개의 원소에서 r개의 중복을 허용하지 않고 순서대로 늘어 놓은 수
- BFS(너비 우선 탐색)
- DFS(깊이 우선 탐색)


















