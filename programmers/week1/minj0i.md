# [1차] 비밀지도 문제풀이
```JAVASCRIPT
function solution(n, arr1, arr2) {
    var answer = [];
    var biArr1 = [];
    var biArr2 = [];
    
    //자리 맞추기
    for(var data of arr1) {
        var bi = data.toString(2).padStart(n, '0');
        biArr1.push(bi)
    }
    
    for(var data2 of arr2) {
        var bi2 = data2.toString(2).padStart(n, '0');
        biArr2.push(bi2)
    }
    
    // 비교해서 둘 중 하나라도 1이면 '#'아니면 ' '
    for(var i=0; i<n; i++) {
        var answerText = '';
        for(var j=0; j<n; j++) {
            if(biArr1[i].charAt(j) == 1 || biArr2[i].charAt(j) == 1) {
                answerText = answerText + '#'
            } else {
                answerText = answerText + ' '
            }
        }
        answer.push(answerText)
    }
    
    return answer;
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
- 정규식활용
- repeat 함수 활용
