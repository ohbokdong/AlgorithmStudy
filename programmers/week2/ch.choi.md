# [2차] 실패율
```c++
struct stage
{
	int n;
	float value;
};
bool compare(stage a, stage b)
{
	if (a.value > b.value)
	{
		return true;
	}
    // 같을때에 대한 처리가 없어서 실패하는 케이스가 생겼음.
    // 낮은 순으로 처리를 하게 해서 문제가 없을줄 알았으나 정렬에서 문제였음.
	else if (a.value == b.value)
	{
		if (a.n < b.n)
		{
			return true;
		}
		else
		{
			return false;
		}
	}
	else
	{
		return false;
	}
}

vector<int> solution(int N, vector<int> stages) {
	vector<int> answer;

	int arrylenght = N + 1;
	int* arr = new int[arrylenght] {0, };

	// 스테이지별 사람 조사
	for (int i = 0; i < stages.size(); i++)
	{
		int index = stages.at(i);
		if (index > N)
		{
			continue;
		}
		arr[index] = arr[index]+1;
	}

	float user = ((float)stages.size());
	vector<stage> s;

	//N 스페이지 별로 실패율 계산
	for (int i = 1; i < arrylenght; i++)
	{
		stage scoreset;
		scoreset.n = i;
		if (arr[i] == 0)
		{
			scoreset.value = 0;
		}
		else
		{
			scoreset.value = arr[i] / user;
			user = user - arr[i];
		}
		s.push_back(scoreset);
	}
	// 실패율에 따른 정렬
	sort(s.begin(), s.end(), compare);

	for (int i = 0; i < s.size(); i++)
	{
		answer.push_back(s.at(i).n);
	}

	return answer;
}
```
## 수행시간 
![시간](https://github.com/ohbokdong/AlgorithmStudy/blob/main/programmers/week2/image/failurelate_Result.PNG) 

## 비교식을 간단히 해보자
내껀 너무 긴데... 생각좀 합시다.
```c++
bool cmp(const pair<double,int>&a, const pair<double,int>&b){
    if(a.first==b.first) return a.second<b.second;
    return a.first>b.first;
}
```
[출처](https://programmers.co.kr/learn/courses/30/lessons/42889/solution_groups?language=cpp) : 프로그래머스 왕예린 , 최우석 님 풀이

