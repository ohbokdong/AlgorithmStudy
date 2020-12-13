# [1차] 다트게임 문제풀이
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
    
    // 비교해서 둘다 1이면 '#'아니면 ' '
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