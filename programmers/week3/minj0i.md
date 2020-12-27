# 예산

```JAVASCRIPT
function solution(d, budget) {
    var answer = 0;
    var sum = 0;
    
    d.sort(function(a, b) {
        return a-b;
    });
    
    for (const item of d) {
        if(budget >= sum+item) {
            sum = sum+item;
            answer++;
        }
    }
    return answer;
}
```