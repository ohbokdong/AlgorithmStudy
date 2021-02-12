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



## 2차 시도

* 메모이제이션 시도

1. 주어진 number를 만드는 최적해를 구해 캐싱
2. 나중에 원래 number를 만드는 cache의 합을 구해 답을 구하도록 설계


* 그러나.. acc가 재귀호출에 종속이 되는 문제 발생
  * 점화식을 조금 더 고민해야 할 듯

```js
// N - 사칙연산에 사용할 수
// number - 사칙연산 결과
function solution(N, number) {
    var answer = 10000;
    
    number = parseInt(number);
    
    // 메모이제이션 필요
    // 나머지 값(number)를 구하는 최소값을 찾아서 메모이제이션 하도록 함
    var cache = {};
    
    
    var nLength = number.toString().length;
    // N으로 number를 만들 때 사용한 N의 최솟값을 return하는 함수
    var getNCnt = function(number, acc) {
        // 기저사례
        if (number < 0) return 10000;   // 0보다 작은 경우 최소값 없는 경우(탐색 실패)
        if (number == 0) {
            if (acc > 8) acc = -1;      // acc가 8 초과 시 -1 리턴
            return acc;                 // number와 동일해지면 연산수 리턴
        }

        // 메모이제이션
        // TODO acc가 종속이 된 상황(수정필요)
        if (cache[number]) return cache[number];
        
        acc++;
        
        // 완전탐색, 나머지에 해당하는 최적해(N 사용횟수)를 찾음
        // number의 length를 넘지 않도록
        for (var i=0; i<nLength; i++) {
            var sub = N;
            
            if (i > 0) {
                sub = N + "" + N;
                sub = parseInt(sub);
            }
            
            // 사칙연산, 부분문제로 변환
            var tmp1 = getNCnt(number - sub, acc);
            var tmp2;
            if (number % sub == 0) {
               tmp2 = getNCnt(number % sub, acc);
            } else { // N/N으로 -1, acc +2 처리
               tmp2 = getNCnt(number - 1, acc + 2); 
            }

            acc = Math.min(tmp1, tmp2);
            
            if (!cache[number])
                cache[number] = acc;
            else
                cache[number] = Math.min(cache[number], acc);
            
        }
        return cache[number];
    };

    getNCnt(number, 0);
    
    // TODO 키 값의 합이 number가 되도록 만들어야 함
    for (var key in cache) {
        answer = Math.min(answer, cache[key]);
    }
    
    return answer;
}
```