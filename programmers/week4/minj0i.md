# [두 개 뽑아서 더하기](https://programmers.co.kr/learn/courses/30/lessons/68644)

```JAVASCRIPT
function solution(numbers) {
  var answer = [];

    // i는 0부터 배열 하나 앞까지
    for (var i = 0; i < numbers.length - 1; i++) {
      // j는 i+1부터 배열 끝까지
      for (var j = i+1; j < numbers.length; j++) {
        // 합
        var hap = numbers[i] + numbers[j]
        //indexOf로 중복값 비교 후 넣음 
        if (answer.indexOf(hap) === -1) {
          answer.push(hap)
        }
      }
    }

    // 막판에 코드줄이기
    return answer.sort((a, b) => a - b);
}
```

- set쓰면 중복값 안되는 건 알았는데 어떻게 쓰는지를 몰라서 못씀ㅎㅎㅎㅎㅎ
- `const answer = [...new Set(temp)]` 인상깊었음
- 역시나 재귀함수 손에 안익고 누군가 썼나 하고 보는데 없네