# [124 나라의 숫자](https://programmers.co.kr/learn/courses/30/lessons/12899#)

```js
function solution(n) {
    var answer = '';
    var arrRest = [4, 1, 2];
    var position = [];

    // 124 나라 수로 변환
    var get124 = function(n) {                
        // 나머지가 0, 1, 2 인 경우 4, 1, 2에 대응됨
        var rest = n%3;
        position.push(arrRest[n%3]);
        
        // n이 3보다 작은 경우(1,2,3) 예외처리(없어도 채점 시 통과돼 당황..)
        if (n < 4) return;
        
        // 나머지가 0이 아닌경우, 나머지 제거한 수로 다시 재귀호출
        n = n - (rest != 0 ? rest : 3);

        var q = parseInt(n/3);
        // 몫이 3 초과인 경우, 다시 재귀적으로 124숫자로 변환
        if (q > 3) {    
            get124(q);
        } else {
            // 3 이하인 경우 몫으로 124 숫자 변환
            position.push(arrRest[q%3]);
        }
    };
    
    // 재귀호출하여 124 나라 수를 구함
    get124(n);

    // 역순으로 하나의 문자열로 만들기
    while (position.length > 0) {
        answer += position.pop();
    }
    return answer;
}
```
