# [3차] 예산
```c++
#include <iostream>
#include <stdio.h>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool compare(int a, int b )
{
    if(a < b)
    {
        return true;
    }
    else
    {
        return false;
    }
}

int solution(vector<int> d, int budget) {
    int answer = 0;
    
    sort(d.begin(), d.end(), compare);
    int total = 0;
    
    for(int i = 0; i<d.size(); i++)
    {
        total = total+d[i];
        
        if(total > budget)
        {
            break;
        }
        answer++;
    }
    
    return answer;
}
```
## 수행시간 

```c++
테스트 1 〉	통과 (0.02ms, 3.88MB)
테스트 2 〉	통과 (0.01ms, 3.96MB)
테스트 3 〉	통과 (0.01ms, 3.94MB)
테스트 4 〉	통과 (0.01ms, 3.88MB)
테스트 5 〉	통과 (0.01ms, 3.95MB)
테스트 6 〉	통과 (0.01ms, 3.97MB)
테스트 7 〉	통과 (0.01ms, 3.95MB)
테스트 8 〉	통과 (0.01ms, 3.91MB)
테스트 9 〉	통과 (0.01ms, 3.95MB)
테스트 10 〉	통과 (0.01ms, 3.95MB)
테스트 11 〉	통과 (0.01ms, 3.93MB)
테스트 12 〉	통과 (0.01ms, 3.89MB)
테스트 13 〉	통과 (0.01ms, 3.94MB)
테스트 14 〉	통과 (0.01ms, 3.95MB)
테스트 15 〉	통과 (0.01ms, 3.96MB)
테스트 16 〉	통과 (0.01ms, 3.77MB)
테스트 17 〉	통과 (0.01ms, 3.96MB)
테스트 18 〉	통과 (0.01ms, 3.96MB)
테스트 19 〉	통과 (0.01ms, 3.95MB)
테스트 20 〉	통과 (0.01ms, 3.89MB)
테스트 21 〉	통과 (0.01ms, 3.89MB)
테스트 22 〉	통과 (0.01ms, 3.96MB)
테스트 23 〉	통과 (0.01ms, 3.96MB)
```

## 비교식을 간단히 해보자
```c++

```
[출처]()

