# [체육복](https://programmers.co.kr/learn/courses/30/lessons/42862)

```JAVASCRIPT
function solution(n, lost, reserve) {
    var answer = 0;
    
    // 진짜로 잃어버린 사람
    let realLost = lost.filter(a => !reserve.includes(a))
    // 진짜로 줄 수 있는 사람
    let realReserve = reserve.filter(a => !lost.includes(a))
    
    // 잃어버린 사람을 돌면서
    realLost.forEach(item => {
        // 줄 수 있는 사람을 돌면서
        realReserve.forEach(value => {
            // 절대값이 1이 차이나는 경우 (대여 성공)
            if (Math.abs(value - item) === 1) {
                // 주는 사람에서 빼주고 (이제 못주니까)
                realReserve = realReserve.filter(reserveItem => reserveItem !== value)
                // 잃어버린 사람에서도 빼주고 (이제 체육복 생겼으니까)
                realLost = realLost.filter(lostItem => lostItem !== item)
            }
        })
    })
    // 전체 수 - 진짜로잃어버린 사람의 수
    answer = n - realLost.length
    return answer;
}
```
