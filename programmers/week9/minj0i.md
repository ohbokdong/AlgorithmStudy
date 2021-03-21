# [프린터](https://programmers.co.kr/learn/courses/30/lessons/42587)

```javascript
function solution(priorities, location) {
    var answer = 0;
    
    let saveLocation = location
    let check = true
    
    while (check) {
        // 맨 앞 비교
        let first = priorities.shift()
        // 중요도가 높은 문서가 한 개라도 존재하면
        if (priorities.filter(v => v > first).length > 0) {
            // 맨 뒤로 넣는다
            priorities.push(first)
        } else { // 뭔가 출력됐을 때 
            // 카운트++
            answer++
            // 만약 지정한곳이 빠진거라면 스탑
            if(saveLocation === 0) {
                check = false
            }
        }
        
        // 맨 앞이었다가 맨 뒤로 가게되면 length - 1 로(= 맨 뒤)
        if (saveLocation === 0) {
            saveLocation = priorities.length - 1
        } else {
            // 아니라면 한 칸 당겨지니 - 1
            saveLocation--
        }
    }
    return answer;
}
```

MDN이 짱이다
- [Array.prototype.shift()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)

`shift()` 메서드는 배열에서 첫 번째 요소를 제거하고, 제거된 요소를 반환합니다. 이 메서드는 배열의 길이를 변하게 합니다