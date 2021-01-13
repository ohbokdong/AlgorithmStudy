# [4차] 두 개 뽑아서 더하기
```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool compare(int a, int b)
{
    if (a < b)
    {
        return true;
    }
    return false;
}

bool CheckValue(vector<int>& answer, int value)
{
    for (size_t i = 0; i < answer.size(); i++)
    {
        if (value == answer[i])
        {
            return false;
        }
    }
    return true;
}

vector<int> solution(vector<int> numbers) {
    vector<int> answer;
    size_t size = numbers.size();

    int num = 0;
    for (size_t i = 0; i < size-1; i++)
    {    
        for (size_t j = i+1; j < size; j++)
        {
            if(i == j)
            { 
                continue;
            }
            else
            {
                num = numbers[i] + numbers[j];
                if (CheckValue(answer, num))
                {
                    answer.push_back(num);
                }
            }
        }
    }
    sort(answer.begin(), answer.end(), compare);
    return answer;
}
```
## 수행시간 

```c++
테스트 1 〉	통과 (0.01ms, 3.96MB)
테스트 2 〉	통과 (0.01ms, 3.96MB)
테스트 3 〉	통과 (0.01ms, 3.95MB)
테스트 4 〉	통과 (0.01ms, 3.96MB)
테스트 5 〉	통과 (0.02ms, 3.84MB)
테스트 6 〉	통과 (0.04ms, 3.95MB)
테스트 7 〉	통과 (0.27ms, 3.97MB)
테스트 8 〉	통과 (0.08ms, 3.97MB)
테스트 9 〉	통과 (0.02ms, 3.95MB)
```

## Set 괜찮아 보이네
```c++
#include <string>
#include <vector>
#include <algorithm>
#include <set>
using namespace std;

vector<int> solution(vector<int> numbers) {
    vector<int> answer;
    set<int> st;
    for(int i = 0;i<numbers.size();++i){
        for(int j = i+1 ; j< numbers.size();++j){
            st.insert(numbers[i] + numbers[j]);
        }
    }
    answer.assign(st.begin(), st.end());
    return answer;
}
```
[Set 이란?](https://hwan-shell.tistory.com/130)
Set의 특징
1. 중복을 없앤다.
2. 순서와 상관없이 정렬이 된다.



[출처](https://programmers.co.kr/learn/courses/30/lessons/68644/solution_groups?language=cpp)박태수 , 이성원 , 백승호 님 풀이

