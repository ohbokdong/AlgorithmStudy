# [1차] 비밀지도 문제풀이
```c++
#include <vector>
#include <string>
#include <iostream>
using namespace std;


vector<int> GetSecurityCode(int n);
vector<string> solution(int n, vector<int> arr1, vector<int> arr2);

int main()
{
    //테스트 데이터 
    vector<int> a;
    vector<int> b;
    a.push_back(9);
    a.push_back(20);
    a.push_back(28);
    a.push_back(18);
    a.push_back(11);

    b.push_back(30);
    b.push_back(1);
    b.push_back(21);
    b.push_back(17);
    b.push_back(28);

    vector<string> answer = solution(5, a, b);

    //결과 출력
    for (size_t i = 0; i < answer.size(); i++)
    {
        cout << answer[i] << endl;
    }
    
	return 0;
}

vector<string> solution(int n, vector<int> arr1, vector<int> arr2) {
    vector<string> answer;
    
    vector<int> securitycodes = GetSecurityCode(n);

    for (size_t i = 0; i < n; i++)
    {
        string row;
        int ornum = arr1[i] | arr2[i]; //or 연산을 통한 합산
        
        for (int j = securitycodes.size()-1; j >= 0; j--)
        {
            //code 값보다 클 경우 #, 아닐 경우 공백
            if (ornum >= securitycodes[j])
            {
                ornum = ornum - securitycodes[j];
                row += "#";
            }
            else
            {
                row += " ";
            }
        }
        answer.push_back(row);
    }

    return answer;
}
// 자리수별 값 저장
vector<int> GetSecurityCode(int n)
{
    vector<int> securitycodes;

    int code = 1;
    securitycodes.push_back(code);
    for (size_t i = 1; i < n; i++)
    {
        code = code * 2;
        securitycodes.push_back(code);
    }
    return securitycodes;
}
```

## 비트연산?? 오호?? 한수배웁니다.
```c++

#include <string>
#include <vector>

using namespace std;

vector<string> solution(int n, vector<int> arr1, vector<int> arr2) {
    vector<string> answer;
    for(int i=0; i <n; i++){
        arr1[i] = arr1[i]|arr2[i];
        string ans = "";
        for(int j = 0; j<n; j++){
            if(arr1[i] % 2 == 0) ans = " " + ans; //나머지가 0이 아니면 값이 있는 것임
            else ans = "#" + ans;
            arr1[i] = arr1[i] >> 1; //다음 비트로 변경 오호... 
        }
        answer.push_back(ans);
    }
    return answer;
}

```
[출처](https://programmers.co.kr/learn/courses/30/lessons/17681/solution_groups?language=cpp&type=all) : 프로그래머스 ddudini , 김제형 , shtjrgus010 님 풀이

