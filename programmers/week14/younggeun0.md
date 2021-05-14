# [행렬의 곱셈](https://programmers.co.kr/learn/courses/30/lessons/12949)

```js
function solution(arr1, arr2) {
    var answer = [];

    // arr1(m*n) arr2(n*l) 일 때 행렬의 곱셈 결과는 m*l
    var nRowNum = arr1.length; // m
    var nColNum = arr2[0].length; // l

    for (var nRowIdx=0; nRowIdx < nRowNum; nRowIdx++) {
        var row = arr1[nRowIdx];
        var newRow = [];
        var value = 0;

        // arr1의 열과 arr2 행은 같은 인덱스끼리 곱함(nColIdx1)
        for (var nColIdx2=0; nColIdx2 < nColNum; nColIdx2++) { // nColIdx2는 arr2의 idx
            for (var nColIdx1=0; nColIdx1 < row.length; nColIdx1++) {
                value += (row[nColIdx1] * arr2[nColIdx1][nColIdx2]);   
            }
            newRow.push(value);
            value = 0;
        }
        answer.push(newRow);
    }

    return answer;
}
```
