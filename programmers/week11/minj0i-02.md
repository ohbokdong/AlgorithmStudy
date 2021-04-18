# [카펫](https://programmers.co.kr/learn/courses/30/lessons/42842)

이건 코드가 아니라 ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ수학적으로 품ㅋㅋㅋㅋㅋㅋㅋ

```js
function solution(brown, yellow) {
  var answer = [0, 0];
  var maxHeight = 0;

  // 임의로 height를 짧은 거로 하고 width를 긴거라고 함
  // 따라서 노란색의 높이 최소값(i)은 1이어야 함
  for (var i = 1; i < brown / 2; i++) {
    // 갈색의 너비 최대는 노란색 높이가 1일때부터 시작
    var brownMaxWidth = brown / 2 - i;
    // 노란색의 너비는 갈색의 너비보다 2만큼 작을 수밖에 없음
    var yellowMaxWidth = brownMaxWidth - 2;

    // 노란색너비 * 높이 = yellow면 답
    if (yellowMaxWidth * i === yellow) {
      // 갈색은 노란색보다 높이나 가로 둘 다 2씩 커야 하므로
      answer[0] = i + 2;
      answer[1] = yellow / i + 2;
    }
  }

  return answer;
}
```
