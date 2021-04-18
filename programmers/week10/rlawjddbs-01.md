# [H-Index](https://programmers.co.kr/learn/courses/30/lessons/42747)
- filter 함수에 집착한 결과물 
```JS
function solution(citations) {
    var answer = 0;
    
    let indexList = [];

    citations.forEach(function(val, i) {
        
        var cnt = i + 1;

        var flag = citations.filter(function(ele) {
            return ele >= cnt; // ele - 논문 인용된 수, cnt - 비교값
        }).length >= cnt; // 반환된 배열의 길이가 비교값보다 크다면 true, 작다면 false

        if (flag == true) {
            indexList.push(cnt);
        }

    });
    
    answer = indexList.sort(function(e1, e2) { return e2 - e1; })[0]; // 저장된 cnt 중 가장 큰 수를 꺼냄
    answer = answer == undefined || answer == null ? 0 : answer; // 테스트 케이스의 예외를 대비한 정의 검증
    
    return answer;
}
```