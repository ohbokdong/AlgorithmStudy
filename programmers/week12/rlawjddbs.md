# [124 나라의 숫자](https://programmers.co.kr/learn/courses/30/lessons/12899#)
- js 풀이 참고해서 풀었음
```JS
function solution(n) {
    var answer = "";
    
    while(n > 0) {
        answer = "412"[n % 3] + answer; // 뒤의 숫자부터 계산
        
        if (n % 3 == 0) {
            n = Math.floor(n / 3) - 1; // 3으로 나누어 떨어지면 1을 뺌
        } else {
            n = Math.floor(n / 3);
        }
    }
    return answer;
}
```