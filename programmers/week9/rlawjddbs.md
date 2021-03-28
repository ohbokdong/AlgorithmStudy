# [프린터](https://programmers.co.kr/learn/courses/30/lessons/42587)
- [구글링](https://bubobubo003.tistory.com/66)으로 참고하여 작성함
```JS
function solution(priorities, location) {
    var answer = 0;
    var queue = [];

    priorities.forEach(function(v, i) {
        queue.push({priority: v, isMine: i == location});
    });
    
    while(true) {
        var firstEle = queue.splice(0, 1)[0]; // 인쇄 목록 첫 번째 추출
        if (queue.some(ele => ele.priority > firstEle.priority)) { // 추출한 첫 번째 보다 우선순위가 높은게 있다면
            queue.push(firstEle);
        } else { // 우선순위 높은 요소가 없다면
            answer++; // 대기 순서 +1
            if (firstEle.isMine) { // 내 인쇄 요청이 맞다면
                return answer;
            }
        }
    }
}
```