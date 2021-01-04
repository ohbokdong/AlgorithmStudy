# [3차] 기능 개발
```c++
#include <string>
#include <vector>

using namespace std;

vector<int> solution(vector<int> progresses, vector<int> speeds) {
    vector<int> answer;

    vector<int> progress;

    for (size_t i = 0; i < progresses.size(); i++)
    {
        int work = 100 - progresses[i];
        int day = work / speeds[i];
        if ((work%speeds[i]) != 0)
        {
            day++;
        }
        progress.push_back(day);
    }

    int value = 0;
    int index = 0;

    while (index < progress.size())
    {
        int value = 1;
        int afterwork = progress[index];

        for (size_t j = index + 1; j < progress.size(); j++)
        {
            if (afterwork >= progress[j])
            {
                value++;
            }
            else
            {
                break;
            }
        }
        index = index + value;

        answer.push_back(value);
    }
    
    return answer;
}
```
단순한 접근을 한듯

## 수행시간 

```c++
테스트 1 〉	통과 (0.01ms, 3.97MB)
테스트 2 〉	통과 (0.01ms, 3.97MB)
테스트 3 〉	통과 (0.01ms, 3.96MB)
테스트 4 〉	통과 (0.01ms, 3.94MB)
테스트 5 〉	통과 (0.01ms, 3.97MB)
테스트 6 〉	통과 (0.01ms, 3.97MB)
테스트 7 〉	통과 (0.01ms, 3.71MB)
테스트 8 〉	통과 (0.01ms, 3.97MB)
테스트 9 〉	통과 (0.01ms, 3.73MB)
테스트 10 〉	통과 (0.01ms, 3.92MB)
테스트 11 〉	통과 (0.01ms, 3.95MB)
```

## 오... 이게 개발자인가...
```c++
#include <string>
#include <vector>
#include <iostream>
using namespace std;

vector<int> solution(vector<int> progresses, vector<int> speeds) {
    vector<int> answer;

    int day;
    int max_day = 0;
    for (int i = 0; i < progresses.size(); ++i)
    {
        day = (99 - progresses[i]) / speeds[i] + 1;

        if (answer.empty() || max_day < day)
            answer.push_back(1);
        else
            ++answer.back();

        if (max_day < day)
            max_day = day;
    }

    return answer;
}
```
[출처](https://programmers.co.kr/learn/courses/30/lessons/42586/solution_groups?language=cpp) sterilizedmilk , 안수연 , madistor , dlwns97 , 송민규 님들 풀이

