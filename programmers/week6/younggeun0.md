# [N으로 표현](https://programmers.co.kr/learn/courses/30/lessons/42895)

## 1차 시도

* 무식하게 풀기
  * 완전  탐색 돌려 찾아봄, 메모이제이션 미적용

```javascript
// N - 사칙연산에 사용할 수
// number - 사칙연산 결과
function solution(N, number) {
    var answer = 0;
    var nLength = number.toString().length;
    // N으로 number를 만들 때 사용한 N의 최솟값을 return하는 함수
    var getNCnt = function(number, acc) {
        // 기저사례
        if (acc > 8) acc = -1;
        if (number < 0) return 10000;
        if (number == 0) return acc;
        
        acc++;
        
        // 완전탐색
        // number의 length를 넘지 않도록
        for (var i=0; i<nLength; i++) {
            var sub = N;
            
            if (i > 0) {
                sub = N + N + "";
                sub = parseInt(sub);
            }
            
            // 사칙연산, 부분문제로 변환
            var tmp1 = getNCnt(parseInt(number) - sub, acc);
            var tmp2;
            if (number % sub == 0) {
               tmp2 = getNCnt(parseInt(number) % sub, acc);
            } else { // N/N으로 -1, acc +2 처리
               tmp2 = getNCnt(parseInt(number) - 1, acc + 2); 
            }
            var tmp2 = 
            acc = Math.min(
                (tmp1 != -1) ? tmp1 : 10000,
                (tmp2 != -1) ? tmp2 : 10000);
        }
        return acc;
    };
    answer = getNCnt(number, 0);
    return answer;
}
```

![img1](https://github.com/ohbokdong/AlgorithmStudy/blob/main/programmers/week6/img/young1.png?raw=true)