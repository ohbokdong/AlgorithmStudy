# [피보나치 수](https://programmers.co.kr/learn/courses/30/lessons/12945)

* 1차 시도, 시간초과...

```javascript
function solution(n) {
    var answer = 0;
    
    var getFibonacci = function(n) {
        if (n == 0) {
            return 0;
        } else if (n == 1) {
            return 1;
        } else {
            return getFibonacci(n-1) + getFibonacci(n-2);
        }    
    }
    
    answer = getFibonacci(n) % 1234567;
    return answer;
}
```

* 테스트케이스 7번부터 다 실패한게 표현할 수 있는 수의 범위를 초과했기 때문이라고 함
  * [7번부터 오답이 나오는 이유에 대한 설명(스포일러 주의)](https://programmers.co.kr/questions/11991)
  * DP 이용해서 풀었지만 테스트 케이스 13,14에서 런타임 에러 발생 -> 콜스택 오버플로우인듯

```js
function solution(n) {
    var answer = 0;
    
    const cache = {
        0: 0,
        1: 1
    };
    
    var getFibonacci = function(n) {
        if (cache[n] != undefined) return cache[n];

        cache[n] = (getFibonacci(n-1) + getFibonacci(n-2)) % 1234567;
        
        return cache[n];
    }
    
    answer = getFibonacci(n) % 1234567;
    return answer;
}
```

* 콜스택이 초과하지 않도록 재귀말고 반복문 사용하는 방식으로 풂..
  * [[프로그래머스] 만만할 줄 알았던 피보나치 수 JS](https://jireh.tistory.com/14)

```js
function solution(n) {
    var answer = [0, 1];
    if (n <= 1) return answer[n];
    
    for(var i=2; i<n+1; i++) {
        answer.push((answer[i-2] + answer[i-1]) % 1234567);
    }
    
    return answer[n];
}
```