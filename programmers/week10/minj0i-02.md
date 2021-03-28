# [스킬트리](https://programmers.co.kr/learn/courses/30/lessons/49993#)

```js
function solution(skill, skill_trees) {
	var answer = 0;
	const skillArr = [...skill];

	// skill_trees의 값 중 skill로만 filter하여 skillTreeFilter를 생성
	let skillTreeFilter = skill_trees.map((value) => {
		// string에 .split("")을 하면 array형태로 변경
		return value.split("").filter((char) => skillArr.indexOf(char) > -1);
	});

	for (let i = 0; i < skillTreeFilter.length; i++) {
		// 만약 skillTreeFilter했는데 아무것도 없다 = skill에 해당하는 것이 없다 이므로 answer 증가
		if (skillTreeFilter[i].length === 0) {
			answer++;
		} else {
			// skill을 skillTreeFilter[i]의 길이만큼 자르고 문자열로 만들어 비교
			const skillLength = skill.slice(0, skillTreeFilter[i].length);
			// array를 join("")를 하면 string형태로 변경
			if (skillLength === skillTreeFilter[i].join("")) answer++;
		}
	}

	return answer;
}
```

- 배열 안의 값을 비교하는 뭔가 ES6스러운게 뭐가 없을까 생각해봤는데..흐음....역시 for문이 최고인가?
