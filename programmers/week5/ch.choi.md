# [5차] 쿼드 압축 후 개수 세기
```c++
#include <string>
#include <vector>

using namespace std;

void CheckCompress(int x, int y, int length, vector<vector<int>>& arr, vector<int>& ans)
{
    
   
    bool compress = false;
    int first = arr[y][x];
    for (size_t yy = y; yy < y + length; yy++)
    {
        for (size_t xx = x; xx < x + length; xx++)
        {
            if (first != arr[yy][xx])
            {
                compress = true;
                break;
            }
        }
        if (compress)
        {
            break;
        }
    } 
    if (!compress)
    {
        ans[first]++;
        return;
    }

    int halflen = length / 2;
    int xadd = x + halflen;
    int yadd = y + halflen;
    
    if(halflen == 0 )
    {   
        return;
    }
    
    //왼위
    CheckCompress(x, y, halflen, arr, ans);
    //오위
    CheckCompress(xadd, y, halflen, arr, ans);
    //왼아
    CheckCompress(x, yadd, halflen, arr, ans);
    //오아
    CheckCompress(xadd, yadd, halflen, arr, ans);
}

vector<int> solution(vector<vector<int>> arr) {
    vector<int> answer(2);
     CheckCompress(0, 0, arr.size(), arr, answer);
    return answer;
}
```
## 수행시간 

```c++

테스트 1 〉	통과 (0.03ms, 3.94MB)
테스트 2 〉	통과 (0.03ms, 3.9MB)
테스트 3 〉	통과 (0.02ms, 3.97MB)
테스트 4 〉	통과 (0.03ms, 3.79MB)
테스트 5 〉	통과 (2.70ms, 13.6MB)
테스트 6 〉	통과 (1.26ms, 13.5MB)
테스트 7 〉	통과 (0.93ms, 13.6MB)
테스트 8 〉	통과 (0.79ms, 13.5MB)
테스트 9 〉	통과 (0.74ms, 13.5MB)
테스트 10 〉	통과 (2.84ms, 40.3MB)
테스트 11 〉	통과 (0.01ms, 3.96MB)
테스트 12 〉	통과 (0.01ms, 3.97MB)
테스트 13 〉	통과 (0.80ms, 13.6MB)
테스트 14 〉	통과 (3.49ms, 40.3MB)
테스트 15 〉	통과 (3.64ms, 40.4MB)
테스트 16 〉	통과 (1.05ms, 13.4MB)
```

## 무제
```c++

```
[-]()


[출처]()

