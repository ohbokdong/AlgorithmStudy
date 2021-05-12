# [행렬의 곱셈](https://programmers.co.kr/learn/courses/30/lessons/12949)

```js
function solution(arr1, arr2) {
  const answer = [];

  arr1.map((arr1V, arr1I) => {
    const newArr2 = [];
    arr2.map((arr2V, arr2I) => {
      let sum = 0;
      arr1V.map((value, index) => {
        sum = sum + value * arr2[index][arr2I];
      });
      newArr2.push(sum);
    });
    answer.push(newArr2);
  });

  return answer;
}
```

위 코드로 하면 테스트케이스 두개는 다 통과하는데 제출하면 다 틀리고 0점이다 ㅎ.....
