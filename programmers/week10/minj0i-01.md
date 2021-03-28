# [H-Index](https://programmers.co.kr/learn/courses/30/lessons/42747)

```javascript
function solution(citations) {
	var answer = [];

	// 어차피 최대는 citations.length
	const maxNum = citations.length;

	// 최대에서 하나씩 뺴줌
	for (let i = maxNum; i >= 0; i--) {
		// hLength: i 이상 인용된 편 수
		const hLength = citations.filter((v) => v >= i).length;
		// i번 이상 인용된 편 수가 i번보다 크다면
		if (hLength >= i) answer.push(i);
	}

	answer = Math.max(...answer);

	return answer;
}
```

흠 그냥 최대값을 구하고 바로 return 해주려면?

```javascript
function solution(citations) {
	var answer = 0;

	// 어차피 최대는 citations.length
	const maxNum = citations.length;

	// 최대에서 하나씩 뺴줌
	for (let i = maxNum; i >= 0; i--) {
		// hLength: i 이상 인용된 편 수
		const hLength = citations.filter((v) => v >= i).length;
		// i번 이상 인용된 편 수가 i번보다 크다면
		if (hLength >= i) {
			answer = i;
			break;
		}
	}

	return answer;
}
```
