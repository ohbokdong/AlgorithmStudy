# [9차] 프린터
```c++
int solution(vector<int> priorities, int location) {
	int answer = 0;

	char len = static_cast<char>(priorities.size());
	char lastindex = 0;
	char serchindex = 0;

	int level = 9; // 최대 레벨을 먼저 priorities 에서 뽑고 진행 해도 좋을듯

	for (int i = level; i >=1 ; i--) // 우선순위 9~1
	{
		serchindex = lastindex; //진행 순서

		for (char j = 0; j < len; j++) // vector size 만큼 순회
		{
			if (priorities[serchindex] == i) //우선 순위가 같다면
			{
				lastindex = serchindex; //진행 순서 변경
				++answer; //진행 완료
				if (location == serchindex) 
				{//찾는 위치 확인
					goto finish;
				}
			}

			++serchindex; // 다음 진행 ㄱㄱ
			if (serchindex >= len) // 길이 초기화
			{
				serchindex = 0;
			}
		}
	}

	finish:
	
	return answer;
}
```
## 수행시간 

```c++

테스트 1 〉	통과 (0.01ms, 3.97MB)
테스트 2 〉	통과 (0.01ms, 3.95MB)
테스트 3 〉	통과 (0.01ms, 3.96MB)
테스트 4 〉	통과 (0.01ms, 3.96MB)
테스트 5 〉	통과 (0.01ms, 3.78MB)
테스트 6 〉	통과 (0.01ms, 3.96MB)
테스트 7 〉	통과 (0.01ms, 3.94MB)
테스트 8 〉	통과 (0.01ms, 3.93MB)
테스트 9 〉	통과 (0.01ms, 3.88MB)
테스트 10 〉	통과 (0.01ms, 3.78MB)
테스트 11 〉	통과 (0.01ms, 3.96MB)
테스트 12 〉	통과 (0.01ms, 3.94MB)
테스트 13 〉	통과 (0.01ms, 3.97MB)
테스트 14 〉	통과 (0.01ms, 3.97MB)
테스트 15 〉	통과 (0.01ms, 3.92MB)
테스트 16 〉	통과 (0.02ms, 3.79MB)
테스트 17 〉	통과 (0.01ms, 3.96MB)
테스트 18 〉	통과 (0.01ms, 3.97MB)
테스트 19 〉	통과 (0.01ms, 3.96MB)
테스트 20 〉	통과 (0.01ms, 3.93MB)
```

## ?
```c++

```
