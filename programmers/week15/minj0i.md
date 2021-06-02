# [키패드 누르기](https://programmers.co.kr/learn/courses/30/lessons/67256)

```js
function solution(numbers, hand) {
  var answer = "";

  const lArray = [1, 4, 7];
  const rArray = [3, 6, 9];
  let lastLeft = 10;
  let lastRight = 12;

  function middle(num) {
    let dist = [[], [], [], [], []];
    if (num === 2) {
      dist = [[2], [1, 3, 5], [4, 6, 8], [7, 9, 11], [10, 12]];
    }
    if (num === 5) {
      dist = [[5], [2, 4, 6, 8], [1, 3, 7, 9, 11], [10, 12], []];
    }
    if (num === 8) {
      dist = [[8], [5, 7, 9, 11], [2, 4, 6, 10, 12], [1, 3], []];
    }
    if (num === 11) {
      dist = [[11], [8, 10, 12], [5, 7, 9], [2, 4, 6], [1, 3]];
    }
    // left와 right 위치에서 얼마나 더 움직여야 하는 지 index로 찾는다.
    const moveLeft = dist
      .map((value) => value.indexOf(lastLeft))
      .findIndex((value) => value > -1);
    const moveRight = dist
      .map((value) => value.indexOf(lastRight))
      .findIndex((value) => value > -1);

    if (moveLeft > moveRight) {
      lastRight = num;
      return "R";
    } else if (moveLeft < moveRight) {
      lastLeft = num;
      return "L";
    } else {
      if (hand === "left") {
        lastLeft = num;
        return "L";
      } else {
        lastRight = num;
        return "R";
      }
    }
  }

  for (let num of numbers) {
    // 0에 위치 시 11로 바꿔줌
    if (num === 0) num = 11;

    if (lArray.indexOf(num) > -1) {
      lastLeft = num;
      answer += "L";
    } else if (rArray.indexOf(num) > -1) {
      lastRight = num;
      answer += "R";
    } else {
      // 2,5,8,0(=11) 일 때
      answer += middle(num);
    }
  }

  return answer;
}
```
