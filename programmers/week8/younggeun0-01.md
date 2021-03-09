# [가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746)

## 1차 시도

* 배열로 순회하며 탐색을 할 경우, numbers의 길이가 너무 길어질 경우 효율이 낮아짐
  * **좀 더 큰 수가 발견될 경우 연결 리스트처럼 앞에 원소를 추가하는 식으로 처리하도록 구현이 필요**
* numbers 배열 중 앞 자리가 가장 큰 수를 탐색, 순서대로 붙이도록 시도
  * 그러나 3, 30의 경우 "303"이 아닌 "330"이 더 큰 수가 될 때가 고려가 안됨
  * **망함**

```javascript
function solution(numbers) {
    var answer = '';
    
    // 최초 설계
    while (numbers.length > 0) {
        var highestFirst = -1;
        var highest = -1;
        var highestIdx = -1;
        
        // 1. numbers 중 앞자리가 가장 큰 수를 탐색
        for (var i=0; i<numbers.length; i++) {
            var number = numbers[i];
            
            // 1. numbers 중 앞자리가 가장 큰 수 탐색
            var first = parseInt(numbers[i].toString().substr(0,1));
            if (highestFirst < first) {
                highestFirst = first;
                highest = numbers[i];
                highestIdx = i;
            } else if (highestFirst == first) { // // 2. 앞 자리 수가 같다면 뒤의 수를 비교, 가장 큰 수를 탐색
                if (highest < numbers[i]) {
                    highest = numbers[i];
                    highestIdx = i;
                }
            } 
        }
        
        // 3. 하나의 문자열로 이어붙임
        answer += highest;
        numbers.splice(highestIdx, 1);
    }
    return answer; // 망함
}
```

## 2차 시도
    
* 정렬을 하되 한 자리 수로 내림차순 정렬, 두 자리 수 내림차순 정렬, 세 자리 수 내림차순 정렬.. 이렇게 자리수에 맞게 내림차순 정렬을 함
  * 정렬된 결과를 하나의 문자열로 이어붙임
  * 34, 3, 30이 있을 때 위 알고리즘대로 정렬하게되면 33430이 돼버림.. 34330이 더 크기 때문이 잘못된 알고리즘
  * **망함**

```javascript
function solution(numbers) {
    var answer = '';
    
    // numbers의 원소는 0~1000 이하이므로 1자리, 2자리, 3자리, 4자리의 원소가 존재
    // 2차원 배열, [자리수][내림차순으로 정렬된 배열]
    var arrSorted = new Array(4);
    for (var i=0; i<4; i++) {
        arrSorted[i] = [];
    }
    
    // 1. 자리 수 별로 내림차순 정렬
    for(var i=0; i<numbers.length; i++) {
        var number = numbers[i];
        var nNumLength = number.toString().length;
        var nIdx = 0;
        
        // 자리수에 인덱스 배열에 삽입정렬
        for (var j=0; j<arrSorted[nNumLength - 1].length; j++) {
            var sortedNum = arrSorted[nNumLength - 1][j];
            
            if (sortedNum > number) {
                nIdx = j;
                break;
            }
        }
        arrSorted[nNumLength - 1].splice(nIdx, 0, number);
    }
    
    
    // 2. 정렬된 결과를 하나의 문자열로 이어붙임
    for (var i=0; i<arrSorted.length; i++) {
        for (var j=0; j<arrSorted[i].length; j++) {
            answer += arrSorted[i][j];
        }
    }

    return answer; // 이번에도 망함
}
```

## 3차 시도 

