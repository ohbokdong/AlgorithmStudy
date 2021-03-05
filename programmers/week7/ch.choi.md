# [8차] 체육복
```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(int n, vector<int> lost, vector<int> reserve) {
    int answer = n;

    int helpnumber = lost.size();

    for (auto i = reserve.begin(); i != reserve.end();)
    {
        auto f = find(lost.begin(), lost.end(), *i);
        if (f != lost.end())
        {
            helpnumber--;
            lost.erase(f);
            i = reserve.erase(i);
        }
        else
        {
            i++;
        }
    }

    for (auto i = reserve.begin(); i != reserve.end(); i++)
    {
        int value = *i;
        vector<int>::iterator f = lost.end();

        for (size_t i = 0; i < 2; i++)
        {
            if (i == 0)
            {
                f = find(lost.begin(), lost.end(), value - 1);
            }
            else
            {
                f = find(lost.begin(), lost.end(), value + 1);
            }

            if (f != lost.end())
            {
                helpnumber--;
                lost.erase(f);
                break;
            }
        }
    }
    answer = answer - helpnumber;
    return answer;
}
```
## 수행시간 

```c++

테스트 1 〉	통과 (0.01ms, 3.94MB)
테스트 2 〉	통과 (0.01ms, 3.96MB)
테스트 3 〉	통과 (0.01ms, 3.97MB)
테스트 4 〉	통과 (0.01ms, 3.77MB)
테스트 5 〉	통과 (0.01ms, 3.96MB)
테스트 6 〉	통과 (0.01ms, 3.98MB)
테스트 7 〉	통과 (0.01ms, 3.96MB)
테스트 8 〉	통과 (0.01ms, 3.98MB)
테스트 9 〉	통과 (0.01ms, 3.96MB)
테스트 10 〉	통과 (0.01ms, 3.96MB)
테스트 11 〉	통과 (0.01ms, 3.98MB)
테스트 12 〉	통과 (0.01ms, 3.97MB)
```

## 오호... 난 멍청인가
```c++
int student[31];
int solution2(int n, vector<int> lost, vector<int> reserve) {
    int answer = 0;
    for (int i : reserve) student[i] += 1; // 가지고 있으면 일단 1로 초기화
    for (int i : lost) student[i] += -1; //없다면 다시 -1 
    //student[31] 에서 -1 이면 잃어버린 사람임 

    for (int i = 1; i <= n; i++) {
        if (student[i] == -1) { // 잃어버린 사람이라면
            if (student[i - 1] == 1) //앞에 사람의 상태 확인 
                student[i - 1] = student[i] = 0; 
            else if (student[i + 1] == 1) // 뒤에 사람 상태 확인
                student[i] = student[i + 1] = 0;
        }
    }
    for (int i = 1; i <= n; i++) // 최종적으로 몇명인지 확인진행
        if (student[i] != -1) answer++;

    return answer;
}
```

[출처](https://programmers.co.kr/learn/courses/30/lessons/42862/solution_groups?language=cpp) 이용철 , JeonSangyeon2304 , 배성희 , PSB , 엄인섭 외 10 명

