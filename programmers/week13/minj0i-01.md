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

```js
function solution(arr1, arr2) {
  // 초기화
  const answer = Array(arr1.length).fill(Array(arr2[0].length).fill(0));
  const arr1Length = arr1[0].length;
  const arr2Length = arr2[0].length;

  for (var i = 0; i < arr1.length; i++) {
    for (var j = 0; j < arr2Length; j++) {
      for (var k = 0; k < arr1Length; k++) {
        answer[i][j] = answer[i][j] + arr1[i][k] * arr2[k][j];
      }
    }
  }
  return answer;
}
```

왜 안될까?

```js
function solution(arr1, arr2) {
  const answer = [];
  const arr1Length = arr1[0].length;
  const arr2Length = arr2[0].length;

  for (var i = 0; i < arr1.length; i++) {
    if (answer[i] == null) answer[i] = [];
    for (var j = 0; j < arr2Length; j++) {
      if (answer[i][j] == undefined) answer[i][j] = 0;
      for (var k = 0; k < arr1Length; k++) {
        answer[i][j] = answer[i][j] + arr1[i][k] * arr2[k][j];
      }
    }
  }
  return answer;
}
```

fill()로 넣으니 뭔가 계속 복사되는 것 같아서 그냥 무식하게 풀었다

- [MDN - fill()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)
