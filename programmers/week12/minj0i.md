# [124 나라의 숫자](https://programmers.co.kr/learn/courses/30/lessons/12899#)

```js
function solution(n) {
  var answer = "";

  // 나눠서 0으로 떨어지면 4고 그 외는 1, 2
  const modValue = [4, 1, 2];

  // 반복문으로 계속 돌린다
  while (n) {
    // 뒤에서부터 계산해서 앞으로 붙여나가야 하므로 앞에 modValue[n%3]을 더해준다
    // 처음에 answer + modValue[n % 3] 해줬더니 반대로 됨 ex)15->114
    answer = modValue[n % 3] + answer;
    // 3으로 나뉠 때 원래 몫보다 1이 적음 그 외는 그냥 나누기 몫
    n = n % 3 === 0 ? n / 3 - 1 : parseInt(n / 3);
  }
  return answer;
}
```
