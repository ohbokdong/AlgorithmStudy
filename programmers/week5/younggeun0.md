# [월간 코드 챌린지 시즌1(쿼드압축 후 개수 세기)](https://programmers.co.kr/learn/courses/30/lessons/68936)

```javascript
var answer = [ 0, 0 ];
function compress(arr, n, x, y) {
    // base case, 전달받은 영역이 모두 0 또는 1로 이루어져 있을 때
    var bCompressed = true;
    var head = arr[y][x];
    for (var i=x; i<x+n; i++) {
        if (!bCompressed) break;
        for (var j=y; j<y+n; j++) {
            if (arr[j][i] != head) {
                bCompressed = false;
                break;
            }
        }
    }

    if (bCompressed) { // 압축됐을 때 head(0 or 1) cnt++
        answer[head]++;
        return;
    }

    // 분할정복 🔥🔥🔥
    n = n/2;

    // recursive call
    compress(arr, n, x, y); // upperLeft
    compress(arr, n, x+n, y); // uppetRight
    compress(arr, n, x, y+n); // lowLeft
    compress(arr, n, x+n, y+n); // lowRight
}

function solution(arr) {
    var n = arr[0].length; // n*n 2차원 정수 배열 

    // 1. 쿼드트리 방식으로 압축
    // - 압축을 모두 한 뒤에 압축결과에서 처음부터 1,0을 카운트 하지 않고 압축하는 과정에서 기저사례에서 1, 0 을 카운트
    compress(arr, n, 0, 0);
    
    // 최종적으로 남는 0과 1의 개수
    return answer;
}
```
