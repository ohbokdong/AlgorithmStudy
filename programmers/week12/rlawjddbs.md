# [124 나라의 숫자](https://programmers.co.kr/learn/courses/30/lessons/12899#)
- js 풀이 참고해서 풀었음
```JS
function solution(n) {
    var answer = "";
    
    while(n) {
        answer = "412"[n % 3] + answer;
        
        if (n % 3 == 0) {
            n = Math.floor(n / 3) - 1;
        } else {
            n = Math.floor(n / 3);
        }
    }
    return answer;
}
```