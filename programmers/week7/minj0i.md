# [체육복](https://programmers.co.kr/learn/courses/30/lessons/42862)

```JAVASCRIPT
function solution(n, lost, reserve) {
    var answer = 0;
    
    let realLost = lost.filter(a => !reserve.includes(a))    
    let realReserve = reserve.filter(a => !lost.includes(a))
    
     realLost.forEach(item => {
        realReserve.forEach(value => {
            if (Math.abs(value - item) === 1) {
                realReserve = realReserve.filter(reserveItem => reserveItem !== value)
                realLost = realLost.filter(lostItem => lostItem !== item)
            }
        })
    })
    answer = n - realLost.length
    return answer;
}
```
