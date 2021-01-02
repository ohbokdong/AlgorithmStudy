# [기능개발](https://programmers.co.kr/learn/courses/30/lessons/42586)

```JAVASCRIPT
function solution(progresses, speeds) {
    var answer = [];
    const lastday = progresses.map((progress, index) => Math.ceil((100 - progress)/speeds[index]))
    
    let maxDay = lastday[0]
    let i = 0;
    let j = 0;
    answer[0] = 0;
    
    for (i; i<lastday.length; i++) {
        if(maxDay >= lastday[i]){
            answer[j]++;
        }
        if(maxDay < lastday[i]) {
            maxDay = lastday[i]
            j++
            answer[j] = 1;
        }
    }
    
    return answer;
}
```

- 원래는 count를 따로 만들어서 answer에다가 넣으려다가 최대값을 만들어서 비교 후 바로 넣는 것이 나을 것 같았다.
- 완전 비슷한 답이 바로 나왔다 신기...
- 변수명 내가 하고싶었던 건 leftDay인데 lastday로 했다.
- 초기화
  - answer[0] = 0;으로 초기화하거나,
  - answer[++j] = 1;로 초기화 할 수 있구나 (j++로 했다가 안되길래 나눠서 했는데.. 생각해보니 ++j하면 해결)
  - let i=0, j=0; 도 마찬가지. for 괄호 옆에도 변수 선언 가능하다
