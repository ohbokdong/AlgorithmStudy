# [가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746)

## 1차 시도

* 배열로 순회하며 탐색을 할 경우, numbers의 길이가 너무 길어질 경우 효율이 낮아짐
  * **좀 더 큰 수가 발견될 경우 연결 리스트처럼 앞에 원소를 추가하는 식으로 처리하도록 구현이 필요**
* numbers 배열 중 앞 자리가 가장 큰 수를 탐색, 순서대로 붙이도록 시도
  * 그러나 3, 30의 경우 "303"이 아닌 "330"이 더 큰 수가 될 때가 고려가 안됨

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
    return answer;
}
```

## 2차 시도
    