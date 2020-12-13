# [1차] 다트게임 문제풀이
```JAVASCRIPT
ffunction solution(n, arr1, arr2) {
    var answer = [];
    // 1. arr1, arr2 정수로 들어옴 -> 이진수 배열로 변환
    var binaryArr1 = [];
    var binaryArr2 = [];
    for (var i=0; i<n; i++) {
        binaryArr1.push(getBinaryNum(arr1[i], n));
        binaryArr2.push(getBinaryNum(arr2[i], n));
    }
    // 2. binaryArr1, binaryArr2 지도 합치기
    // - 어느 하나라도 벽(1)인 부분은 벽, 모두 공백(0)인 부분은 공백
    console.log(binaryArr1);
    console.log(binaryArr2);
    
    var tmpVal = "";
    for (var i=0; i<n; i++) {
        for (var j=0; j<n; j++) {
            var map1 = binaryArr1[i].charAt(j);
            var map2 = binaryArr2[i].charAt(j);
            
            if (map1 == "0" && map2 == "0") {
                tmpVal = tmpVal + ' ';
            } else {
                tmpVal = tmpVal + '#';
            }
        }
        answer.push(tmpVal);
        tmpVal = "";
    }
    return answer;
}

function getBinaryNum(num, n) {
    var r = "";
    r = num.toString(2); // toString(2)를 이용하면 정수를 이진수로 변환가능
    
    // n 보다 짧으면 앞에 0을 붙여줌 
    if (r.length < n) {
        var nZero = n - r.length;
        for (var i=0; i<nZero; i++) {
            r = "0" + r;
        }
    }
    return r;
}
```

## 인상깊은 다른사람 풀이
```JAVASCRIPT
function solution(n, arr1, arr2) {
    return arr1.map((v, i) => addZero(n, (v | arr2[i]).toString(2)).replace(/1|0/g, a => +a ? '#' : ' '));
}

const addZero = (n, s) => {
    return '0'.repeat(n - s.length) + s;
}
```
- for문 대신 map사용
- if else 대신 삼항연산자
- 화살표연산자 활용