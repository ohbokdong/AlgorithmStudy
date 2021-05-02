# [카펫](https://programmers.co.kr/learn/courses/30/lessons/42842)

```js
function solution(brown, yellow) {
    var answer = [];
    // 3x3(9) 이상, 
    // 8 <= brown <= 5000
    // 1 <= yellow <= 2000000
    // brown + yello = w * h
    // w >= h
    
    // yellow 격자 높이가 1부터 시작해 yellow 격자의 형태로 만들어지는 모든 경우의 수 확인
    let yellowW = yellow;
    let yellowH = 1;
    let bFound = false;

    while (true) {
        // yellow + 2한 결과가 카펫의 사이즈
        const carpetW = yellowW + 2;
        const carpetH = yellowH + 2;
        
        const yellowSize = yellowW * yellowH;
        const carpetSize = carpetW * carpetH;
        
        // carpet사이즈에서 yellow 사이즈를 빼서 brown의 개수를 구함
        const nBrownSize = carpetSize - yellowSize;
        
        // brown의 개수가 주어진 개수와 같으면 해당 carpet사이즈가 정답
        if (brown == nBrownSize) {
            answer.push(carpetW);
            answer.push(carpetH);
            break;
        } else {
            // brown의 개수가 다를 경우 yellow의 높이를 키우고, yellow 너비값 수정
            yellowH++;
            yellowW = yellow / yellowH;
        }
    }
    
    return answer;
}
```
