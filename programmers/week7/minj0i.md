# [체육복](https://programmers.co.kr/learn/courses/30/lessons/42862)

```JAVASCRIPT
function solution(n, lost, reserve) {
    var answer = 0;
    
    const realLost = lost.filter(a => !reserve.includes(a))
    const realReserve = reserve.filter(a => !lost.includes(a))
    
    answer = n
    
     realLost.forEach(item => {
        let copyRealReserve = realReserve
        for (const a of copyRealReserve) {
            
        }
        realReserve.forEach(value => {
            if (Math.abs(value - item) <= 1) {
                realLost.splice(realLost.indexOf(item),1)
                realReserve.splice(realReserve.indexOf(value),1)
            }
        })
    })
    
    answer = n - realLost.length
    return answer;
}
```
