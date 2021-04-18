# [피보나치 수](https://programmers.co.kr/learn/courses/30/lessons/12945)

```js
// 당연히 될 거라고 생각했는데 왜 안될까
function solution(n) {
  var answer = 0;

  function fibo(n) {
    if (n === 0) return 0;
    if (n === 1) return 1;
    return fibo(n - 1) + fibo(n - 2);
  }

  answer = fibo(n) % 1234567;

  return answer;
}
```

함수로 만든게 문제인가...? 그냥 배열로 바꾸니 됨 흠...?

```js
function solution(n) {
  var answer = 0;
  let fibo = [];

  fibo[0] = 0;
  fibo[1] = 1;

  for (let i = 2; i < n + 1; i++) {
    fibo[i] = (fibo[i - 1] + fibo[i - 2]) % 1234567;
  }

  answer = fibo[n];
  return answer;
}
```
