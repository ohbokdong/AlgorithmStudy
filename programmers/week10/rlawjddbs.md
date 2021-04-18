# [H-Index](https://programmers.co.kr/learn/courses/30/lessons/42747)
```JS
function solution(citations) {
    var answer = 0;
    
    let indexList = [];

    citations.forEach(function(val, i) {
        
        var cnt = i + 1;

        var flag = citations.filter(function(ele) {
            return ele >= cnt;
        }).length >= cnt;

        if (flag) {
            indexList.push(cnt);
        }

    });
    
    answer = indexList.sort(function(e1, e2) { return e2 - e1; })[0];
    answer = answer == undefined || answer == null ? 0 : answer;
    
    return answer;
}
```