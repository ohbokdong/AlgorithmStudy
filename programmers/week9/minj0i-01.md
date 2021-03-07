# [가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746)

```JAVASCRIPT
function solution(numbers) {
    var answer = '';
    
    // 두개를 + 해도 문자처럼 합쳐질 수 있도록 먼저 문자열처럼 바꾸고
    // sort에서 (b+a) - (a+b) 형태로 해준다. (a-b, b-a 형태만 생각해서 자꾸 에러남)
    // forEach문으로 answer에다가 문자열로 더해준다
    let newArr = numbers.map(v => v+'').sort((a, b) => (b+a) - (a+b)).forEach(item => {
        answer = answer+item
    })
    
    // 테스트 마지막 케이스가 실패가 떠서
    // 만약에 [0, 0, 0]일때 "000"처럼 뜨는 것을 막아주기 위해 이런 경우 0만 나오도록 한다
    if ([...answer][0] === '0') answer = '0'

    return answer;
}
```
